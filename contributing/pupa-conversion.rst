Converting Scrapers to pupa
===========================

.. note::

    This document is very much a work-in-progress, feel free to `suggest contributions <http://github.com/openstates/documentation>`_.

As of early 2017 we've embarked on a process to switch away from our legacy backend (``billy``) that has been in use since 2009, to a more modern backend based on the `Open Civic Data <https://github.com/opencivicdata>`_ specification and tools.

This task will require updates to all of our scrapers, and during the interim period will also require some conversion scripts to exist to sync data between the two systems.  One of the best ways to help us right now is to follow this guide and help us start converting a state.


Before You Start
----------------

Before you start it is important to check the current status of the pupa-migration for the state you're considering migrating.

The master ticket tracking this is `#1442 <https://github.com/openstates/openstates/issues/1442>`_, and contains a table on the current status of each state.  Before beginning work, check to make sure that there isn't already work in progress.  If there is consider picking another state or seeing how you can help out with the existing work.

Beginning Work
--------------

For example purposes, let's look at what it takes to convert North Carolina.

Let's do our work on a ``pupa-nc`` branch::

    $ git checkout -b pupa-nc

Metadata
~~~~~~~~

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

    You can check out what the diff looks like: `NC billy_metadata diff <https://github.com/openstates/openstates/commit/29b7bb41405ad5001d783e5d9a5c9cd81fd06fcf?w=1>`_.

3) Create a new ``openstates/nc/__init__.py``:

    There's a script to help with this, it isn't guaranteed to work perfectly but should at least provide a start::

        $ ./scripts/convert_metadata.py nc > openstates/nc/__init__.py

    `example output of this script <https://github.com/openstates/openstates/commit/3adba1ebe903fc448260b6a75133d6799a5eb27d>`_

    You may need to tweak this some, you'll at least need to set the ``url`` property to a valid URL representing the state's government.


At this point you should have a fairly complete jurisdiction defined.  Next we'll move on to converting the ``legislators.py`` file.

Legislators
~~~~~~~~~~~

In a billy scraper, we have a ``legislators.py`` file that contains a scraper
that derives from ``billy.scrape.legislators.Legislator`` and saves ``Legislator``
objects.

The convention in pupa is to call this a ``people`` scraper, and as you'll see, all pupa scrapers ``yield`` objects back where a billy scraper would call ``save_legislator``.

Steps:

1) rename the file and change the imports:

    First let's rename the file::

        $ git mv openstates/nc/legislators.py openstates/nc/people.py

    Then we can look at the imports, you'll see something like::

        from billy.scrape.legislators import LegislatorScraper, Legislator

    We instead want::

        from pupa.scrape import Person, Scraper

    (pupa doesn't have different ``Scraper`` subclasses.)

    And you'll want to be sure to update the ``LegislatorScraper`` subclass to
    just subclass ``Scraper``.

2) Update the ``scrape()`` method's signature.

Here's an example commit that converted NC's legislator scraper

    https://github.com/openstates/openstates/commit/1f96aaaf5d7de49986c84b8d339c7e3f4ab4262e
