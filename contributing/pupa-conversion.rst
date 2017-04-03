Converting Scrapers to pupa
===========================

.. note::

    This document is a work-in-progress; if any part of it is unclear, `suggest changes or improvements <http://github.com/openstates/documentation>`_.

As of early 2017, we've embarked on a process to switch away from our legacy backend (billy) that has been in use since 2009, to a more modern backend (pupa) based on the `Open Civic Data <https://github.com/opencivicdata>`_ specification and tools. We've written in a few places why this change is important and worth our time:

* https://github.com/openstates/meta/wiki/2017-Roadmap#pupa-ization
* https://blog.openstates.org/post/whats-next-2017/

This task will require updates to every single one of our scrapers. Given that this is such a big task, and will enable many more converting scrapers from billy to pupa is one of the best ways to help out on Open States right now. Follow this guide and start converting a state!


Before You Start
----------------

If you haven't already, you should read our :doc:`getting-started` guide. It's important to know how scrapers are run, before starting to convert them.

It's also important to make sure that the state you've chosen to convert from billy to pupa hasn't already been converted! This is tracked in ticket `#1442 <https://github.com/openstates/openstates/issues/1442>`_. If your state of choice isn't available, consider picking another state, or fixing some `Open States bugs <https://github.com/openstates/openstates/issues>`_.


Begin
-----

Comment on the aforementioned `tracking ticket <https://github.com/openstates/openstates/issues/1442>`_ so that no one else accidentally works on this state as well, duplicating effort. And once you have a Git branch with some conversion work in it, open a WIP PR (Work-in-Progress Pull Request) against the Open States repository, so that others can follow along.


Converting Metadata
-------------------

For example purposes, let's look at what it takes to convert North Carolina. Going forward, you can replace ``nc`` with the abbreviation for your selected state.

Each state has static metadata found in ``openstates/{{state}}/__init__.py``. The first step will be to convert this metadata from the billy format to the new pupa format.

1) Start by moving the existing metadata to our new ``billy_metadata/`` directory; until all states are in Pupa format, we're going to need to keep the billy-specific metadata around::

    $ git mv openstates/nc/__init__.py billy_metadata/nc.py

2) Edit ``billy_metadata/nc.py``:

    Delete everything in the module besides the ``metadata`` dictionary and any imports it requires. You temporarily may want to leave ``session_list`` around as well, as we'll be using it in the next step.

    **Example diff:** `NC billy_metadata <https://github.com/openstates/openstates/commit/29b7bb41405ad5001d783e5d9a5c9cd81fd06fcf?w=1>`_

3) Create a new ``openstates/nc/__init__.py``:

    There's a script to help with this step. It isn't guaranteed to work perfectly, but should at least provide a good starting point::

        $ ./scripts/convert_metadata.py nc > openstates/nc/__init__.py

    Then, set the ``url`` property inside to a valid URL representing the state's government. You may need to modify the file further, such as indicating the number of seats in the upper and lower houses of your state.

    Delete the `session_list` function from `billy_metadata/nc.py` add it to the jurisdiction subclass in the newly created `openstates/nc/__init__.py` file, renamed to `get_session_list`.

    **Example diff:** `updated NC metadata <https://github.com/openstates/openstates/commit/3adba1ebe903fc448260b6a75133d6799a5eb27d>`_

At this point you should have a fairly complete OCD jurisdiction defined. Next, we'll move on to converting the legislator scraper.


Converting Legislators
-----------------------

We won't be able to test our pupa metadata file until we write a pupa scraper, so let's do that!

For a state in the billy framework, we have a ``legislators.py`` file that contains a scraper instantiated from ``billy.scrape.legislators.Legislator``. This scraper captures and saves ``Legislator`` objects.

