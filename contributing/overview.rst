.. _overview:

Overview & Roadmap
==================

Open States is a fairly large & somewhat complex project comprised of many moving parts with a long history.

As you look to contribute, it may be beneficial to understand a little bit about the various components.

Repositories
------------

These repositories make up the core of the project, if you're looking to contribute there's a 90% chance one of these is what you're looking for.

- `openstates`_ - Open States bill & vote scrapers.
- `people`_ - Open States People data, maintained as editable YAML files
- `text-extraction`_ - Text extraction powering full text search.
- `openstates.org`_ - Powers https://openstates.org/ website & API.
- `metadata`_ - Data on states and their districts, etc. that changes rarely.
- `documentation`_ - https://docs.openstates.org/ (you're reading it now)

These repositories are other pieces of our infrastructure, but are generally not interesting to the average person.

- `task-definitions`_ - Definitions for bobsled.
- `maintenance-scripts`_ - Internal scripts used for various maintenance tasks.
- `indiana-docs`_ - A proxy for fetching Indiana’s docs without API key.
- `blog`_ - https://blog.openstates.org/
- `openstates-district-maps`_ - Source for generating Open States' map tiles.


2020 Roadmap
------------

Our current priorities:

Power User Features
~~~~~~~~~~~~~~~~~~~

- Add user logins & profiles.   (**launched January 2020**)
- Introduce bill & issue tracking.  (**launched February 2020**)

Data Quality
~~~~~~~~~~~~

- Improve data quality and timeliness.  (**in progress**)
- Provide publicly accessible data quality dashboard.  (Q2)

API
~~~

- Improve speed of most popular graph queries.  (TBD)
- Provide simplified endpoints for common queries.  (TBD)
- Introduce a pub/sub type mechanism for staying in sync with bill & vote updates.  (TBD)

Bulk Data
~~~~~~~~~

- Add new per-state CSV data exports.  (**launched February 2020**)
- <s>Add custom data-export creation page.</s> (**replaced with per-session JSON data instead, February 2020**)
- Provide bulk geographic data ahead of 2021 redistricting. (TBD)

Community
~~~~~~~~~

- Documentation updates (**in progress**)
- New Contributor Support (Q1-Q2)
- API User Dashboard (**launched March 2020**)

Recent Major Work
-----------------

To give a sense of recent priorities, here are major milestones from the past few years:

- `Legislation Tracking <https://blog.openstates.org/tracking-legislation-on-open-states/>`_ - Q1 2020
- **Restoration of Historical Legislator Data** - Q4 2019
- `Full Text Search <https://blog.openstates.org/adding-full-text-search-to-open-states-14b665c1fe30/>`_ - Q4 2019
- **2019 Legislative Session Updates** - Q1 2019
- `OpenStates.org 2019 rewrite <https://blog.openstates.org/introducing-the-new-openstates-org-64bcbd765f58/>`_ - Q1 2019
- `OpenStates GraphQL API <https://blog.openstates.org/more-ways-to-get-state-legislative-data-d9aece2245f0/>`_ - Q4 2018
- **Scraper Overhaul** - Throughout much of 2017 we reworked our scrapers to be more resilient and to use an updated tech stack, replacing the one that powered the site from 2011-2016.


.. _text-extraction: https://github.com/openstates/text-extraction
.. _blog: https://github.com/openstates/blog
.. _maintenance-scripts: https://github.com/openstates/maintenance-scripts
.. _documentation: https://github.com/openstates/documentation
.. _indiana-docs: https://github.com/openstates/indiana-docs
.. _openstates.org: https://github.com/openstates/openstates.org
.. _openstates-district-maps: https://github.com/openstates/openstates-district-maps
.. _openstates: https://github.com/openstates/openstates
.. _people: https://github.com/openstates/people
.. _metadata: https://github.com/openstates/metadata
.. _task-definitions: https://github.com/openstates/task-definitions

