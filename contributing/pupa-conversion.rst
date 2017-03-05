Converting Scrapers to pupa
===========================

.. note::

    This document is very much a work-in-progress, feel free to `suggest contributions <http://github.com/openstates/documentation>`_.

As of early 2017 we've embarked on a process to switch away from our legacy backend (billy) that has been in use since 2009, to a more modern backend based on the `Open Civic Data <https://github.com/opencivicdata>`_ specification and tools (pupa).

This task will require updates to all of our scrapers, and during the interim period will also require some conversion scripts to exist to sync data between the two systems.  One of the best ways to help us right now is to follow this guide and help us start converting a state.


Before You Start
----------------

If you haven't already, you may want to check out our :doc:`getting-started` guide before starting converting scrapers.

Before you start it is important to check the current status of the pupa-migration for the state you're considering migrating.

The master ticket tracking this is `#1442 <https://github.com/openstates/openstates/issues/1442>`_, and contains a table on the current status of each state.  Before beginning work, check to make sure that there isn't already work in progress.  If there is consider picking another state or seeing how you can help out with the existing work.

If you are going to start work on a state it is a good idea to comment on that ticket and once you have branch with real work on it to open a WIP PR against the repository so others can follow along.


Converting Metadata
-------------------

For example purposes, let's look at what it takes to convert North Carolina.

Each state has static metadata found in the ``openstates/{{state}}/__init__.py`` file.  The first step will be in converting this metadata from the billy format to the new pupa format.

For the period during the conversion, we're going to need to make sure we keep the billy-specific metadata around.

1) Start by moving the existing metadata to our new ``billy_metadata/`` directory::

    $ git mv openstates/nc/__init__.py billy_metadata/nc.py

2) Edit ``billy_metadata/nc.py``

    All we need to leave is the ``metadata`` dictionary and any imports it requires.

    It is especially important to remove the scraper imports that look like::

        from .bills import NCBillScraper
        from .legislators import NCLegislatorScraper
        from .committees import NCCommitteeScraper
        from .votes import NCVoteScraper

    As those will no longer be available for import.  You temporarily may want to leave ``session_list`` around for now as we'll be using it in the next step.

    **example diff:** `NC billy_metadata <https://github.com/openstates/openstates/commit/29b7bb41405ad5001d783e5d9a5c9cd81fd06fcf?w=1>`_

3) Create a new ``openstates/nc/__init__.py``:

    There's a script to help with this, it isn't guaranteed to work perfectly but should at least provide a start::

        $ ./scripts/convert_metadata.py nc > openstates/nc/__init__.py

    You may need to tweak this some, you'll at least need to set the ``url`` property to a valid URL representing the state's government.

    **example diff:** `updated NC metadata <https://github.com/openstates/openstates/commit/3adba1ebe903fc448260b6a75133d6799a5eb27d>`_


At this point you should have a fairly complete jurisdiction defined.  Next we'll move on to converting the ``legislators.py`` file.

Converting Legislators
-----------------------

We won't truly be able to test our metadata until we write a scraper, so let's proceed for now.

In a billy scraper, we have a ``legislators.py`` file that contains a scraper
that derives from ``billy.scrape.legislators.Legislator`` and saves ``Legislator``
objects.

The convention in pupa is to call this a ``people`` scraper, and as you'll see, all pupa scrapers ``yield`` objects back where a billy scraper would call ``save_legislator``.

(It might be a good idea to look over the docs for `billy legislator scrapers <https://billy.readthedocs.io/en/latest/scrapers.html#legislators>`_
and `pupa person scrapers <https://opencivicdata.readthedocs.io/en/latest/scrape/people.html>`_.)

Steps:

1) Rename the file and change the imports:

    First let's rename the file::

        $ git mv openstates/nc/legislators.py openstates/nc/people.py

    Then we can look at the imports, you'll see something like::

        from billy.scrape.legislators import LegislatorScraper, Legislator

    We instead want::

        from pupa.scrape import Person, Scraper

    (pupa doesn't have different ``Scraper`` subclasses.)

    And you'll want to be sure to update the ``LegislatorScraper`` subclass to
    just subclass ``Scraper``.

2) Update the ``scrape()`` method's signature:

    In old scrapers the ``scrape`` method signature was ``scrape(term, chambers)`` and serves as an entrypoint.

    pupa scrapers also use a ``scrape`` method as an entrypoint, but the parameters should all be optional.

    Because most legislator scrapers only scrape the latest term, we'll drop the ``term`` argument, and we'll make the ``chambers`` argument into an
    optional ``chamber`` argument.  If no arguments are supplied the scraper should scrape all current legislators.

    In our case the NC scraper already had a ``scrape_chamber`` method, so we wind up updating our ``scrape`` method to dispatch like this::

        def scrape(self, chamber=None):
            if chamber:
                yield from self.scrape_chamber(chamber)
            else:
                yield from self.scrape_chamber('upper')
                yield from self.scrape_chamber('lower')

    The ``scrape`` method is required to ``yield`` objects, so since we're dispatching we have to use the `yield from <https://docs.python.org/3/whatsnew/3.3.html#pep-380-syntax-for-delegating-to-a-subgenerator>`_ construct that yields all objects from a subgenerator.

