.. _v3:

Open States v3 API
==================

Open States provides a JSON API that can be used to programatically access state legislative information.

API Basics
----------

The root URL for the API is https://v3.openstates.org/.

API keys are required.  You can `register for an API key <https://openstates.org/accounts/profile/>`_ and once activated, you'll pass your API key via the ``X-API-KEY`` header or ``?apikey`` query parameter.

Auto-generated interactive documentation is available at either:

  * https://v3.openstates.org/docs/ 
  * https://v3.openstates.org/redoc/ (whichever you prefer)


Issues should be filed at `our issue tracker <https://github.com/openstates/issues/issues>`_.

You can also check out our `introductory blog post <TODO>`_ for more details.


Methods
-------

====================================  ====================================================   ================================================================================================================
Method                                Description                                            Interactive Docs
====================================  ====================================================   ================================================================================================================
/jurisdictions                        Get list of available jurisdictions.                   `[1] <https://v3.openstates.org/docs#/jurisdictions/jurisdiction_list_jurisdictions_get>`_
/jurisdictions/{jurisdiction_id}      Get detailed metadata for a particular jurisdiction.   `[2] <https://v3.openstates.org/docs#/jurisdictions/jurisdiction_detail_jurisdictions__jurisdiction_id__get>`_
/people                               List or search people (legislators, governors, etc.)   `[3] <https://v3.openstates.org/docs#/people/people_search_people_get>`_
/people.geo                           Get legislators for a given location.                  `[4] <https://v3.openstates.org/docs#/people/people_geo_people_geo_get>`_
/bills                                Search bills by various criteria.                      `[5] <https://v3.openstates.org/docs#/bills/bills_search_bills_get>`_
/bills/ocd-bill/{uuid}                Get bill by internal ID.                               `[6] <https://v3.openstates.org/docs#/bills/bill_detail_by_id_bills_ocd_bill__openstates_bill_id__get>`_
/bills/{jurisdiction}/{session}/{id}  Get bill by jurisdiction, session, and ID.             `[7] <https://v3.openstates.org/docs#/bills/bill_detail_bills__jurisdiction___session___bill_id__get>`_
====================================  ====================================================   ================================================================================================================

Concepts
--------

**Jurisdiction**
  The fundamental unit by which data is partitioned is the 'jurisdiction.'  If you are just interested in states you can consider the words synonymous for the most part.  Jurisdictions include states, DC & Puerto Rico, and municipal governments for which we have limited support.

**Person**
  A legislator, governor, mayor, etc.

  Each person possibly has a number of roles, at most one of which is considered 'current.'

**Bill**
  A proposed piece of legislation, encompasses bills, resolutions, constitutional amendments, etc.

  A given bill may have any number of votes, sponsorships, actions, etc.

Further Details
---------------

.. toctree::
   :maxdepth: 2

   changelog