In pupa, this scraper is called ``people`` scraper instead. (This is because OCD can easily model individuals who aren't members of a legislature.)

As you'll see, pupa scrapers ``yield`` scraped objects, whereas a billy scraper would call ``save_legislator``; ``yield`` and ``yield from`` expose Python 3's powerful `generator and subroutine <https://jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/>`_ capabilities.

Before diving in, it's helpful to look over the docs for `billy legislator scrapers <https://billy.readthedocs.io/en/latest/scrapers.html#legislators>`_
and `pupa person scrapers <https://opencivicdata.readthedocs.io/en/latest/scrape/people.html>`_.

1) Rename the scraper file::

    $ git mv openstates/nc/legislators.py openstates/nc/people.py

2) Update the ``import`` statement:

    At the top of the file, you'll see something like::

        from billy.scrape.legislators import LegislatorScraper, Legislator

    We instead want::

        from pupa.scrape import Person, Scraper

    (pupa doesn't have different ``Scraper`` subclasses.)

    Also, rename the file's instantiated scraper (so that it refers to ``Person`` rather than ``Legislator``), and make it a subclass of ``Scraper`` rather than ``LegislatorScraper``.

3) Update the ``scrape`` method's signature:

    In billy scrapers, the ``scrape`` method signature is ``scrape(term, chambers)``, and serves as an entrypoint for the scraper class.

    pupa scrapers also use a ``scrape`` method as an entrypoint, but the parameters are all optional.

    Because most legislator scrapers only scrape the current session, we'll drop the ``term`` argument, and the ``chambers`` argument can be made into an optional ``chamber`` argument.

    The NC scraper already had a ``scrape_chamber`` method that was invoked by the ``scrape`` method. So, we updated our ``scrape`` method to dispatch like this::

        def scrape(self, chamber=None):
            if chamber:
                yield from self.scrape_chamber(chamber)
            else:
                yield from self.scrape_chamber('upper')
                yield from self.scrape_chamber('lower')

    pupa ``scrape`` methods (which are generators) must ``yield`` objects. Since the NC scraper's ``scrape_chamber`` method (also a generator) will collect and ``yield`` the People objects initially, the ``scrape`` method must ``yield from`` that generator itself.

4) Update the portion of the code that creates and saves ``Legislator`` objects:

    The billy scrapers create ``Legislator`` objects, and then call ``self.save_legislator``. We'll need to turn ``self.save_legislator`` into a ``yield`` of ``Person`` objects.

    This change is typically minimal; there's a lot of code in billy legislator scrapers, but very little of it should need to be edited for the purposes of pupa.

    Instead of instantiating ``Legislator`` objects, instantiate ``Person`` objects instead. Unlike ``Legislator`` constructors in billy, ``Person`` constructors in pupa require all arguments to be named. And several properties need to be changed:

        * ``term`` is no longer a parameter
        * ``chamber`` has become ``primary_org``
        * ``photo_url`` has become ``image``
        * ``full_name`` has become ``name``
        * instead of ``url`` as a legislator's canonical URL, add any such links with the ``Person.add_link`` method
        * billy allowed arbitrary parameters on a ``Legislator`` object; in pupa, these should now be in a ``Person.extras`` dictionary

    Update the ``add_office`` method to ``add_contact_detail``::

        # old
        add_office(type, note, address, phone, email)

        # new
        add_contact_detail(
            type,  # One of the vCard RDF standards, see [this list](http://www.popoloproject.com/specs/contact-detail.html)
            value,
            note  # Eg, 'District Office', 'Capitol Office'
        )

    Instead of ``self.save_legislator(Legislator)`` from billy, simply ``yield person`` (make sure that any function that creates ``Person`` objectss outside of ``scrape`` is invoked by ``scrape`` using ``yield from``, as described above).

    Again, it might be a good idea to look over the docs for `billy legislator scrapers <https://billy.readthedocs.io/en/latest/scrapers.html#legislators>`_
    and `pupa person scrapers <https://opencivicdata.readthedocs.io/en/latest/scrape/people.html>`_.

    Since you're also switching from Python 2 (billy) to Python 3 (pupa), you may need to make syntax changes to the module. For instance, if ``Dict.iteritems()`` is used anywhere, it would have to be replaced by ``Dict.items()``.

    At this point, your person scraper should essentially be converted.

    **Example diff:** `converted legislator scraper <https://github.com/openstates/openstates/commit/1f96aaaf5d7de49986c84b8d339c7e3f4ab4262e>`_

