# Running a Local Database

If you want to ensure your scraped data imports or work on OpenStates.org or API v3, you'll need a local database.

This can be a bit cumbersome, since running Postgres locally varies a lot platform-to-platform, and you'll need to populate it as well.

If you're comfortable with Postgres, most of these steps can be easily modified to use your own Postgres instance, but for the remainder of this guide we'll be using a dockerized postgres image.

## Prerequisites

Be sure you've already installed `docker` and `docker-compose`, as noted in [Installing Prerequisites](index.md#installing-prerequisites).

You'll need [openstates-scrapers](https://github.com/openstates/openstates-scrapers) checked out, even if you aren't working on scrapers.  This repository has the `docker-compose.yml` config and initialization scripts for the database.

If you want to initialize the database for [openstates.org](https://github.com/openstates/openstates.org) work you'll need that project checked out as well.

## Initialize Database For Scraping

0. Run `init-db.sh` from within the `openstates-scrapers` directory:

!!! warning

    If you've already run this before, running `scripts/init-db.sh` will reset your database to scratch!

``` console
openstates-scrapers/$ ./scripts/init-db.sh
+ unset DATABASE_URL
+ docker-compose down
Removing scrapers_db_1 ... done
Removing network openstates-network
+ docker volume rm openstates-postgres
openstates-postgres
+ docker-compose up -d db
Creating network "openstates-network" with the default driver
Creating volume "openstates-postgres" with default driver
Creating scrapers_db_1 ... done
+ sleep 3
+ DATABASE_URL=postgis://openstates:openstates@db/openstatesorg
+ docker-compose run --rm --entrypoint 'poetry run os-initdb' scrape
Creating scrapers_scrape_run ... done
Operations to perform:
  Apply all migrations: contenttypes, data
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying data.0001_initial... OK
  Applying data.0002_auto_20200422_0028... OK
  Applying data.0003_auto_20200422_0031... OK

...TRUNCATED...

loading WY
loading DC
loading PR
loading US
```

This will populate your database with the tables needed for scraping, as well as some basic static data such as the jurisdiction metadata.  If all you want to do is run scrapers and import data into a database for inspection, you're good to go!

## Initialize Database for OpenStates.org

!!! note

    You **must** run `openstates-scrapers`' init-db.sh as shown above first!

From within the `openstates.org` directory run `docker/init-db.sh`:


``` console
openstates.org/$ ./docker/init-db.sh
+ unset DATABASE_URL
+ docker-compose run --rm -e PYTHONPATH=docker/ --entrypoint 'poetry run ./manage.py migrate' django
Creating openstatesorg_django_run ... done
Operations to perform:
  Apply all migrations: account, admin, auth, bulk, bundles, contenttypes, dashboards, data, people_admin, profiles, sessions, sites, socialaccount
Running migrations:
  Applying auth.0001_initial... OK

...TRUNCATED...

docker-compose run --rm -e PYTHONPATH=docker/ --entrypoint 'poetry run ./manage.py shell -c "import testdata"' django
```

This creates the Django-specific tables, and also creates a local API key `testkey` that can be used for local development.
