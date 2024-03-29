# Changelog

## 2021.11.12

-   add LegislativeSession.downloads

## 2021.10.27

-   add Bill.related_bills include

## 2021.10.12

-   added classification field on Person.offices
-   Person.offices now always have all fields present, even if empty

## 2021.09.24

-   added experimental event endpoints

## 2021.08.30

-   added identifiers parameter to bill search

## 2021.08.03

-   added latest_bill_update & latest_people_update to API v3

## 2021.08.02

-   addded experimental committee endpoints

## 2021.04.19

-   experimental support for Jurisdiction.latest_runs include parameter

## 2021.03.24

-   Jurisdiction classification can now be \'country\'
-   experimental US federal support

## 2021.02.23

-   people district= parameter added

## 2020.12.21

-   people name= parameter is now fuzzy-searched, matching API v1 and v2

## 2020.10.30

-   move email to a top-level field on Person responses
-   add consistent post ordering

## 2020.10.21

-   fix datetime handling for updated_since and created_since filters
-   add list of divisions when including Jurisdiction.organizations

## 2020.10.13

-   add updated_asc sort option
-   add rate limiting
-   bugfix for New York jurisdiction lookup (openstates/issues\#136)

## 2020.09.28

-   set permissive CORS settings
-   bills endpoint updates:
    -   added created_since filter, thanks to Donald Wasserman!
    -   added sponsor and sponsor_classification filters
    -   added sort parameter
    -   added useful error message when searching /bills by session
        without jurisdiction
-   restored missing Bill.from_organization field
-   introduced new fields: Person.openstates_url, Bill.openstates_url

## 2020.09.14

-   removed some unused fields from responses
-   removed deprecated government classification from Jurisdiction

## 2020.09.10

-   added Jurisdiction.legislative_sessions
-   corrected initial pagination limits for release

## 2020.09.09

-   Initial beta release.
