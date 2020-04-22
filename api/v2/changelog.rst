Changelog
=========

Changelog for Open States GraphQL API:

v2.4 (April 2019)
-----------------

* removed unused fields from graph (organization.links, organization.other_names)

v2.3 (August 2019)
------------------

* add experimental full text search via searchQuery parameter to bills node

v2.2 (June 2019)
-----------------

* add openstatesUrl to bills query
* speed improvments

v2.1 (Feb 2019)
------------------

* fix lat-lon behavior to limit to active memberships
* improve handling of retired legislators
* fix type of maximum_memberships
* bill version ordering is now consistent

v2.0 (January 2019)
-------------------

* bugfix for maximum_memberships type
* bugfix for versions field
* improve tests

Beta Release (November 2018)
-------------------------------

* **API Keys are now required**
* consider classification when using current_memberships
* fix geo filtering
* add openstatesUrl to Bill node for ease of linkage to OpenStates.org
* add Person.oldMemberships as analog to currentMemberships 
* add actionSince filter to bills node
* fix 500 errors/optimization when using GraphQL fragments
* addition of basic protection for excessive queries
* add totalCount to assist in pagination
* add Organization.currentMemberships


Preview Release 1 (May 2018)
----------------------------

* fix for people pagination
* add updatedSince for people
* add sponsor argument for bills node
* allow votes to take pagination parameters
* allow traversing to votes from person


Preview Release 0 (Dec 2017)
----------------------------

Initial draft release of the API, no backwards-compatibility guarantee made.
