Committees
==========

There are two methods available for committee data:

.. raw:: html

   <table>

.. raw:: html

   <tr>

.. raw:: html

   <th>

Method

.. raw:: html

   </th>

.. raw:: html

   <th>

URL pattern

.. raw:: html

   </th>

.. raw:: html

   <th>

Description

.. raw:: html

   </th>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

::

    <td> [Committee Search](#methods/committee-search) </td>
    <td> /committees/ </td>
    <td> Search committees by any of their attributes.  </td>

.. raw:: html

   </tr>

.. raw:: html

   <tr>

::

    <td> [Committee Detail](#methods/committee-detail) </td>
    <td> /committees/`committee_id`/ </td>
    <td> Get full detail for committee, including all members. </td>

.. raw:: html

   </tr>

.. raw:: html

   </table>

Committee Fields
----------------

The following fields are available on committee objects:

-  ``id`` Open States assigned committee ID.
-  ``state`` State abbreviation.
-  ``chamber`` Chamber committee belongs to: 'upper', 'lower', 'joint'.
-  ``committee`` Name of committee.
-  ``subcommittee`` Name of subcommittee. (if null, object describes the
   ``committee``)
-  ``parent_id`` Committee id pointing to the parent committee if this
   is a subcommittee.
-  ``sources`` List of URLs used in gathering information for this
   legislator.
-  ``created_at`` The date that this object first appeared in our
   system.
-  ``updated_at`` The date that this object was last updated in our
   system.
-  ``members`` List of member objects, each has the following keys:

   -  ``name`` Name of legislator as provided by state source.
   -  ``leg_id`` Open States-assigned legislator id. (null if no match
      found).
   -  ``role`` Member's role on the committee (e.g. 'chair',
      'vice-chair', default role is 'member')

Methods
-------

Committee Search
~~~~~~~~~~~~~~~~

This method allows searching by a number of fields:

-  ``committee``
-  ``subcommittee``
-  ``chamber``
-  ``state``

Committee objects returned by this method do not include the list of
members by default.

**Example:**
`openstates.org/api/v1/committees/?state=dc <#examples/committee-search>`__

Committee Detail
~~~~~~~~~~~~~~~~

This method returns the full committee object given a committee id.

**Example:**
`openstates.org/api/v1/committees/DCC000029/ <#examples/committee-detail>`__

Examples
--------

Committee Search
~~~~~~~~~~~~~~~~

``http://openstates.org/api/v1/committees/?state=dc``

.. code:: json

    [
     { "level": "state", 
      "created_at": "2011-11-09 02:43:35", 
      "updated_at": "2013-03-27 03:23:42", 
      "parent_id": null, 
      "state": "dc", 
      "subcommittee": null, 
      "committee": "Finance and Revenue", 
      "chamber": "upper", 
      "id": "DCC000017" }, 
     { "level": "state", 
      "created_at": "2011-11-09 02:43:35", 
      "updated_at": "2013-03-06 02:18:33", 
      "parent_id": null, 
      "state": "dc", 
      "subcommittee": null, 
      "committee": "Subcommittee on Redistricting 2011", 
      "chamber": "upper", 
      "id": "DCC000025" }, 
     { "chamber": "upper", 
      "created_at": "2013-01-07 21:05:11", 
      "updated_at": "2013-03-27 03:23:42", 
      "parent_id": null, 
      "state": "dc", 
      "subcommittee": null, 
      "committee": "Business, Consumer and Regulatory Affairs", 
      "id": "DCC000029" }, 
     { "level": "state", 
      "created_at": "2011-11-09 02:43:35", 
      "updated_at": "2013-03-27 03:23:41", 
      "parent_id": null, 
      "state": "dc", 
      "subcommittee": null, 
      "committee": "Human Services", 
      "chamber": "upper", 
      "id": "DCC000014" }, 
      ...truncated...
    ]

Committee Detail
~~~~~~~~~~~~~~~~

``http://openstates.org/api/v1/committees/DCC000029/``

.. code:: json

    {
     "chamber": "upper", 
     "committee": "Business, Consumer and Regulatory Affairs", 
     "created_at": "2013-01-07 21:05:11", 
     "id": "DCC000029", 
     "members": [
      {
       "leg_id": "DCL000014", 
       "role": "chairperson", 
       "name": "Vincent Orange"
      }, 
      {
       "leg_id": "DCL000020", 
       "role": "member", 
       "name": "David Grosso"
      }, 
      {
       "leg_id": "DCL000007", 
       "role": "member", 
       "name": "Jim Graham"
      }, 
      {
       "leg_id": "DCL000002", 
       "role": "member", 
       "name": "Mary M. Cheh"
      }, 
      {
       "leg_id": "DCL000010", 
       "role": "member", 
       "name": "Yvette Alexander"
      }
     ], 
     "parent_id": null, 
     "sources": [ { "url": "http://dccouncil.us/committees/committee-on-business-consumer-and-regulatory-affairs" } ], 
     "state": "dc", 
     "subcommittee": null, 
     "updated_at": "2013-03-27 03:23:42"
    }
