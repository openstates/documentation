Changelog
=========

Changelog for Open States GraphQL API:

Preview Release 2 (November 2018)
-------------------------------

* **API Keys are now required**
* consider classification when using current_memberships
* fix geo filtering
* add openstatesUrl to Bill node for ease of linkage to OpenStates.org
* add Person.oldMemberships as analog to currentMemberships 
* add actionSince filter to bills node
* fix 500 errors/optimization when using GraphQL fragments
* addition of basic protection for excessive queries


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
