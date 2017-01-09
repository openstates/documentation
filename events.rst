Events
======

Events are not available in all states, to ensure that events are
available check the ``feature_flags`` list in a states'
`metadata <metadata.html>`__.

There are two methods available for event data:

Event Fields
------------

The following fields are available on event objects:

-  ``id`` Open States assigned event ID.
-  ``state`` State abbreviation.
-  ``type`` Categorized event type. ('committee:meeting' for now)
-  ``description`` Description of event from state source.
-  ``documents`` List of related documents.
-  ``location`` Location if known, as given by state (is often just a
   room number).
-  ``when`` Time event begins.
-  ``end`` End time (null if unknown).
-  ``timezone`` Timezone event occurs in (e.g. 'America/Chicago').
-  ``participants`` List of participant objects, consisting of the
   following fields:

   -  ``chamber`` Chamber of participant.
   -  ``type`` Type of participants ('legislator', 'committee')
   -  ``participant`` String representation of participant (e.g.
      'Housing Committee', 'Jill Smith')
   -  ``id`` Open States id for participant if a match was found (e.g.
      'TXC000150', 'MDL000101')
   -  ``type`` What role this participant played (will be 'host',
      'chair', 'participant').

-  ``related_bills`` List of related bills for this event. Comprised of
   the following fields:

   -  ``type`` Type of relationship (e.g. 'consideration')
   -  ``description`` Description of how the bill is related given by
      the state.
   -  ``bill_id`` State's bill id (e.g. 'HB 273')
   -  ``id`` Open States assigned bill id (e.g. 'TXB00001234')

-  ``sources`` List of URLs used in gathering information for this
   legislator.
-  ``created_at`` The date that this object first appeared in our
   system.
-  ``updated_at`` The date that this object was last updated in our
   system.

Methods
-------

.. _event-search:

Event Search
~~~~~~~~~~~~

This method allows searching by a number of fields:

-  ``state``
-  ``type``

This method also allows specifying an alternate output format, by
specifying ``format=rss`` or ``format=ics``.

**Example:**
`openstates.org/api/v1/events/?state=ca <#examples/event-search>`__

.. _event-detail:

Event Detail
~~~~~~~~~~~~

This method returns an event object given an event id.

**Example:**
`openstates.org/api/v1/events/TXE00026474/ <#examples/event-detail>`__

Examples
--------

Event Search
~~~~~~~~~~~~

``http://openstates.org/api/v1/events/?state=tx``

