
.. _metadata:

State Metadata
==============

.. warning:: API v1 is deprecated, please check out `API v3 <https://docs.openstates.org/en/latest/api/v3/>`_ or `API v2 <https://docs.openstates.org/en/latest/api/v2/>`_ both of which are actively supported.

:ref:`metadata-overview`
    Get list of all states with data available and basic metadata about their status.
:ref:`state-metadata`
    Get detailed metadata for a particular state.

Metadata Fields
---------------

The following fields are available on metadata objects:

-  ``abbreviation`` The two-letter abbreviation of the state.
-  ``capitol_timezone`` Timezone of state capitol (e.g.
   'America/New\_York')
-  ``chambers`` Dictionary mapping chamber type (upper/lower) to an
   object with the following fields:

   -  ``name`` Short name of the chamber (e.g. 'House', 'Senate')
   -  ``title`` Title of legislators in this chamber (e.g. 'Senator')

-  ``feature_flags`` A list of which optional features are available,
   options include:

   -  'subjects' - bills have categorized subjects
   -  'influenceexplorer' - legislators have influence explorer ids
   -  'events' - event data is present

-  ``latest_csv_date`` Date that the CSV file at ``latest_csv_url`` was
   generated.
-  ``latest_csv_url`` URL from which a CSV dump of all data for this
   state can be obtained.
-  ``latest_json_date`` Date that the JSON file at ``latest_json_url``
   was generated.
-  ``latest_json_url`` URL from which a JSON dump of all data for this
   state can be obtained.
-  ``latest_update`` Last time a successful scrape was run.
-  ``legislature_name`` Full name of legislature (e.g. 'North Carolina
   General Assembly')
-  ``legislature_url`` URL to legislature's official website.
-  ``name`` Name of state.
-  ``session_details`` Dictionary of session names to detail
   dictionaries with the following keys:

   -  ``type`` 'primary' or 'special'
   -  ``display_name`` e.g. '2009-2010 Session'
   -  ``start_date`` date session began
   -  ``end_date`` date session began

-  ``terms`` List of terms in order that they occurred. Each item in the
   list is comprised of the following keys:

   -  ``start_year`` Year session started.
   -  ``end_year`` Year session ended.
   -  ``name`` Display name for term (e.g. '2009-2011').
   -  ``sessions`` List of sessions (e.g. '2009'). Each session will be
      present in ``session_details``.

.. _terms-sessions:

Terms & Sessions
~~~~~~~~~~~~~~~~

A common area for confusion, terms describe a period of time between
legislative elections, for example '2009-2010'. A term can be comprised
of one or more sessions:depending on how often the legislature
met/adjourned within the term.

Terms are associated with legislators, while sessions are associated
with bills.

Methods
-------

.. _metadata-overview:

Metadata Overview
~~~~~~~~~~~~~~~~~

This method returns just a subset (``abbreviation``, ``name``,
``chambers``, ``feature_flags``) of metadata across all available
entities.

**Example:**
:ref:`openstates.org/api/v1/metadata/ <metadata-overview-example>`

.. _state-metadata:

State Metadata
~~~~~~~~~~~~~~

This method returns the full metadata for a state.

**Example:**
:ref:`openstates.org/api/v1/metadata/nc/ <metadata-detail-example>`

Examples
--------

.. _metadata-overview-example:

Metadata Overview
~~~~~~~~~~~~~~~~~

``openstates.org/api/v1/metadata/``

.. code::

    [
     { "name": "Alabama",
      "abbreviation": "al",
      "feature_flags": [ "subjects", "influenceexplorer" ],
      "chambers": {
       "upper": { "name": "Senate", "title": "Senator" },
       "lower": { "name": "House", "title": "Representative" }
      } },
     { "name": "Alaska",
      "abbreviation": "ak",
      "feature_flags": [ "subjects", "influenceexplorer" ],
      "chambers": {
       "upper": { "name": "Senate", "title": "Senator" },
       "lower": { "name": "House", "title": "Representative" }
      } },
     { "name": "Arizona",
      "abbreviation": "az",
      "feature_flags": [ "events", "influenceexplorer" ],
      "chambers": {
       "upper": { "name": "Senate", "title": "Senator" },
       "lower": { "name": "House", "title": "Representative" }
      } },
     { "name": "Arkansas",
      "abbreviation": "ar",
      "feature_flags": [ "influenceexplorer" ],
      "chambers": {
       "upper": { "name": "Senate", "title": "Senator" },
       "lower": { "name": "House", "title": "Representative" }
      } },
     { "name": "California",
      "abbreviation": "ca",
      "feature_flags": [ "subjects", "influenceexplorer" ],
      "chambers": {
       "upper": { "name": "Senate", "title": "Senator" },
       "lower": { "name": "Assembly", "title": "Assemblymember" }
      } },
     { "name": "Colorado",
      "abbreviation": "co",
      "feature_flags": [ "influenceexplorer" ],
      "chambers": {
       "upper": { "name": "Senate", "title": "Senator" },
       "lower": { "name": "House", "title": "Representative" }
      } },
     { "name": "Connecticut",
      "abbreviation": "ct",
      "feature_flags": [ "subjects", "events", "influenceexplorer" ],
      "chambers": {
       "upper": { "name": "Senate", "title": "Senator" },
       "lower": { "name": "House", "title": "Representative" }
      } },
     { "name": "Delaware",
      "abbreviation": "de",
      "feature_flags": [ "events", "influenceexplorer" ],
      "chambers": {
       "upper": { "name": "Senate", "title": "Senator" },
       "lower": { "name": "House", "title": "Representative" }
      } },
     { "name": "District of Columbia",
      "abbreviation": "dc",
      "feature_flags": [],
      "chambers": {
       "upper": { "name": "Council", "title": "Councilmember" }
      } },
      ...truncated...
    ]

.. _metadata-detail-example:

State Metadata
~~~~~~~~~~~~~~

``openstates.org/api/v1/metadata/nc/``

.. code:: json

    {
     "abbreviation": "nc",
     "capitol_timezone": "America/New_York",
     "chambers": {
      "upper": { "name": "Senate", "title": "Senator" },
      "lower": { "name": "House", "title": "Representative" }
     },
     "feature_flags": [ "subjects", "influenceexplorer" ],
     "id": "nc",
     "latest_csv_date": "2013-03-01 09:04:45",
     "latest_csv_url": "http://static.openstates.org/downloads/2013-03-01-nc-csv.zip",
     "latest_json_date": "2013-03-05 23:46:34",
     "latest_json_url": "http://static.openstates.org/downloads/2013-03-05-nc-json.zip",
     "latest_update": "2013-03-24 01:38:51",
     "legislature_name": "North Carolina General Assembly",
     "legislature_url": "http://www.ncleg.net/",
     "name": "North Carolina",
     "session_details": {
      "2009": { "type": "primary", "display_name": "2009-2010 Session", "start_date": "2009-01-28 00:00:00" },
      "2011": { "type": "primary", "display_name": "2011-2012 Session", "start_date": "2011-01-26 00:00:00" },
      "2013": { "type": "primary", "display_name": "2013-2014 Session", "start_date": "2013-01-30 00:00:00" }
     },
     "terms": [
      { "end_year": 2010, "start_year": 2009, "name": "2009-2010", "sessions": [ "2009" ] },
      { "end_year": 2012, "start_year": 2011, "name": "2011-2012", "sessions": [ "2011" ] },
      { "end_year": 2014, "start_year": 2013, "name": "2013-2014", "sessions": [ "2013" ] }
     ]
    }
