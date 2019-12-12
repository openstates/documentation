
.. _running-the-site:

Working On openstates.org
=========================

openstates.org is the public-facing result of all the scraping we do.  The site is built in Django and includes the web frontend and API.

Checking Out
------------

Fork and clone the main `openstates.org repository <https://github.com/openstates/openstates.org>`_:

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

There's a one line command that will download a recent copy of the Open States database and restore it::

  docker-compose run --rm --entrypoint ./docker/download-db.sh django

.. warning::
  This command takes several GB of disk space.  The pgdump file that is downloaded is ~2GB as of December 2019 and the restored database is around 7GB.  If you don't have room for this but want to contribute let us know so we can prioritize more compact options.

You'll see this command download a file from S3, which can take a while depending upon your internet connection.  It will then go silent for a while as it works to restore the database.  This takes 5-10 minutes on a late-2018 Macbook Pro, but your experience may vary.  So long as it isn't spitting out errors, things should be fine.

If you're working on scrapers you'll now find that this database is available to your scrape processes! 

Running openstates.org
----------------------

Simply running ``docker-compose up`` should start django & the database, then browse to http://localhost:8000 and you'll be looking at your own local copy of openstates.org

More coming soon!