.. code:: json

    [
     {
      "documents": [], 
      "end": null, 
      "description": "Special Purpose Districts", 
      "state": "tx", 
      "+agenda": "HOUSE OF REPRESENTATIVES NOTICE OF FORMAL MEETING \u00a0 COMMITTEE:\u00a0\u00a0 Special Purpose Districts\u00a0 TIME & DATE: During reading and referral of bills Thursday, March 21, 2013\u00a0 PLACE:\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 3W.9\u00a0 CHAIR:\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Rep. Dennis Bonnen\u00a0 \u00a0 \u00a0 Notice of this meeting was announced from the house floor.", 
      "created_at": "2013-03-24 08:38:18", 
      "when": "2013-03-21 05:00:00", 
      "updated_at": "2013-03-24 08:38:18", 
      "sources": [
       {
        "url": "http://www.capitol.state.tx.us/tlodocs/83R/schedules/html/C4482013032100001.HTM"
       }
      ], 
      "participants": [
       {
        "chamber": "lower", 
        "participant_type": "committee", 
        "participant": "Special Purpose Districts", 
        "id": "TXC000150", 
        "type": "host"
       }, 
       {
        "chamber": "lower", 
        "participant_type": "legislator", 
        "participant": "Rep. Dennis Bonnen", 
        "id": "TXL000223", 
        "type": "chair"
       }
      ], 
      "session": "83", 
      "location": "3W.9\u00a0 ", 
      "related_bills": [], 
      "timezone": "America/Chicago", 
      "type": "committee:meeting", 
      "id": "TXE00026474", 
      "+chamber": "lower"
     }, 
     {
      "documents": [], 
      "end": null, 
      "description": "State Affairs", 
      "state": "tx", 
      "+agenda": "HOUSE OF REPRESENTATIVES NOTICE OF FORMAL MEETING \u00a0 COMMITTEE:\u00a0\u00a0 State Affairs\u00a0 TIME & DATE: During reading and referral of bills Thursday, March 21, 2013\u00a0 PLACE:\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Agricultural Museum, 1W.14\u00a0 CHAIR:\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Rep. Byron Cook\u00a0 \u00a0 Notice of this meeting was announced from the House floor.", 
      "created_at": "2013-03-24 08:38:18", 
      "when": "2013-03-21 05:00:00", 
      "updated_at": "2013-03-24 08:38:18", 
      "sources": [
       {
        "url": "http://www.capitol.state.tx.us/tlodocs/83R/schedules/html/C4502013032100001.HTM"
       }
      ], 
      "participants": [
       {
        "chamber": "lower", 
        "participant_type": "committee", 
        "participant": "State Affairs", 
        "id": "TXC000022", 
        "type": "host"
       }, 
       {
        "chamber": "lower", 
        "participant_type": "legislator", 
        "participant": "Rep. Byron Cook", 
        "id": "TXL000236", 
        "type": "chair"
       }
      ], 
      "session": "83", 
      "location": "Agricultural Museum, 1W.14\u00a0 ", 
      "related_bills": [], 
      "timezone": "America/Chicago", 
      "type": "committee:meeting", 
      "id": "TXE00026476", 
      "+chamber": "lower"
     }, 
     {
      "documents": [], 
      "end": null, 
      "description": "Defense & Veterans' Affairs", 
      "type": "committee:meeting", 
      "created_at": "2013-03-15 07:37:08", 
      "related_bills": [
       {
        "type": "consideration", 
        "description": "Bill up for discussion", 
        "bill_id": "HB 846", 
        "id": "TXB00024869"
       }, 
       {
        "type": "consideration", 
        "description": "Bill up for discussion", 
        "bill_id": "HB 1348", 
        "id": "TXB00025984"
       }, 
       {
        "type": "consideration", 
        "description": "Bill up for discussion", 
        "bill_id": "HB 1832", 
        "id": "TXB00026956"
       }, 
       {
        "type": "consideration", 
        "description": "Bill up for discussion", 
        "bill_id": "HB 1939", 
        "id": "TXB00027260"
       }, 
       {
        "type": "consideration", 
        "description": "Bill up for discussion", 
        "bill_id": "HB 2387", 
        "id": "TXB00028147"
       }, 
       {
        "type": "consideration", 
        "description": "Bill up for discussion", 
        "bill_id": "HB 2392", 
        "id": "TXB00028152"
       }, 
       {
        "type": "consideration", 
        "description": "Bill up for discussion", 
        "bill_id": "HB 2071", 
        "id": "TXB00027470"
       }
      ], 
      "when": "2013-03-21 13:00:00", 
      "updated_at": "2013-03-21 08:03:49", 
      "sources": [
       {
        "url": "http://www.capitol.state.tx.us/tlodocs/83R/schedules/html/C3052013032108001.HTM"
       }
      ], 
      "state": "tx", 
      "session": "83", 
      "location": "E2.012\u00a0 ", 
      "participants": [
       {
        "chamber": "lower", 
        "participant_type": "committee", 
        "participant": "Defense & Veterans' Affairs", 
        "id": "TXC000058", 
        "type": "host"
       }, 
       {
        "chamber": "lower", 
        "participant_type": "legislator", 
        "participant": "Rep. Jos\u00e9 Men\u00e9ndez", 
        "id": "TXL000312", 
        "type": "chair"
       }
      ], 
      "timezone": "America/Chicago", 
      "+agenda": "** REVISION **HOUSE OF REPRESENTATIVES NOTICE OF PUBLIC HEARING \u00a0 COMMITTEE:\u00a0\u00a0 Defense & Veterans' Affairs\u00a0 TIME & DATE: 8:00 AM, Thursday, March 21, 2013\u00a0 PLACE:\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 E2.012\u00a0 CHAIR:\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Rep. Jos\u00e9 Men\u00e9ndez\u00a0 \u00a0 HB 846\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Lucio III Relating to additional periods of possession of or access to a child after conclusion of a parent's military deployment. HB 1348\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Men\u00e9ndez\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Relating to the taxation of certain tangible personal property located inside a defense base development authority. HB 1832\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Miller, Rick\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Relating to granting certain local governments general zoning authority around certain military facilities; providing a penalty. HB 1939\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Orr\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Relating to a veteran's employment preference for employment with a public entity or public work of this state. HB 2387\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Men\u00e9ndez\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Relating to the taxation of certain tangible personal property located inside a defense base development authority. HB 2392\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Men\u00e9ndez\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Relating to the mental health program for veterans. \u00a0 \u00a0 Bills deleted after last posting: HB 2071 HCR 69 \u00a0 **\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 See Committee Coordinator for previous versions\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 ** of the schedule, if applicable. NOTICE OF ASSISTANCE AT PUBLIC MEETINGS Persons with disabilities who plan to attend this meeting and who may need assistance, such as a sign language interpreter, are requested to contact Stacey Nicchio at (512) 463-0850, 72 hours prior to the meeting so that appropriate arrangements can be made. \u00a0 To find information about electronic witness registration for a public hearing and to create a profile to be used when registering as a witness, please visit www.house.state.tx.us/resources/. Registration must be performed the day of the meeting and within the Capitol Complex.", 
      "id": "TXE00026387", 
      "+chamber": "lower"
     }, 
     ...truncated...
    ]

Event Detail
~~~~~~~~~~~~

``http://openstates.org/api/v1/event/TXE00026474/``

.. code:: json

    {
     "+agenda": "HOUSE OF REPRESENTATIVES NOTICE OF FORMAL MEETING \u00a0 COMMITTEE:\u00a0\u00a0 Special Purpose Districts\u00a0 TIME & DATE: During reading and referral of bills Thursday, March 21, 2013\u00a0 PLACE:\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 3W.9\u00a0 CHAIR:\u00a0\u00a0\u00a0\u00a0\u00a0\u00a0 Rep. Dennis Bonnen\u00a0 \u00a0 \u00a0 Notice of this meeting was announced from the house floor.", 
     "+chamber": "lower", 
     "created_at": "2013-03-24 08:38:18", 
     "description": "Special Purpose Districts", 
     "documents": [], 
     "end": null, 
     "id": "TXE00026474", 
     "location": "3W.9\u00a0 ", 
     "participants": [
      {
       "chamber": "lower", 
       "participant_type": "committee", 
       "participant": "Special Purpose Districts", 
       "id": "TXC000150", 
       "type": "host"
      }, 
      {
       "chamber": "lower", 
       "participant_type": "legislator", 
       "participant": "Rep. Dennis Bonnen", 
       "id": "TXL000223", 
       "type": "chair"
      }
     ], 
     "related_bills": [], 
     "session": "83", 
     "sources": [
      {
       "url": "http://www.capitol.state.tx.us/tlodocs/83R/schedules/html/C4482013032100001.HTM"
      }
     ], 
     "state": "tx", 
     "timezone": "America/Chicago", 
     "type": "committee:meeting", 
     "updated_at": "2013-03-24 08:38:18", 
     "when": "2013-03-21 05:00:00"
    }
