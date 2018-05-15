Start Contributing to Open States
=================================

.. note::

    This document is very much a work-in-progress, feel free to `suggest contributions <http://github.com/openstates/documentation>`_.

Scrapers are at the core of what Open States does, each state requires several custom scrapers designed to extract bills, legislators, committees, votes, and events from the state website.  All together there are around 200 scrapers, each one essentially independent, which means that there is always more work to do, but plenty of prior work to learn from.

Code of Conduct
---------------

Open States is a project that can only exist because of the fantastic community that has worked on it over the years.
In the interest of keeping this community a healthy and welcoming place we have a :doc:`code-of-conduct` and encourage you to familiarize yourself with it.

Prerequisites
-------------

This guide assumes a basic familiarity with:
    - using the command line
    - git
    - Python

No worries if you aren't an expert though, we'll walk you through the steps.  And as for Python, if you've written other languages like Javascript or Ruby you'll probably be just fine.  Don't be afraid to :ref:`ask for help <getting-help>` either!

Getting Started
---------------

First thing you will need to do is get a working development environment on your local machine.  We'll do this using Docker.  No worries if you aren't familiar with Docker, you'll barely have to touch it.

**Step 1)** Install Docker (and docker-compose) if not already installed on your local system.

    * On OSX: `Docker for Mac <https://docs.docker.com/docker-for-mac/>`_ is perhaps the easiest way.
    * On Windows: `Docker for Windows <https://docs.docker.com/docker-for-windows/>`_
    * On Linux: Use your package manager of choice or `follow Docker's instructions <https://docs.docker.com/engine/installation/linux/>`_.
    * `Generic instructions from Docker <https://docs.docker.com/compose/install/>`_.

**Step 2)** Ensure that Docker (and docker-compose) are installed locally and check their versions::

    $ docker --version
    Docker version 17.03.0-ce, build 60ccb22
    $ docker-compose --version
    docker-compose version 1.11.2, build dfed245

Of course, your versions may be newer. The minimum required versions for Open States are:

    * 1.9.0 of Docker
    * 1.10.0 of Docker Compose

**Step 3)** We'll fork and clone the main `Open States scraper repository <https://github.com/openstates/openstates>`_.

  * Visit https://github.com/openstates/openstates and click the 'Fork' button.
  * Clone your fork using your tool of choice or the command line::

        $ git clone git@github.com:yourname/openstates.git
        Cloning into 'openstates'...

At this point you'll have a local ``openstates`` directory.  Let's go ahead and look at it::

    $ cd openstates
    $ ls
    AUTHORS.md          README.rst          openstates/         setup.cfg
    CODE_OF_CONDUCT.md  billy_metadata/     pupa-scrape.sh*     setup.py
    Dockerfile          billy_settings.py   pupa2billy/
    Dockerfile-alpine   docker-compose.yml  requirements.txt
    LICENSE             manual_data/        scripts/

There are a few top level text files, some docker files, which we'll come back to shortly, and some directories.  The directory we care about is the one called ``openstates``.::

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

Some states' directories will differ a bit, but all will have ``__init__.py``, ``bills.py``, and ``people.py``.  These are the NC scrapers that collect these objects.

Running Our First Scraper
-------------------------
**Step 4)** Choose a state; we'll be using NC for this tutorial.

**Step 5)** Let's run <your state's> legislator scraper (substitute your state for 'nc' below) ::

    $ docker-compose run --rm scrape nc --fastmode

The parameters you pass after ``docker-compose run --rm scrape`` are passed to ``pupa update``.  Here we're saying that we're running NC's scrapers, and that we want to do it in "fast mode."

You'll see the database start up, which is a separate Docker container, coordinated by the same docker-compose file::

    Starting openstates_database_1 ... done

And the *run plan*, which is what ``pupa`` aims to capture; in this case we're scraping the state website's data into JSON files, and then importing those JSON files into the database::

    no pupa_settings on path, using defaults
    nc (scrape, import)
      bills: {}
      people: {}
      committees: {}
      votes: {}

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

Depending on the scraper you run, this part takes a while.  Some scrapers can take hours to run, but most people scrapers take only a few minutes.

At the end of the scrape, you should see a conversion of the scraped data `from Pupa to Billy <https://github.com/openstates/meta/wiki/2017-Roadmap#pupa-ization>`_; right now our website is still on our old Billy framework, so our production database has to use that database schema. This means that the data is now in the database. Congratulations, you just ran your first state scrape!

**Step 6)** To review the data you just fetched, you can connect to the database as follows: ::

    $ docker-compose run --entrypoint mongo database mongodb://database
    
This loads the mongodb shell to the Billy database. You may close the mongo connection with::
    > quit()

You can also view the data as JSON files in the ``_data`` directory of your local repository.

.. note::
    It is of course possible that the scrape fails.  If so, there's a good chance that isn't your fault, especially if it starts to run and then errors out.  Scrapers do break, and there's no guarantee North Carolina didn't change their legislator page yesterday, breaking our tutorial here.

    If that's the case and you think the issue is with the scraper, feel free to get in touch with us or `file an issue <https://github.com/openstates/openstates/issues>`_.

Next Steps
----------

At this point you're ready to run scrapers and contribute fixes. Hop onto `our GitHub ticket queue <https://github.com/openstates/openstates/issues>`_, pick an Issue to solve, and then submit a Pull Request!

.. _getting-help:

Getting Help
------------

Right now the best way to get help is to `join our Discourse <https://discourse.openstates.org/>`_, plenty of the core team and other contributors are around to answer any questions you may have.
