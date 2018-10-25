.. _legislators:

Legislators
===========

.. note:: A beta of API v2 is now available, please check out `API v2 <http://docs.openstates.org/en/latest/api/v2/>`_.

:ref:`legislator-search`
    Search legislators by their attributes.
:ref:`legislator-detail`
    Get full detail for a legislator, including all roles.
:ref:`legislator-geo`
    Lookup all legislators that serve districts containing a given point.

Legislator Fields
-----------------

The following fields are available on legislator objects:

-  ``leg_id`` Legislator's permanent Open States ID. (e.g. 'ILL000555',
   'NCL000123')
-  ``state`` Legislator's state.
-  ``active`` Boolean value indicating whether or not the legislator is
   currently in office.
-  ``chamber`` Chamber the legislator is currently serving in if active
   ('upper' or 'lower')
-  ``district`` District the legislator is currently serving in if
   active (e.g. '7', '6A')
-  ``party`` Party the legislator is currently representing if active.
-  ``email`` Legislator's primary email address.
-  ``full_name`` Full display name for legislator.
-  ``first_name`` First name of legislator.
-  ``middle_name`` Middle name of legislator.
-  ``last_name`` Last name of legislator.
-  ``suffixes`` Name suffixes (e.g. 'Jr.', 'III') of legislator.
-  ``photo_url`` URL of an official photo of this legislator.
-  ``url`` URL of an official webpage for this legislator.
-  ``created_at`` The date that this object first appeared in our
   system.
-  ``updated_at`` The date that this object was last updated in our
   system.
