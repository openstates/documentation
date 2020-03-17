
.. _running-the-site:

Working On openstates.org
=========================

openstates.org is the public-facing result of all the work we do.  The site is built in Django and includes the web frontend and API.

Checking out
------------

Fork and clone the openstates.org repository:

  * Visit https://github.com/openstates/openstates.org and click the 'Fork' button.
  * Clone your fork using your tool of choice or the command line::

        $ git clone git@github.com:yourname/openstates.org.git
        Cloning into 'openstates.org'...

  * And remember to :ref:`install pre-commit <pre-commit>`::

        $ pre-commit install
        pre-commit installed at .git/hooks/pre-commit

.. _working-database:

Getting a working database
--------------------------

Whether you're aiming to work on openstates.org or just want to import scraped data, you'll need postgres server running in your docker environment.

If you haven't set up docker yet, see :ref:`prerequisites`.

To download a recent copy of the Open States database and restore it::

  docker-compose up -d db
  docker-compose run --rm --entrypoint ./docker/init-db.sh django

.. warning::
  This command takes several GB of disk space.  The pgdump file that is downloaded is ~4GB as of March 2020 and the restored database is around 8GB.  If you don't have room for this but want to contribute let us know so we can prioritize more compact options.

You'll see this command download a file from S3, which can take a while depending upon your internet connection.  It will then go silent for a while as it works to restore the database.  This takes 5-10 minutes on a late-2018 Macbook Pro, but your experience may vary.  So long as it isn't spitting out errors, things should be fine.

If you're working on scrapers you'll now find that this database is available to your scrape processes! 

Running Tests
-------------

You can run the tests for the project via::

  docker-compose run --rm --entrypoint ./docker/run-tests.sh django

You can also append standard pytest arguments such as `-x` to bail on first failure.

Example of running just the v1 tests, bailing on error::

  docker-compose run --rm --entrypoint ./docker/run-tests.sh django v1 -x

Repository overview
-------------------

The project is rather large, with quite a few django apps, here's a quick guide:

Django Apps:

  * bulk/       - handles bulk downloads on the website
  * dashboards/ - dashboards for viewing various statistics
  * geo/        - geography services for legislator lookup
  * graphapi/   - powers GraphQL API
  * profiles/   - user and subscription management
  * public/     - public-facing pages (bulk of the site)
  * utils/      - utilities shared by the other applications
  * v1/         - backwards-compatibility shim implementing much of the old v1 API 

Other Stuff:

  * ansible/ - the files used to deploy OpenStates.org are here
  * docker/  - special scripts for running tests, etc. within docker
  * openstates/ - core Django settings files
  * static/     - various static assets, including frontend code
  * templates/  - Django templates


Running openstates.org
----------------------

Simply running ``docker-compose up`` should start django & the database, then browse to http://localhost:8000 and you'll be looking at your own local copy of openstates.org
