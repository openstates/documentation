# API v3 Overview

Open States provides a JSON API that can be used to programatically
access state legislative information.

## API Basics

The root URL for the API is <https://v3.openstates.org/>.

API keys are required. You can [register for an API
key](https://open.pluralpolicy.com/accounts/profile/) and once activated,
you\'ll pass your API key via the `X-API-KEY` header or `?apikey` query
parameter.

Auto-generated interactive documentation is available at either:

> -   <https://v3.openstates.org/docs/>
> -   <https://v3.openstates.org/redoc/> (whichever you prefer)

Issues should be filed at [our issue
tracker](https://github.com/openstates/issues/issues).

You can also check out our [introductory blog
post](https://blog.openstates.org/open-states-api-v3-beta/) for more
details.

## Methods

  Method                                |Description                                           |Interactive Docs
  --------------------------------------|------------------------------------------------------|----------------------------------------------------------------------------------------------------------------
  /jurisdictions                        |Get list of available jurisdictions.                  |[try it!](https://v3.openstates.org/docs#/jurisdictions/jurisdiction_list_jurisdictions_get)
  /jurisdictions/{jurisdiction_id}      |Get detailed metadata for a particular jurisdiction.  |[try it!](https://v3.openstates.org/docs#/jurisdictions/jurisdiction_detail_jurisdictions__jurisdiction_id__get)
  /people                               |List or search people (legislators, governors, etc.)  |[try it!](https://v3.openstates.org/docs#/people/people_search_people_get)
  /people.geo                           |Get legislators for a given location.                 |[try it!](https://v3.openstates.org/docs#/people/people_geo_people_geo_get)
  /bills                                |Search bills by various criteria.                     |[try it!](https://v3.openstates.org/docs#/bills/bills_search_bills_get)
  /bills/ocd-bill/{uuid}                |Get bill by internal ID.                              |[try it!](https://v3.openstates.org/docs#/bills/bill_detail_by_id_bills_ocd_bill__openstates_bill_id__get)
  /bills/{jurisdiction}/{session}/{id}  |Get bill by jurisdiction, session, and ID.            |[try it!](https://v3.openstates.org/docs#/bills/bill_detail_bills__jurisdiction___session___bill_id__get)
  /committees                           |Get list of committees by jurisdiction.               |[try it!](https://v3.openstates.org/docs#/committees/committee_list_committees_get)
  /committees/{committee_id}            |Get details on committee by internal ID.              |[try it!](https://v3.openstates.org/docs#/committees/committee_detail_committees__committee_id__get)
  /events                               |Get list of events by jurisdiction.                   |[try it!](https://v3.openstates.org/docs#/events/event_list_events_get)
  /events/{event_id}                    |Get details on event by internal ID.                  |[try it!](https://v3.openstates.org/docs#/events/event_detail_events__event_id__get)


## Concepts

**Jurisdiction**

:   The fundamental unit by which data is partitioned is the
    \'jurisdiction.\' If you are just interested in states you can
    consider the words synonymous for the most part. Jurisdictions
    include states, DC & Puerto Rico, and municipal governments for
    which we have limited support.

**Person**

:   A legislator, governor, mayor, etc.

    Each person possibly has a number of roles, at most one of which is
    considered \'current.\'

**Bill**

:   A proposed piece of legislation, encompasses bills, resolutions,
    constitutional amendments, etc.

    A given bill may have any number of votes, sponsorships, actions,
    etc.