-  ``created_at`` Date at which this legislator was added to our system.
-  ``updated_at`` Date at which this legislator was last updated.
-  ``offices`` List of office objects representing contact details for
   the legislator. Comprised of the following fields:

   -  ``type`` 'capitol' or 'district'
   -  ``name`` Name of the address (e.g. 'Council Office', 'District
      Office')
   -  ``address`` Street address.
   -  ``phone`` Phone number.
   -  ``fax`` Fax number.
   -  ``email`` Email address. *Any of these fields may be ``null`` if
      not found.*

-  ``roles`` List of currently active :ref:`role objects <roles>` if legislator is in office.
-  ``old_roles`` Dictionary mapping term keys to lists of roles that
   were valid for that term.

.. _roles:

Roles
~~~~~

``roles`` and ``old_roles`` are comprised of role objects.

Role objects can have the following fields:

-  ``term`` Term key for this role. (See metadata :ref:`notes on terms and
   sessions <terms-sessions>` for
   details.)
-  ``chamber``
-  ``state``
-  ``start_date`` (optional)
-  ``end_date`` (optional)
-  ``type`` 'member' or 'committee member'

If the role type is 'member':

-  ``party``
-  ``district``

And if the type is 'committee member':

-  ``committee`` name of parent committee
-  ``subcommittee`` name of subcommittee (if null, membership is just
   for a committee)
-  ``committee_id`` Open States id for committee that legislator is a
   member of
-  ``position`` position on committee
-  ``old_roles``
-  ``sources`` List of URLs used in gathering information for this
   legislator.

Methods
-------

.. _legislator-search:

Legislator Search
~~~~~~~~~~~~~~~~~

This method allows looking up a legislator by a number of parameters,
the results do not include the ``roles`` or ``old_roles`` items by
default.

Parameters
^^^^^^^^^^

-  ``state`` Filter by state.
-  ``chamber`` Only legislators with a role in the specified chamber.
-  ``district`` Only legislators that have represented the specified
   district.

**Example:**
:ref:`openstates.org/api/v1/legislators/?state=dc&chamber=upper <example-legislator-search>`

.. _legislator-detail:

Legislator Detail
~~~~~~~~~~~~~~~~~

This method returns the full detail for a legislator.

**Example:**
:ref:`openstates.org/api/v1/legislators/DCL000012/ <example-legislator-detail>`

.. _legislator-geo:

Geo Lookup
~~~~~~~~~~

Lookup all legislators serving districts containing a given location.

**Example:**
:ref:`openstates.org/api/v1/legislators/geo/?lat=35.79&long=-78.78 <example-legislator-geo>`

Examples
--------

.. _example-legislator-search:

Legislator Search
~~~~~~~~~~~~~~~~~

``openstates.org/api/v1/legislators/?state=dc&chamber=upper``

.. code:: json

    [
     {
      "first_name": "Anita", 
      "last_name": "Bonds", 
      "middle_name": "", 
      "district": "At-Large", 
      "chamber": "upper", 
      "url": "http://dccouncil.us/council/anita-bonds", 
      "created_at": "2013-01-07 21:05:06", 
      "updated_at": "2013-03-26 03:22:24", 
      "email": "abonds@dccouncil.us", 
      "active": true, 
      "state": "dc", 
      "offices": [
       {
        "fax": "(202) 724-8099", 
        "name": "Council Office", 
        "phone": "(202) 724-8064", 
        "address": "1350 Pennsylvania Avenue NW, Suite 408, Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "full_name": "Anita Bonds", 
      "leg_id": "DCL000021", 
      "party": "Democratic", 
      "suffixes": "", 
      "id": "DCL000021", 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/AAA_small.jpg"
     }, 
     {
      "+fax": "(202) 724-8099", 
      "last_name": "Mendelson", 
      "updated_at": "2013-03-26 03:20:14", 
      "full_name": "Phil Mendelson", 
      "id": "DCL000005", 
      "first_name": "Phil", 
      "middle_name": "", 
      "district": "Chairman", 
      "office_address": "1350 Pennsylvania Avenue NW, Suite 402, Washington, DC 20004", 
      "state": "dc", 
      "votesmart_id": "72089", 
      "party": "Democratic", 
      "email": "pmendelson@dccouncil.us", 
      "leg_id": "DCL000005", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/mendelson.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/phil-mendelson", 
      "created_at": "2011-02-17 22:43:55", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 724-8099", 
        "name": "Council Office", 
        "phone": "(202) 724-8032     ", 
        "address": "1350 Pennsylvania Avenue NW, Suite 504, Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "suffixes": "", 
      "+phone": "(202) 724-8064      "
     }, 
     {
      "first_name": "David", 
      "last_name": "Grosso", 
      "middle_name": "", 
      "district": "At-Large", 
      "chamber": "upper", 
      "url": "http://dccouncil.us/council/david-grosso", 
      "created_at": "2013-01-07 21:05:06", 
      "updated_at": "2013-03-26 03:22:24", 
      "email": "dgrosso@dccouncil.us", 
      "active": true, 
      "state": "dc", 
      "offices": [
       {
        "fax": "(202) 724-8071", 
        "name": "Council Office", 
        "phone": "(202) 724-8105", 
        "address": "1350 Pennsylvania Avenue NW, Suite 406, Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "full_name": "David Grosso", 
      "leg_id": "DCL000020", 
      "party": "Independent", 
      "suffixes": "", 
      "id": "DCL000020", 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/david_grosso_color__small.jpg"
     }, 
     {
      "+fax": "(202) 741-0911", 
      "last_name": "Alexander", 
      "updated_at": "2013-03-26 03:22:24", 
      "full_name": "Yvette Alexander", 
      "id": "DCL000010", 
      "first_name": "Yvette", 
      "middle_name": "", 
      "district": "Ward 7", 
      "office_address": "1350 Pennsylvania Avenue, Suite 400, NW Washington, DC 20004", 
      "state": "dc", 
      "votesmart_id": "72072", 
      "party": "Democratic", 
      "email": "yalexander@dccouncil.us", 
      "leg_id": "DCL000010", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/alexander_dec2011.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/yvette-alexander", 
      "created_at": "2011-02-17 22:43:55", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 741-0911", 
        "name": "Council Office", 
        "phone": "(202) 724-8068", 
        "address": "1350 Pennsylvania Avenue, Suite 400, NW Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "+phone": "(202) 724-8068", 
      "suffixes": ""
     }, 
     {
      "+fax": "(202) 724-8054", 
      "last_name": "Wells", 
      "updated_at": "2013-03-26 03:22:24", 
      "full_name": "Tommy Wells", 
      "id": "DCL000008", 
      "first_name": "Tommy", 
      "middle_name": "", 
      "district": "Ward 6", 
      "office_address": "1350 Pennsylvania Avenue, Suite 408, NW Washington, DC 20004", 
      "state": "dc", 
      "votesmart_id": "72071", 
      "party": "Democratic", 
      "email": "twells@dccouncil.us", 
      "leg_id": "DCL000008", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/wells2.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/tommy-wells", 
      "created_at": "2011-02-17 22:43:55", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 724-8054", 
        "name": "Council Office", 
        "phone": "(202) 724-8072", 
        "address": "1350 Pennsylvania Avenue, Suite 402, NW Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "+phone": "(202) 724-8072", 
      "suffixes": ""
     }, 
     {
      "+fax": "(202) 727-8210", 
      "last_name": "Orange", 
      "updated_at": "2013-03-26 03:22:24", 
      "full_name": "Vincent Orange", 
      "id": "DCL000014", 
      "first_name": "Vincent", 
      "middle_name": "", 
      "district": "At-Large", 
      "office_address": "1350 Pennsylvania Avenue NW, Suite 107, Washington, DC 20004", 
      "state": "dc", 
      "party": "Democratic", 
      "email": "vorange@dccouncil.us", 
      "leg_id": "DCL000014", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/orange.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/vincent-orange", 
      "created_at": "2011-05-12 02:08:19", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 727-8210", 
        "name": "Council Office", 
        "phone": "(202) 724-8174      ", 
        "address": "1350 Pennsylvania Avenue NW, Suite 107, Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "+phone": "(202) 724-8174      ", 
      "suffixes": ""
     }, 
     {
      "+fax": "(202) 741-0908", 
      "last_name": "Bowser", 
      "updated_at": "2013-03-26 03:22:24", 
      "full_name": "Muriel Bowser", 
      "id": "DCL000011", 
      "first_name": "Muriel", 
      "middle_name": "", 
      "district": "Ward 4", 
      "office_address": "1350 Pennsylvania Avenue, Suite 110, NW Washington, DC 20004", 
      "state": "dc", 
      "votesmart_id": "72064", 
      "party": "Democratic", 
      "email": "mbowser@dccouncil.us", 
      "leg_id": "DCL000011", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/Bowser_Official_Photo_2012_small.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/muriel-bowser", 
      "created_at": "2011-02-17 22:43:55", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 741-0908", 
        "name": "Council Office", 
        "phone": "(202) 724-8052", 
        "address": "1350 Pennsylvania Avenue, Suite 110, NW Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "suffixes": "", 
      "+phone": "(202) 724-8052"
     }, 
     {
      "+fax": "(202) 724-8087", 
      "last_name": "Catania", 
      "updated_at": "2013-03-26 03:22:24", 
      "full_name": "David Catania", 
      "id": "DCL000003", 
      "first_name": "David", 
      "middle_name": "", 
      "district": "At-Large", 
      "office_address": "1350 Pennsylvania Avenue NW, Suite 404, Washington, DC 20004", 
      "state": "dc", 
      "votesmart_id": "72081", 
      "party": "Independent", 
      "email": "dcatania@dccouncil.us", 
      "leg_id": "DCL000003", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/catania.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/david-catania", 
      "created_at": "2011-02-17 22:43:55", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 724-8087", 
        "name": "Council Office", 
        "phone": "(202) 724-7772      ", 
        "address": "1350 Pennsylvania Avenue NW, Suite 404, Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "+phone": "(202) 724-7772      ", 
      "suffixes": ""
     }, 
     {
      "+fax": "(202) 724-8076", 
      "last_name": "McDuffie", 
      "updated_at": "2013-03-26 03:22:24", 
      "full_name": "Kenyan McDuffie", 
      "id": "DCL000017", 
      "first_name": "Kenyan", 
      "middle_name": "", 
      "district": "Ward 5", 
      "office_address": "1350 Pennsylvania Avenue NW, Suite 410, Washington, DC 20004", 
      "state": "dc", 
      "party": "Democratic", 
      "email": "kmcduffie@dccouncil.us", 
      "leg_id": "DCL000017", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/Councilmember_Kenyan_R._McDuffie_Official_Photograph_small.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/kenyan-mcduffie", 
      "created_at": "2012-05-31 02:28:23", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 724-8076", 
        "name": "Council Office", 
        "phone": "(202) 724-8028 ", 
        "address": "1350 Pennsylvania Avenue NW, Suite 506, Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "suffixes": "", 
      "+phone": "(202) 724-8028 "
     }, 
     {
      "+fax": "(202) 724-8023", 
      "last_name": "Evans", 
      "updated_at": "2013-03-26 03:22:24", 
      "full_name": "Jack Evans", 
      "id": "DCL000009", 
      "first_name": "Jack", 
      "middle_name": "", 
      "district": "Ward 2", 
      "office_address": "1350 Pennsylvania Avenue, Suite 106, NW Washington, DC 20004", 
      "state": "dc", 
      "votesmart_id": "72044", 
      "party": "Democratic", 
      "email": "jevans@dccouncil.us", 
      "leg_id": "DCL000009", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/evans.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/jack-evans", 
      "created_at": "2011-02-17 22:43:55", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 724-8023", 
        "name": "Council Office", 
        "phone": "(202) 724-8058", 
        "address": "1350 Pennsylvania Avenue, Suite 106, NW Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "+phone": "(202) 724-8058", 
      "suffixes": ""
     }, 
     {
      "+fax": "(202) 724-8109", 
      "last_name": "Graham", 
      "updated_at": "2013-03-26 03:22:24", 
      "full_name": "Jim Graham", 
      "id": "DCL000007", 
      "first_name": "Jim", 
      "middle_name": "", 
      "district": "Ward 1", 
      "office_address": "1350 Pennsylvania Avenue, Suite 105, NW Washington, DC 20004", 
      "state": "dc", 
      "votesmart_id": "72038", 
      "party": "Democratic", 
      "email": "jgraham@dccouncil.us", 
      "leg_id": "DCL000007", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/graham.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/jim-graham", 
      "created_at": "2011-02-17 22:43:55", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 724-8109", 
        "name": "Council Office", 
        "phone": "(202) 724-8181", 
        "address": "1350 Pennsylvania Avenue, Suite 105, NW Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "+phone": "(202) 724-8181", 
      "suffixes": ""
     }, 
     {
      "+fax": "(202) 724-8118", 
      "last_name": "Cheh", 
      "updated_at": "2013-03-26 03:22:24", 
      "full_name": "Mary M Cheh", 
      "id": "DCL000002", 
      "first_name": "Mary", 
      "middle_name": "M", 
      "district": "Ward 3", 
      "office_address": "1350 Pennsylvania Avenue, Suite 108, NW  Washington, DC 20004", 
      "state": "dc", 
      "votesmart_id": "72047", 
      "party": "Democratic", 
      "email": "mcheh@dccouncil.us", 
      "leg_id": "DCL000002", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/cheh.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/mary-m.-cheh", 
      "created_at": "2011-02-17 22:43:55", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 724-8118", 
        "name": "Council Office", 
        "phone": "(202) 724-8062", 
        "address": "1350 Pennsylvania Avenue, Suite 108, NW  Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "+phone": "(202) 724-8062", 
      "suffixes": ""
     }, 
     {
      "+fax": "(202) 724-8055", 
      "last_name": "Barry", 
      "updated_at": "2013-03-26 03:22:24", 
      "full_name": "Marion Barry", 
      "id": "DCL000012", 
      "first_name": "Marion", 
      "middle_name": "", 
      "district": "Ward 8", 
      "office_address": "1350 Pennsylvania Avenue NW, Suite 102, Washington, DC 20004", 
      "state": "dc", 
      "votesmart_id": "72074", 
      "party": "Democratic", 
      "email": "mbarry@dccouncil.us", 
      "leg_id": "DCL000012", 
      "active": true, 
      "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/barry.jpg", 
      "level": "state", 
      "url": "http://dccouncil.us/council/marion-barry", 
      "created_at": "2011-02-17 22:43:55", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": "(202) 724-8055", 
        "name": "Council Office", 
        "phone": "(202) 724-8045", 
        "address": "1350 Pennsylvania Avenue NW, Suite 102, Washington, DC 20004", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "+phone": "(202) 724-8045", 
      "suffixes": ""
     }
    ]

.. _example-legislator-detail:

Legislator Detail
~~~~~~~~~~~~~~~~~

``openstates.org/api/v1/legislators/DCL000012/``

.. code:: json

    {
     "active": true, 
     "chamber": "upper", 
     "created_at": "2011-02-17 22:43:55", 
     "district": "Ward 8", 
     "email": "mbarry@dccouncil.us", 
     "first_name": "Marion", 
     "full_name": "Marion Barry", 
     "id": "DCL000012", 
     "last_name": "Barry", 
     "leg_id": "DCL000012", 
     "level": "state", 
     "middle_name": "", 
     "office_address": "1350 Pennsylvania Avenue NW, Suite 102, Washington, DC 20004", 
     "offices": [
      {
       "fax": "(202) 724-8055", 
       "name": "Council Office", 
       "phone": "(202) 724-8045", 
       "address": "1350 Pennsylvania Avenue NW, Suite 102, Washington, DC 20004", 
       "type": "capitol", 
       "email": null
      }
     ], 
     "old_roles": {
      "2011-2012": [
       {
        "term": "2011-2012", 
        "end_date": null, 
        "district": "Ward 8", 
        "chamber": "upper", 
        "state": "dc", 
        "party": "Democratic", 
        "type": "member", 
        "start_date": null
       }, 
       {
        "term": "2011-2012", 
        "committee_id": "DCC000017", 
        "chamber": "upper", 
        "state": "dc", 
        "subcommittee": null, 
        "committee": "Finance and Revenue", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2011-2012", 
        "committee_id": "DCC000027", 
        "chamber": "upper", 
        "state": "dc", 
        "subcommittee": null, 
        "committee": "Jobs and Workforce Development", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2011-2012", 
        "committee_id": "DCC000021", 
        "chamber": "upper", 
        "state": "dc", 
        "subcommittee": null, 
        "committee": "the Judiciary", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2011-2012", 
        "committee_id": "DCC000019", 
        "chamber": "upper", 
        "state": "dc", 
        "subcommittee": null, 
        "committee": "Aging and Community Affairs", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2011-2012", 
        "committee_id": "DCC000026", 
        "chamber": "upper", 
        "state": "dc", 
        "subcommittee": null, 
        "committee": "Economic Development and Housing", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2011-2012", 
        "committee_id": "DCC000014", 
        "chamber": "upper", 
        "state": "dc", 
        "subcommittee": null, 
        "committee": "Human Services", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2011-2012", 
        "committee_id": "DCC000023", 
        "chamber": "upper", 
        "state": "dc", 
        "subcommittee": null, 
        "committee": "Health", 
        "position": "member", 
        "type": "committee member"
       }
      ]
     }, 
     "party": "Democratic", 
     "photo_url": "http://dccouncil.us/files/user_uploads/member_photos/barry.jpg", 
     "roles": [
      {
       "term": "2013-2014", 
       "end_date": null, 
       "district": "Ward 8", 
       "chamber": "upper", 
       "state": "dc", 
       "party": "Democratic", 
       "type": "member", 
       "start_date": null
      }, 
      {
       "term": "2013-2014", 
       "committee_id": "DCC000014", 
       "chamber": "upper", 
       "state": "dc", 
       "subcommittee": null, 
       "committee": "Human Services", 
       "position": "member", 
       "type": "committee member"
      }, 
      {
       "term": "2013-2014", 
       "committee_id": "DCC000017", 
       "chamber": "upper", 
       "state": "dc", 
       "subcommittee": null, 
       "committee": "Finance and Revenue", 
       "position": "member", 
       "type": "committee member"
      }, 
      {
       "term": "2013-2014", 
       "committee_id": "DCC000032", 
       "chamber": "upper", 
       "state": "dc", 
       "subcommittee": null, 
       "committee": "Education", 
       "position": "member", 
       "type": "committee member"
      }, 
      {
       "term": "2013-2014", 
       "committee_id": "DCC000031", 
       "chamber": "upper", 
       "state": "dc", 
       "subcommittee": null, 
       "committee": "Workforce and Community Affairs", 
       "position": "member", 
       "type": "committee member"
      }
     ], 
     "sources": [ { "url": "http://dccouncil.us/council/marion-barry" } ], 
     "state": "dc", 
     "suffixes": "", 
     "updated_at": "2013-03-26 03:22:24", 
     "url": "http://dccouncil.us/council/marion-barry", 
     "votesmart_id": "72074"
    }

.. _example-legislator-geo:

Geo Lookup
~~~~~~~~~~

``openstates.org/api/v1/legislators/geo/?lat=35.79&long=-78.78``

.. code:: json

    [
     {
      "last_name": "Stein", 
      "suffix": "", 
      "updated_at": "2013-03-27 02:35:39", 
      "sources": [ { "url": "http://www.ncga.state.nc.us/gascripts/members/viewMember.pl?sChamber=Senate&nUserID=267" } ], 
      "full_name": "Josh Stein", 
      "old_roles": {
       "2009-2010": [
        {
         "term": "2009-2010", 
         "end_date": null, 
         "district": "16", 
         "level": "state", 
         "chamber": "upper", 
         "state": "nc", 
         "party": "Democratic", 
         "type": "member", 
         "start_date": null
        }, 
        {
         "term": "2009-2010", 
         "committee_id": "NCC000002", 
         "level": "state", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Appropriations on Department of Transportation", 
         "type": "committee member"
        }, 
        {
         "term": "2009-2010", 
         "committee_id": "NCC000008", 
         "level": "state", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Appropriations/Base Budget", 
         "type": "committee member"
        }, 
        {
         "term": "2009-2010", 
         "committee_id": "NCC000009", 
         "level": "state", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Commerce", 
         "type": "committee member"
        }, 
        {
         "term": "2009-2010", 
         "committee_id": "NCC000010", 
         "level": "state", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Education/Higher Education", 
         "type": "committee member"
        }, 
        {
         "term": "2009-2010", 
         "committee_id": "NCC000073", 
         "level": "state", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Finance", 
         "type": "committee member"
        }, 
        {
         "term": "2009-2010", 
         "committee_id": "NCC000012", 
         "level": "state", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Health Care", 
         "type": "committee member"
        }, 
        {
         "term": "2009-2010", 
         "committee_id": "NCC000074", 
         "level": "state", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Judiciary I", 
         "type": "committee member"
        }, 
        {
         "term": "2009-2010", 
         "committee_id": "NCC000022", 
         "level": "state", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Select Committee on Economic Recovery", 
         "type": "committee member"
        }, 
        {
         "term": "2009-2010", 
         "committee_id": "NCC000024", 
         "level": "state", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Select Committee on Energy, Science and Technology", 
         "type": "committee member"
        }
       ], 
       "2011-2012": [
        {
         "term": "2011-2012", 
         "end_date": null, 
         "district": "16", 
         "chamber": "upper", 
         "state": "nc", 
         "party": "Democratic", 
         "type": "member", 
         "start_date": null
        }, 
        {
         "term": "2011-2012", 
         "committee_id": "NCC000009", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Commerce", 
         "position": "member", 
         "type": "committee member"
        }, 
        {
         "term": "2011-2012", 
         "committee_id": "NCC000100", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Education / Higher Education", 
         "position": "member", 
         "type": "committee member"
        }, 
        {
         "term": "2011-2012", 
         "committee_id": "NCC000073", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Finance", 
         "position": "member", 
         "type": "committee member"
        }, 
        {
         "term": "2011-2012", 
         "committee_id": "NCC000074", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Judiciary I", 
         "position": "member", 
         "type": "committee member"
        }, 
        {
         "term": "2011-2012", 
         "committee_id": "NCC000018", 
         "chamber": "upper", 
         "state": "nc", 
         "subcommittee": null, 
         "committee": "Rules and Operations of the Senate", 
         "position": "member", 
         "type": "committee member"
        }
       ]
      }, 
      "id": "NCL000047", 
      "first_name": "Josh", 
      "middle_name": "", 
      "district": "16", 
      "state": "nc", 
      "votesmart_id": "102971", 
      "party": "Democratic", 
      "email": "Josh.Stein@ncleg.net", 
      "leg_id": "NCL000047", 
      "boundary_id": "sldu/nc-16", 
      "active": true, 
      "transparencydata_id": "d3917a35b626477a9a7afaf7dbf206be", 
      "photo_url": "http://www.ncga.state.nc.us/Senate/pictures/hiRes/267.jpg", 
      "roles": [
       {
        "term": "2013-2014", 
        "end_date": null, 
        "district": "16", 
        "chamber": "upper", 
        "state": "nc", 
        "party": "Democratic", 
        "type": "member", 
        "start_date": null
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000009", 
        "chamber": "upper", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Commerce", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000100", 
        "chamber": "upper", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Education / Higher Education", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000073", 
        "chamber": "upper", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Finance", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000012", 
        "chamber": "upper", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Health Care", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000074", 
        "chamber": "upper", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Judiciary I", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000018", 
        "chamber": "upper", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Rules and Operations of the Senate", 
        "position": "member", 
        "type": "committee member"
       }
      ], 
      "level": "state", 
      "url": "http://www.ncga.state.nc.us/gascripts/members/viewMember.pl?sChamber=Senate&nUserID=267", 
      "created_at": "2010-08-03 17:14:46", 
      "nimsp_id": "9383", 
      "chamber": "upper", 
      "offices": [
       {
        "fax": null, 
        "name": "Capitol Office", 
        "phone": "(919) 715-6400", 
        "address": "NC Senate\n16 W. Jones Street, Room 1113\n\nRaleigh, NC 27601-2808", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "suffixes": ""
     }, 
     {
      "last_name": "Hall", 
      "updated_at": "2013-03-27 02:35:42", 
      "sources": [
       {
        "url": "http://www.ncga.state.nc.us/gascripts/members/viewMember.pl?sChamber=House&nUserID=679"
       }
      ], 
      "full_name": "Duane Hall", 
      "id": "NCL000282", 
      "first_name": "Duane", 
      "middle_name": "", 
      "district": "11", 
      "state": "nc", 
      "party": "Democratic", 
      "email": "Duane.Hall@ncleg.net", 
      "leg_id": "NCL000282", 
      "boundary_id": "sldl/nc-11", 
      "+notice": null, 
      "transparencydata_id": "07eff70ee51441d093b33667a2a6f877", 
      "active": true, 
      "photo_url": "http://www.ncga.state.nc.us/House/pictures/hiRes/679.jpg", 
      "roles": [
       {
        "term": "2013-2014", 
        "end_date": null, 
        "district": "11", 
        "chamber": "lower", 
        "state": "nc", 
        "party": "Democratic", 
        "type": "member", 
        "start_date": null
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000028", 
        "chamber": "lower", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Appropriations", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000035", 
        "chamber": "lower", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Appropriations Subcommittee on Transportation", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000082", 
        "chamber": "lower", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Commerce and Job Development", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000178", 
        "chamber": "lower", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Commerce and Job Development Subcommittee on Alcoholic Beverage Control", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000168", 
        "chamber": "lower", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Elections", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000088", 
        "chamber": "lower", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Government", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000107", 
        "chamber": "lower", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Homeland Security, Military, and Veterans Affairs", 
        "position": "member", 
        "type": "committee member"
       }, 
       {
        "term": "2013-2014", 
        "committee_id": "NCC000172", 
        "chamber": "lower", 
        "state": "nc", 
        "subcommittee": null, 
        "committee": "Public Utilities and Energy", 
        "position": "member", 
        "type": "committee member"
       }
      ], 
      "url": "http://www.ncga.state.nc.us/gascripts/members/viewMember.pl?sChamber=House&nUserID=679", 
      "created_at": "2013-01-03 19:15:14", 
      "chamber": "lower", 
      "offices": [
       {
        "fax": null, 
        "name": "Capitol Office", 
        "phone": "919-733-5755", 
        "address": "NC House of Representatives\n16 W. Jones Street, Room 1019\n\nRaleigh, NC 27601-1096", 
        "type": "capitol", 
        "email": null
       }
      ], 
      "suffixes": ""
     }
    ]