4) Revisiting the metadata:

    We now need to make one small change to the metadata (ie, the ``__init__.py`` file) to let pupa know about our person scraper. Import our new scraper at the top of ``openstates/nc/__init__.py``::

        from .people import NCPersonScraper

    And within the Jurisdiction object, update the ``scrapers`` dictionary to look like::

        scrapers = {
            'people': NCPersonScraper,
        }

5) Running your first scraper:

    Now let's try giving it a run::

        $ docker-compose run scrape nc

    This runs pupa scrapers for the state. A second script is then executed, back-porting the scraped pupa data to billy format; since the API and website currently rely on the billy format, this is necessary during the transition off of billy.

You'll probably see output like::

    no pupa_settings on path, using defaults
    nc (scrape)
      people: {}
    Not checking sessions...
    15:35:05 INFO pupa: save jurisdiction North Carolina as jurisdiction_ocd-jurisdiction-country:us-state:nc-government.json
    15:35:05 INFO pupa: save organization North Carolina General Assembly as organization_6ecadcc4-0122-11e7-91f7-0242ac130003.json
    15:35:05 INFO pupa: save organization Senate as organization_6ecae228-0122-11e7-91f7-0242ac130003.json
    15:35:05 INFO pupa: save post 1 as post_6ecb36e2-0122-11e7-91f7-0242ac130003.json
    15:35:05 INFO pupa: save post 2 as post_6ecb3840-0122-11e7-91f7-0242ac130003.json
    15:35:05 INFO pupa: save post 3 as post_6ecb3976-0122-11e7-91f7-0242ac130003.json
    15:35:05 INFO pupa: save post 4 as post_6ecb3ab6-0122-11e7-91f7-0242ac130003.json

The ``people: {}`` line describes what type of data pupa is trying to scrape, that it has found your Person scraper, and that it is running without any arguments.

Next, you see the line ``Not checking sessions...``, which we'll revisit later.

If all goes well, the scraper will run for a while, writing JSON objects to the ``_data`` directory as it goes.

Finally, you'll see output like::

    nc (scrape)
      people: {}
    jurisdiction scrape:
      duration:  0:00:00.561228
      objects:
        jurisdiction: 1
        organization: 5
        post: 170
    people scrape:
      duration:  0:00:03.910275
      objects:
        membership: 340
        person: 170

This is the result of the scrape, including the metadata and person objects that were successfully collected.

Once that is done you'll see the to-billy conversion begin, ultimately ending in some lines like::

    15:43:34 INFO billy: billy-update abbr=nc
        actions=import,report
        types=bills,legislators,votes,committees,alldata
        sessions=2017
        terms=2017-2018
    15:43:35 INFO billy: Finished importing 170 legislator files.
    15:43:35 INFO billy: imported 0 vote files
    15:43:35 INFO billy: imported 0 bill files
    15:43:35 INFO billy: imported 0 committee files

The import part to check is the ``{{n}} legislator files``, which ought to match the number of person objects reported by pupa.

Once you get to this point, you have successfully converted a scraper to pupa!  Congratulations, and thank you! Let's make sure your hard work gets integrated.


Creating Your Pull Request
--------------------------

Once you have this work done, go ahead and let us know so that we can avoid duplicating effort.

The preferred way to do this is to open a work-in-progress PR, naming your PR something like ``[WIP] Convert {{state}} to pupa``. A helpful guide to making PRs with GitHub is here: https://help.github.com/articles/creating-a-pull-request/

Someone from the team will review the PR and possibly request that you make some minor fixes, but no matter the status your work will be helpful. If you'd like to continue on, :doc:`pupa-conversion-2` has information on converting the remaining types of scrapers.
