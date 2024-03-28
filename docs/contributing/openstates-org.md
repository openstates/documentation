# History of open.pluralpolicy.com / openstates.org

open.pluralpolicy.com is the public-facing web app that has historically provided free democracy tools, API key registration,
and access to bulk data. However, we are in the process of migrating functionality into the main Plural application.
The Plural application provides continued [free legislative research and tracking tools](https://pluralpolicy.com/app/legislative-tracking)
as well as the ability to [Find Your Legislators](https://pluralpolicy.com/open/). The Plural app is not open source,
because our business model to support continued free democracy tools and expanded open data depends on providing
premium policy intelligence features to organizational customers.

For now, open.pluralpolicy.com will continue to provide a subset of related features:

- Register and manage your API key
- Bulk data downloads
- v2 of the API until is taken out of service (already deprecated, so please use the [v3 API](https://v3.openstates.org/docs)

The application is built in Django. Even after migration is complete, we will keep the repository up for anyone who
is interested in the open source code. The rest of this documentation is maintained here for reference.

## Checking out

Fork and clone the openstates.org repository:

- Visit <https://github.com/openstates/openstates.org> and click the
    'Fork' button.

- Clone your fork using your tool of choice or the command line:

        $ git clone git@github.com:yourname/openstates.org.git
        Cloning into 'openstates.org'...

- Be sure to run `poetry install` to fetch the correct version of dependencies.
- And remember to `install pre-commit <pre-commit>`:

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
own local copy of openstates.org. In a separate terminal window, run `npm run build` and `npm run start` to see the 
site's react and style components.

!!! note 
    
    If you're running into issues with models not being found or an incorrectly configured virtual environment, running
    `docker-compose build` should help to fix it.

If you have issues getting your instance up and running, please document the
errors you're seeing and [reach out](../index.md#communication).

## Running outside of Docker

It might be desirable to test outside of docker sometimes to bypass
caching or other issues that make development within the docker
environment difficult. If so, you can install
[goreman](https://github.com/mattn/goreman) (or any foreman clone) and
run `goreman start`.
