Converting Scrapers to pupa
===========================

.. note::

    This document is very much a work-in-progress, feel free to `suggest contributions <http://github.com/openstates/documentation>`_.

As of early 2017 we've embarked on a process to switch away from our legacy backend (``billy``) that has been in use since 2009, to a more modern backend based on the `Open Civic Data <https://github.com/opencivicdata>`_ specification and tools.

This task will require updates to all of our scrapers, and during the interim period will also require some conversion scripts to exist to sync data between the two systems.  One of the best ways to help us right now is to follow this guide and help us start converting a state.


Before You Start
----------------

Before you start it is important to check the current status of the pupa-migration for the state you're considering migrating.

The master ticket tracking this is `#1442 <https://github.com/openstates/openstates/issues/1442`_, and contains a table on the current status of each state.  Before beginning work, check to make sure that there isn't already work in progress.  If there is consider picking another state or seeing how you can help out with the existing work.

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

(TODO: link to pupa docs for Jurisdiction)

You'll start by defining a class for the Jurisdiction::

    # sessions dict:
    # key -> identifier
    # display_name -> name

    class NC(Jurisdiction):
        division_id = "ocd-division/country:us/state:nc"
        classification = "government"
        name = "North Carolina"
        url = "http://nc.gov"
        scrapers = {
        }
        parties = [
            {'name': 'Republican'},
            {'name': 'Democratic'}
        ]
        legislative_sessions = [
        '2009': {'start_date': datetime.date(2009, 1, 28), 'type': 'primary',
                 'name': '2009-2010 Session',
                 '_scraped_name': '2009-2010 Session',
                 },
        '2011': {'start_date': datetime.date(2011, 1, 26), 'type': 'primary',
                 'display_name': '2011-2012 Session',
                 '_scraped_name': '2011-2012 Session',
                 },
        '2013': {'start_date': datetime.date(2013, 1, 30), 'type': 'primary',
                 'display_name': '2013-2014 Session',
                 '_scraped_name': '2013-2014 Session',
                 },
        '2015': {'start_date': datetime.date(2015, 1, 30), 'type': 'primary',
                 'display_name': '2015-2016 Session',
                 '_scraped_name': '2015-2016 Session',
                 },
        '2015E1': {'type': 'special',
                   'display_name': '2016 Extra Session 1',
                   '_scraped_name': '2016 Extra Session 1',
                   },
        '2015E2': {'type': 'special',
                   'display_name': '2016 Extra Session 2',
                   '_scraped_name': '2016 Extra Session 2',
                   },
        '2015E3': {'type': 'special',
                   'display_name': '2016 Extra Session 3',
                   '_scraped_name': '2016 Extra Session 3',
                   },
        '2015E4': {'type': 'special',
                   'display_name': '2016 Extra Session 4',
                   '_scraped_name': '2016 Extra Session 4',
                   },
        '2015E5': {'type': 'special',
                   'display_name': '2016 Extra Session 5',
                   '_scraped_name': '2016 Extra Session 5',
                   },
        '2017': {'type': 'primary',
                 'display_name': '2017-2018 Session',
                 '_scraped_name': '2017-2018 Session',
                 },
        ]
        ignored_scraped_sessions = [
        ]

A lot of these values come from the old metadata