3) Update the portion of the code that creates/saves ``Legislator`` objects:

    The existing scrapers create ``Legislator`` objects and then call ``self.save_legislator``, we'll need to turn this into ``yield``-ing ``Person`` objects.

    It's important to note that this change can typically be pretty minimal, there's a lot of code in the scraper that'll be parsing the relevant data, but 95% of that code shouldn't need to be edited here.

    The main things that need to be changed:

        * ``chamber`` has become ``primary_org``
        * ``photo_url`` has become ``image``
        * ``term`` is no longer a parameter
        * offices used to be added via ``add_office(type, note, address, phone, email)`` and now individual contact details are added via ``add_contact_detail(type, value, note)``
        * instead of passing ``url`` to the constructor for a legislator's canonical URL, add any links with ``person.add_link``
        * it used to be possible to add arbitrary parameters to the ``Person`` constructor, these should now be added to the ``person.extras`` dictionary
        * instead of ``self.save_legislator(person)`` simply ``yield person`` (make sure that any function that yields is invoked with ``yield from`` from ``scrape``)


    Again, it might be a good idea to look over the docs for `billy legislator scrapers <https://billy.readthedocs.io/en/latest/scrapers.html#legislators>`_
    and `pupa person scrapers <https://opencivicdata.readthedocs.io/en/latest/scrape/people.html>`_.

    At this point, your person scraper should essentially be converted.

    **example diff:** `converted legislator scraper <https://github.com/openstates/openstates/commit/1f96aaaf5d7de49986c84b8d339c7e3f4ab4262e>`_

4) Revisiting the metadata:

    We now need to make one small change to the metadata to let pupa know about our person scraper.

    Let's import our new scraper at the top of ``openstates/nc/__init__.py``::

        from .people import NCPersonScraper

    And within we update the ``scrapers`` dictionary to look like::

        scrapers = {
            'people': NCPersonScraper,
        }

5) Running your first scraper:

    Now let's try giving it a run.

    Right now we're running ``pupa`` scrapers and then a second script that back-migrates the scraped data to a billy database.  This is a temporary step to enable us to transition the scrapers first and API, website, etc. once a significant number are done.  The easiest way to run this script is to use ``docker-compose`` like so::

    $ docker-compose run scrape nc

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

The ``people: {}`` line shows what it is trying to scrape, that it has found your Person scraper and is running without any arguments.

Then you'll see the line ``Not checking sessions...``, which we'll revisit in a second.

If all goes well, the scraper will run for a while, writing objects to the ``_data`` directory as it goes.  You'll see output like::

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

This shows the results of the scrape, the metadata and person objects that were successfully collected.

Once that is done you'll see billy take over for the conversion, ultimately ending in some lines like::

    15:43:34 INFO billy: billy-update abbr=nc
        actions=import,report
        types=bills,legislators,votes,committees,alldata
        sessions=2017
        terms=2017-2018
    15:43:35 INFO billy: Finished importing 170 legislator files.
    15:43:35 INFO billy: imported 0 vote files
    15:43:35 INFO billy: imported 0 bill files
    15:43:35 INFO billy: imported 0 committee files

The key line there is the 170 legislator files, matching the number of person objects reported by pupa.

Once you get to this point you have successfully converted a scraper to pupa!  Congratulations and thank you!

Now let's make sure your work gets integrated.


Creating Your Pull Request
--------------------------

Once you have some real work done it'd be best to go ahead and let us know so that we can avoid duplicating effort.

The preferred way to do this is to open a work-in-progress PR, naming your PR something like [WIP] convert $STATE to pupa.
(A helpful guide to making PRs with GitHub is here: https://help.github.com/articles/creating-a-pull-request/)

It'd also be a good time (if you hadn't already) to comment on `#1442 <https://github.com/openstates/openstates/issues/1442>`_ so that we can update it so that others beginning this process can be aware of your work and avoid duplicating it.

Someone from the team will review the changes and possibly ask if you can make some minor fixes, but no matter the state your work will be helpful.  If you'd like to continue, :doc:`pupa-conversion-2` has information on converting the remaining scrapers.
