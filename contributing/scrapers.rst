.. _contributing-to-scrapers:

Contributing to Scrapers
========================

Scrapers are at the core of what Open States does, each state requires several custom scrapers designed to extract bills, legislators, committees, and votes from the state website.  All together there are around 200 scrapers, each one essentially independent, which means that there is always more work to do, but fortunately plenty of prior work to learn from.

Checking out
------------

Fork and clone the main scraper repository:

  * Visit https://github.com/openstates/openstates and click the 'Fork' button.
  * Clone your fork using your tool of choice or the command line::

        $ git clone git@github.com:yourname/openstates.git
        Cloning into 'openstates'...

  * And remember to :ref:`install pre-commit <pre-commit>`::

        $ pre-commit install
        pre-commit installed at .git/hooks/pre-commit

.. warning::  
  Before cloning on a Windows computer, you will need to disable line-ending conversion.  ``git config --global core.autocrlf false``  After cloning and entering the repo, you'll likely want to set global line-ending conversion back to `true`, and set local conversion to `false`.

Repository overview
-------------------

At this point you'll have a local ``openstates`` directory.  Within it, you'll find a directory called ``openstates`` lets take a look at it::

    $ ls openstates
    __init__.py dc          in          mn          nj          pr          va
    ak          de          ks          mo          nm          ri          vi
    al          fl          ky          ms          nv          sc          vt
    ar          ga          la          mt          ny          sd          wa
    az          hi          ma          nc          oh          tn          wi
    ca          ia          md          nd          ok          tx          wv
    co          id          me          ne          or          ut          wy
    ct          il          mi          nh          pa          utils

This directory is a python module with 50+ subpackages, one for each state.

Let's look inside one::

    $ ls openstates/nc
    __init__.py    bills.py       committees.py  people.py      votes.py

Some states' directories will differ a bit, but all will have ``__init__.py`` and ``bills.py``.

The ``__init__.py`` file for each state has basic metadata on the state including a list of sessions.

Other files contain the scrapers, typically named ``bills``, ``votes``, etc.

Running Your First Scraper
--------------------------

Let's run your state's bills scraper (substitute your state for 'nc' below) ::

    $ docker-compose run --rm scrape nc bills --fastmode --scrape

The parameters you pass after ``docker-compose run --rm scrape`` are passed to ``pupa update``.  Here we're saying that we're running NC's scrapers, and that we want to do it in "fast mode".  By default, ``pupa update`` imports results into a postgres database; the ``--scrape`` flag skips that step.

You'll see the *run plan*, which is what ``pupa`` aims to capture; in this case we're scraping the state website's data into JSON files::

    loaded Open States pupa settings...
    nc (scrape)
      bills: {}

Then legislative posts and organizations get created, which is mostly boilerplate::

    08:46:35 INFO pupa: save jurisdiction North Carolina as jurisdiction_ocd-jurisdiction-country:us-state:nc-government.json
    08:46:35 INFO pupa: save organization North Carolina General Assembly as organization_01d6327c-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO pupa: save organization Executive Office of the Governor as organization_01d63560-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO pupa: save organization Senate as organization_01d636e6-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO pupa: save post 1 as post_01d63a06-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO pupa: save post 2 as post_01d63b96-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO pupa: save post 3 as post_01d63cea-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO pupa: save post 4 as post_01d63e34-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO pupa: save post 5 as post_01d63f74-72d2-11e7-8df8-0242ac130003.json

And then the actual data scraping begins, defaulting to the most recent legislative session::

    08:46:36 INFO pupa: no session specified, using 2017
    08:46:36 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/SimpleBillInquiry/displaybills.pl?Session=2017&tab=Chamber&Chamber=Senate
    08:46:38 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/BillLookUp/BillLookUp.pl?Session=2017&BillID=S1
    08:46:39 INFO pupa: save bill SR 1 in 2017 as bill_03c7edb4-72d2-11e7-8df8-0242ac130003.json
    08:46:39 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/BillLookUp/BillLookUp.pl?Session=2017&BillID=S2
    08:46:39 INFO pupa: save bill SJR 2 in 2017 as bill_044a5fc4-72d2-11e7-8df8-0242ac130003.json
    08:46:39 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/BillLookUp/BillLookUp.pl?Session=2017&BillID=S3
    08:46:40 INFO pupa: save bill SB 3 in 2017 as bill_04e8c66e-72d2-11e7-8df8-0242ac130003.json
    08:46:40 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/BillLookUp/BillLookUp.pl?Session=2017&BillID=S4
    08:46:41 INFO pupa: save bill SB 4 in 2017 as bill_05781f08-72d2-11e7-8df8-0242ac130003.json
    08:46:41 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/BillLookUp/BillLookUp.pl?Session=2017&BillID=S5

Depending on the scraper you run, this part takes a while.  Some scrapers can take hours to run depending on the number of bills and speed of the state's website.

.. note::
    It is often desirable to bail out of running the whole scrape (Ctrl-C) after it has gotten a bit of data, instead of letting it run the entire scrape.

To review the data you just fetched, you can browse the _data/nc/ directory and inspect the JSON files.  If you're trying to make a small fix this is often sufficient, you can confirm that the scraped data looks correct and move on.

.. note::
    It is of course possible that the scrape fails.  If so, there's a good chance that isn't your fault, especially if it starts to run and then errors out.  Scrapers do break, and there's no guarantee North Carolina didn't change their legislator page yesterday, breaking our tutorial here.

    If that's the case and you think the issue is with the scraper, feel free to get in touch with us or `file an issue <https://github.com/openstates/openstates/issues>`_.

At this point you're ready to run scrapers and contribute fixes. Hop onto `our GitHub ticket queue <https://github.com/openstates/openstates/issues>`_, pick an issue to solve, and then submit a Pull Request!

Next Steps
----------

If you'd like to see how your scraped data imports into the database, perhaps to diagnose an issue that is happening after the scrape, continue to :ref:`getting a working database <working-database>` to see how to get a local database that you can import data into.

If you want to run imports, you can drop the ``--scrape`` portion of the command you've been running.  At that point you should see data being imported into your database.
