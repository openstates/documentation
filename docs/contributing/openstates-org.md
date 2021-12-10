# Working On openstates.org

openstates.org is the public-facing result of all the work we do. The
site is built in Django and includes the web frontend and API.

## Checking out

Fork and clone the openstates.org repository:

-   Visit <https://github.com/openstates/openstates.org> and click the
    'Fork' button.

-   Clone your fork using your tool of choice or the command line:

        $ git clone git@github.com:yourname/openstates.org.git
        Cloning into 'openstates.org'...

-   And remember to `install pre-commit <pre-commit>`:

        $ pre-commit install
        pre-commit installed at .git/hooks/pre-commit

## Getting a working database

See [Running a Local Database](local-database.md) to get your database ready for OpenStates.org.

## Running Tests

You can run the tests for the project via:

    ./docker/run-tests.sh

You can also append standard pytest arguments such as `-x` to bail on first failure.

Example of running just the graphapi tests, bailing on error:

    ./docker/run-tests.sh graphapi -x

## Repository overview

The project is rather large, with quite a few django apps, here's a
quick guide:

Django Apps:

-   bulk/ - handles bulk downloads on the website
-   dashboards/ - dashboards for viewing various statistics
-   geo/ - geography services for legislator lookup
-   graphapi/ - powers GraphQL API
-   profiles/ - user and subscription management
-   public/ - public-facing pages (bulk of the site)
-   utils/ - utilities shared by the other applications

Other Stuff:

-   ansible/ - the files used to deploy OpenStates.org are here
-   docker/ - special scripts for running tests, etc. within docker
-   openstates/ - core Django settings files
-   static/ - various static assets, including frontend code
-   templates/ - Django templates

## Running openstates.org

Simply running `docker-compose up` should start django & the database,
then browse to <http://localhost:8000> and you'll be looking at your
own local copy of openstates.org

## Running outside of Docker

It might be desirable to test outside of docker sometimes to bypass
caching or other issues that make development within the docker
environment difficult. If so, you can install
[goreman](https://github.com/mattn/goreman) (or any foreman clone) and
run `goreman start`.
