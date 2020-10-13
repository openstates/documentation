API v3 Changelog
================

2020.10.13
----------

- add updated_asc sort option
- add rate limiting
- bugfix for New York jurisdiction lookup (openstates/issues#136)

2020.09.28
----------

- set permissive CORS settings
- bills endpoint updates:

  - added created_since filter, thanks to Donald Wasserman!
  - added sponsor and sponsor_classification filters
  - added sort parameter
  - added useful error message when searching /bills by session without jurisdiction

- restored missing Bill.from_organization field
- introduced new fields: Person.openstates_url, Bill.openstates_url

2020.09.14
----------

- removed some unused fields from responses
- removed deprecated government classification from Jurisdiction

2020.09.10
----------

- added Jurisdiction.legislative_sessions
- corrected initial pagination limits for release

2020.09.09
----------

- Initial beta release.
