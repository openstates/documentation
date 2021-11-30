# Getting Started

No matter how experienced you are, it is a good idea to read this
section before diving into Open States' code.

This guide assumes a basic familiarity with using the command line, git,
and Python.

No worries if you aren't an expert though, we'll walk you through the
steps. And as for Python, if you've written other languages like
Javascript or Ruby you'll probably be just fine. Don't be afraid to
[ask for help](../index.md#communication) either!

## Get Familiar With Our Processes

We're glad to have you joining us, taking a few minutes to read the
following pages will help you be a better member of our community:

-   Our [Code of Conduct](../code-of-conduct.md) is important to us, and helps us maintain a healthy community.
-   We also have a [guide to help you learn where to get help](../index.md#communication) that you should look over.

## Installing docker

The first thing you will need to do is get a working docker environment
on your local machine. We'll do this using Docker. No worries if you
aren't familiar with Docker, you'll barely have to touch it beyond
what this guide explains.

Install Docker and docker-compose (if not already installed on your
local system):

**(a)** Installing Docker:

-   On OSX: [Docker for Mac](https://docs.docker.com/docker-for-mac/)
-   On Windows: [Docker for Windows](https://docs.docker.com/docker-for-windows/)
-   On Linux: Use your package manager of choice or [follow Docker's instructions](https://docs.docker.com/engine/installation/linux/).

(*Docker Compose is probably already installed by step 1(a) if not, proceed to step 1(b)*)

**(b)** Installing docker-compose:

-   For easy installation on [macOS, Windows, and 64-bit Linux.](https://docs.docker.com/compose/install/#prerequisites)

Ensure that Docker and docker-compose are installed locally:

    $ docker --version
    Docker version 19.03.4, build 9013bf5
    $ docker-compose --version
    docker-compose version 1.24.1, build 4667896b

Of course, your versions will differ, but ensure they are relatively
recent to avoid strange issues.

## Installing pre-commit

To help keep the code as managable as possible we **strongly recommend**
you use pre-commit to make sure all commits adhere to our preferred
style.

-   See [pre-commit's installation instructions](https://pre-commit.com/#installation)

-   Within each repo you check out, run `pre-commit install` after checking out. It should look something like:

        $ pre-commit install
        pre-commit installed at .git/hooks/pre-commit

!!! note

    If you're running `flake8` and `black` yourself via your editor or
    similar this isn't strictly necessary, but we find it helps ensure
    commits don't fail linting. **We require all PRs to pass linting!**

## Project Overview

Open States is a fairly large & somewhat complex project comprised of
many moving parts with a long history.

As you look to contribute, it may be beneficial to understand a little
bit about the various components.


These repositories make up the core of the project, if you're looking
to contribute there's a 90% chance one of these is what you want.

-   [openstates-scrapers](https://github.com/openstates/openstates-scrapers) - Open States' scrapers.
-   [people](https://github.com/openstates/people) - Open States People data, maintained as editable YAML files
-   [openstates-core](https://github.com/openstates/openstates-core) - Open States data model & scraper backend.
-   [openstates.org](https://github.com/openstates/openstates.org) - Powers <https://openstates.org/> website & GraphQL API.
-   [api-v3](https://github.com/openstates/api-v3) - Powers <https://v3.openstates.org> API.
-   [documentation](https://github.com/openstates/documentation) - <https://docs.openstates.org/> (you're reading it now)

These repositories are other pieces of our infrastructure, but are generally not interesting to the average contributor.

-   [task-definitions](https://github.com/openstates/task-definitions) - Tasks for our task runner.
-   [maintenance-scripts](https://github.com/openstates/maintenance-scripts) - Internal scripts used for various maintenance tasks.
-   [indiana-docs](https://github.com/openstates/indiana-docs) - A proxy for fetching Indiana's docs without API key.
-   [blog](https://github.com/openstates/blog) - <https://blog.openstates.org/>
-   [openstates-geo](https://github.com/openstates/openstates-geo) - Source for generating Open States' map tiles.

## Recent Major Work

To give a sense of recent priorities, here are major milestones from the
past few years:

- [Federal Data & Committee Data](https://blog.openstates.org/open-states-2021-q2/) - 2021
- [API v3](https://blog.openstates.org/open-states-api-v3/) - Q3 2020
- [Legislation Tracking](https://blog.openstates.org/tracking-legislation-on-open-states/) - Q1 2020
- **Restoration of Historical Legislator Data** - Q4 2019
- [Full Text Search](https://blog.openstates.org/adding-full-text-search-to-open-states-14b665c1fe30/) - Q4 2019
- **2019 Legislative Session Updates** - Q1 2019
- [OpenStates.org 2019 rewrite](https://blog.openstates.org/introducing-the-new-openstates-org-64bcbd765f58/) - Q1 2019
- [OpenStates GraphQL API](https://blog.openstates.org/more-ways-to-get-state-legislative-data-d9aece2245f0/) - Q4 2018
- **Scraper Overhaul** - Throughout much of 2017 we reworked our
  scrapers to be more resilient and to use an updated tech stack,
  replacing the one that powered the site from 2011-2016.
