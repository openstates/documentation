.. _bills:

Bills
=====

:ref:`bill-search`
    Search bills by (almost) any of their attributes, or full text.
:ref:`bill-detail`
    Get full detail for bill, including any actions, votes, etc.


Bill Fields
-----------

The following fields are available on bill objects:

-  ``state`` State abbreviation.
-  ``session`` Session key (see :ref:`metadata` for details).
-  ``bill_id`` The official id of the bill (e.g. 'SB 27', 'A 2111')
-  ``title`` The official title of the bill.
   Bill titles vary widely in size and content by state.
   Some are over 10KB long, while others are a few non-specific words,
   e.g. "Regarding taxes".
-  ``alternate_titles`` List of alternate titles that the bill has had.
   (Often empty.)
-  ``action_dates`` Dictionary of notable action dates (useful for
   determining status). Contains the following fields:

   -  ``first`` First action (only null if there are no actions).
   -  ``last`` Last action (only null if there are no actions).
   -  ``passed_lower`` Date that the bill seems to have passed the lower
      chamber (might be null).
   -  ``passed_upper`` Date that the bill seems to have passed the upper
      chamber (might be null).
   -  ``signed`` Date that the bill appears to have signed into law
      (might be null).

-  ``actions`` List of objects representing every recorded action for
   the bill. Action objects have the following fields:

   -  ``date`` Date of action.
   -  ``action`` Name of action as state provides it.
   -  ``actor`` The chamber, person, committee, etc. responsible for
      this action.
   -  ``type`` Open States-provided action categories, see :ref:`action-categorization`.

-  ``chamber`` The chamber of origination ('upper' or 'lower')
-  ``created_at`` The date that this object first appeared in our
   system. (Note: not the date of introduction, see ``action_dates`` for
   that information.)
-  ``updated_at`` The date that this object was last updated in our
   system. (Note: not the last action date, see ``action_dates`` for
   that information.)
-  ``documents`` List of associated documents, see ``versions`` for
   field details.
-  ``id`` Open States-assigned permanent ID for this bill.
-  ``scraped_subjects`` List of subject areas that the state categorized
   this bill under.
-  ``subjects`` List of Open States standardized bill subjects, see
   :ref:`subject-categorization`.
-  ``sources`` List of source URLs used to compile information on this
   object.
-  ``sponsors`` List of bill sponsors.

   -  ``name`` Name of sponsor as it appears on state website.
   -  ``leg_id`` Open States assigned legislator ID (will be null if no
      match was found).
   -  ``type`` Type of sponsor ('primary' or 'cosponsor')

-  ``type`` List of :ref:`bill-types`.
-  ``versions`` Versions of the bill text. Both documents and
   ``versions`` have the following fields:

   -  ``url`` Official URL for this document.
   -  ``name`` An official name for this document.
   -  ``mimetype`` The mimetype for the document (e.g. 'text/html')
   -  ``doc_id`` An Open States-assigned id uniquely identifying this
      document.

-  ``votes`` List of vote objects.  Votes may be in the past or future;
      a future vote does not have vote-outcome fields like ``passed``.
      A vote object consists of the following fields:

   -  ``motion`` Name of motion being voted upon (e.g. 'Passage').
      The nature of these varies widely by state.
      Some states have a concise vocabulary, some a sloppy vocabulary.
      Other states include a vote ID in the motion, rendering every motion unique.
   -  ``chamber`` Chamber vote took place in ('upper', 'lower', 'joint')
   -  ``date`` Date of vote.
   -  ``passed`` Boolean; true if *vote* (not bill) succeeded.
   -  ``id`` Open States-assigned unique identifier for vote.
   -  ``state`` State abbreviation.
   -  ``session`` Session key (see :ref:`metadata` for details).
   -  ``sources`` List of source URLs used to compile information on
      this object. (Can be empty if vote shares sources with bill.)
   -  ``yes_count`` Total number of yes votes.
   -  ``no_count`` Total number of no votes.
   -  ``other_count`` Total number of 'other' votes (abstain, not
      present, etc.).
   -  ``yes_votes``, ``no_votes``, ``other_votes`` List of roll calls of
      each type. Each is an object consisting of two keys:

      -  ``name`` Name of voter as it appears on state website.
      -  ``leg_id`` Open States assigned legislator ID (will be null if
         no match was found).

Methods
-------

.. _bill-search:

Bill Search
~~~~~~~~~~~

This method returns just a subset (``state``, ``chamber``, ``session``,
``subjects``, ``type``, ``id``, ``bill_id``, ``title``, ``created_at``,
``updated_at``) of the bill fields by default.

Filter Parameters
^^^^^^^^^^^^^^^^^

The following parameters filter the returned set of bills, at least one
must be provided.

-  ``state`` Only return bills from a given state (e.g. 'nc')
-  ``chamber`` Only return bills matching the provided chamber ('upper'
   or 'lower')
-  ``bill_id`` Only return bills with a given bill\_id.
-  ``bill_id__in`` Accepts a pipe (\|) delimited list of bill ids.
-  ``q`` Only return bills matching the provided full text query.
-  ``search_window`` By default all bills are searched, but if a time
   window is desired the following options can be passed to
   search\_window:

   -  ``search_window=all`` Default, include all sessions.
   -  ``search_window=term`` Only bills from sessions within the current
      term.
   -  ``search_window=session`` Only bills from the current session.
   -  ``search_window=session:2009`` Only bills from the session named
      2009.
   -  ``search_window=term:2009-2011`` Only bills from the sessions in
      the 2009-2011 session.

-  ``updated_since`` Only bills updated since a provided date (provided
   in YYYY-MM-DD format)
-  ``sponsor_id`` Only bills sponsored by a given legislator id (e.g.
   'ILL000555')
-  ``subject`` Only bills categorized by Open States as belonging to
   this subject.
-  ``type`` Only bills of a given type (e.g. 'bill', 'resolution', etc.)

Additional Parameters
^^^^^^^^^^^^^^^^^^^^^

``sort`` Sort-order of results, defaults to 'last', options are:

-  first
-  last
-  signed
-  passed\_lower
-  passed\_upper
-  updated\_at
-  created\_at

See the above ``action_dates``, ``created_at``, and ``updated_at``
documentation for the meaning of these dates.

``page`` and ``per_page`` The API will not return exceedingly large responses, so it may be
necessary to use ``page`` and ``per_page`` to control the number of
results returned:

-  ``page`` Page of results, each of size ``per_page`` (defaults to 1)
-  ``per_page`` Number of results per page, is unlimited unless page is
   set, in which case it defaults to 50.

**Example:**
:ref:`openstates.org/api/v1/bills/?state=dc&q=taxi <bill-search-example>`

.. _bill-detail:

Bill Detail
~~~~~~~~~~~

This method returns the full detail object for a bill.

**Example:**
:ref:`openstates.org/api/v1/bills/ca/20092010/AB%20667/ <bill-detail-example>`


**Note:** This method has an alternate URL form:

-  ``bills/openstates_bill_id`` - e.g.
   ``openstates.org/api/v1/bills/CAB00004148/`` - allows lookup by
   bill\_id

Examples
--------

.. _bill-search-example:

Bill Search
~~~~~~~~~~~

``openstates.org/api/v1/bills/?state=dc&q=taxi``

.. code:: json

    [
     {
      "title": "\"DOC INMATE PROCESSING AND RELEASE AMENDMENT ACT OF 2012\". ",
      "created_at": "2011-07-18 04:35:16",
      "updated_at": "2012-09-14 03:49:38",
      "chamber": "upper",
      "state": "dc",
      "session": "19",
      "subjects": [],
      "type": [ "bill" ],
      "id": "DCB00001021",
      "bill_id": "B 19-0428"
     },
     {
      "title": "\"TAXICAB SERVICE IMPROVEMENT AMENDMENT ACT OF 2012\".\r\n\r\n ",
      "created_at": "2012-01-06 20:53:35",
      "updated_at": "2012-12-07 20:31:54",
      "chamber": "upper",
      "state": "dc",
      "session": "19",
      "subjects": [],
      "type": [ "bill" ],
      "id": "DCB00001501",
      "bill_id": "B 19-0630"
     },
     {
      "title": "\"FISCAL YEAR 2013 BUDGET SUPPORT ACT OF 2012\". ",
      "created_at": "2012-03-27 02:19:29",
      "updated_at": "2012-10-18 03:33:02",
      "chamber": "upper",
      "state": "dc",
      "session": "19",
      "subjects": [],
      "type": [ "bill" ],
      "id": "DCB00001892",
      "bill_id": "B 19-0743"
     },
     {
      "title": "\"FISCAL YEAR 2013 BUDGET SUPPORT EMERGENCY ACT OF 2012\". ",
      "created_at": "2012-06-08 02:51:47",
      "updated_at": "2012-09-07 03:51:01",
      "chamber": "upper",
      "state": "dc",
      "session": "19",
      "subjects": [],
      "type": [ "bill" ],
      "id": "DCB00002085",
      "bill_id": "B 19-0796"
     },
     {
      "title": "\"LEON SWAIN, JR. RECOGNITION RESOLUTION OF 2012\". ",
      "created_at": "2012-04-27 02:36:38",
      "updated_at": "2012-08-22 04:20:34",
      "chamber": "upper",
      "state": "dc",
      "session": "19",
      "subjects": [],
      "type": [ "resolution" ],
      "id": "DCB00001959",
      "bill_id": "CER 19-0218"
     },
     {
      "title": "\"WASHINGTON CONVENTION CENTER ADVISORY COMMITTEE RECOGNITION RESOLUTION OF 2011\".",
      "created_at": "2012-03-20 02:17:18",
      "updated_at": "2012-08-22 04:20:34",
      "chamber": "upper",
      "state": "dc",
      "session": "19",
      "subjects": [],
      "type": [ "resolution" ],
      "id": "DCB00001795",
      "bill_id": "CER 19-0171"
     },
     {
      "title": "\"WHEELCHAIR ACCESSIBLE TAXICABS PARITY AMENDMENT ACT OF 2011\".",
      "created_at": "2012-01-06 20:53:35",
      "updated_at": "2012-08-22 04:20:26",
      "chamber": "upper",
      "state": "dc",
      "session": "19",
      "subjects": [],
      "type": [ "bill" ],
      "id": "DCB00001506",
      "bill_id": "B 19-0635"
     },
     {
      "title": "\"FISCAL YEAR 2012 BUDGET SUPPORT ACT OF 2011\".",
      "created_at": "2011-04-06 01:53:14",
      "updated_at": "2012-10-18 03:32:58",
      "chamber": "upper",
      "state": "dc",
      "session": "19",
      "subjects": [],
      "type": [ "bill" ],
      "id": "DCB00000427",
      "bill_id": "B 19-0203"
     },
     {
      "title": "\"FISCAL YEAR 2012 BUDGET SUPPORT EMERGENCY ACT OF 2011\".\r\n ",
      "created_at": "2011-06-16 04:18:55",
      "updated_at": "2012-08-22 04:20:21",
      "chamber": "upper",
      "state": "dc",
      "session": "19",
      "subjects": [],
      "type": [ "bill" ],
      "id": "DCB00000794",
      "bill_id": "B 19-0338"
     },
     {
      "title": "\"PROFESSIONAL TAXICAB STANDARDS AND MEDALLION ESTABLISHMENT ACT OF 2011\".",
      "created_at": "2011-03-21 18:55:32",
      "updated_at": "2012-08-22 04:20:17",
      "chamber": "upper",
      "state": "dc",
      "session": "19",
      "subjects": [],
      "type": [ "bill" ],
      "id": "DCB00000339",
      "bill_id": "B 19-0172"
     }
    ]

.. _bill-detail-example:

Bill Detail
~~~~~~~~~~~

``openstates.org/api/v1/bills/ca/20092010/AB%20667/``

.. code:: json

    {
     "action_dates": {
      "passed_upper": null,
      "passed_lower": null,
      "last": "2009-08-06 00:00:00",
      "signed": null,
      "first": "2009-02-25 00:00:00"
     },
     "actions": [
      { "date": "2009-02-25 00:00:00",
       "action": "Read first time. To print.",
       "type": [ "bill:introduced", "bill:reading:1" ],
       "actor": "lower (Desk)" },
      { "date": "2009-02-26 00:00:00",
       "action": "From printer. May be heard in committee March 28.",
       "type": [ "other" ],
       "actor": "lower (Desk)" },
      { "date": "2009-03-23 00:00:00",
       "action": "Referred to Com. on HEALTH.",
       "type": [ "committee:referred" ],
       "actor": "lower (Committee CX08)" },
      { "date": "2009-04-02 00:00:00",
       "action": "From committee chair, with author's amendments: Amend, and re-refer to Com. on HEALTH. Read second time and amended.",
       "type": [ "bill:reading:2" ],
       "actor": "lower (E&E Engrossing)" },
      { "date": "2009-04-13 00:00:00",
       "action": "Re-referred to Com. on HEALTH.",
       "type": [ "committee:referred" ],
       "actor": "lower (Committee CX08)" },
      { "date": "2009-04-15 00:00:00",
       "action": "From committee: Do pass, and re-refer to Com. on B. & P. with recommendation: To Consent Calendar. Re-referred. (Ayes 19. Noes 0.) (April 14).",
       "type": [ "other" ],
       "actor": "lower (Committee)" },
      { "date": "2009-04-29 00:00:00",
       "action": "From committee: Do pass, and re-refer to Com. on APPR. with recommendation: To Consent Calendar. Re-referred. (Ayes 10. Noes 0.) (April 28).",
       "type": [ "other" ],
       "actor": "lower (Committee)" },
      { "date": "2009-05-04 00:00:00",
       "action": "From committee chair, with author's amendments: Amend, and re-refer to Com. on APPR. Read second time and amended.",
       "type": [ "bill:reading:2" ],
       "actor": "lower (E&E Engrossing)" },
      { "date": "2009-05-05 00:00:00",
       "action": "Re-referred to Com. on APPR.",
       "type": [ "committee:referred" ],
       "actor": "lower (Committee CX25)" },
      { "date": "2009-05-14 00:00:00",
       "action": "From committee: Do pass. To Consent Calendar. (May 13).",
       "type": [ "other" ],
       "actor": "lower" },
      { "date": "2009-05-18 00:00:00",
       "action": "Read second time. To Consent Calendar.",
       "type": [ "bill:reading:2" ],
       "actor": "lower" },
      { "date": "2009-05-21 00:00:00",
       "action": "Read third time, passed, and to Senate. (Ayes 77. Noes 0. Page 1628.)",
       "type": [ "other" ],
       "actor": "lower (E&E Engrossing)" },
      { "date": "2009-05-21 00:00:00",
       "action": "In Senate. Read first time. To Com. on RLS. for assignment.",
       "type": [ "bill:reading:1", "committee:referred" ],
       "actor": "upper (Rules)" },
      { "date": "2009-06-04 00:00:00",
       "action": "Referred to Com. on B., P. & E.D.",
       "type": [ "committee:referred" ],
       "actor": "upper (Committee CS42)" },
      { "date": "2009-06-22 00:00:00",
       "action": "From committee: Do pass, and re-refer to Com. on APPR. Re-referred. (Ayes 10. Noes 0.) (June 22).",
       "type": [ "other" ],
       "actor": "upper (Committee)" },
      { "date": "2009-06-29 00:00:00",
       "action": "From committee: Be placed on second reading file pursuant to Senate Rule 28.8.",
       "type": [ "other" ],
       "actor": "upper" },
      { "date": "2009-06-30 00:00:00",
       "action": "Read second time. To third reading.",
       "type": [ "bill:reading:2" ],
       "actor": "upper" },
      { "date": "2009-07-02 00:00:00",
       "action": "Ordered to Special Consent Calendar.",
       "type": [ "other" ],
       "actor": "upper" },
      { "date": "2009-07-09 00:00:00",
       "action": "Read third time, passed, and to Assembly. (Ayes 34. Noes 0. Page 1667.)",
       "type": [ "other" ],
       "actor": "upper (Desk)" },
      { "date": "2009-07-09 00:00:00",
       "action": "In Assembly. To enrollment.",
       "type": [ "other" ],
       "actor": "lower (E&E Enrollment)" },
      { "date": "2009-07-30 00:00:00",
       "action": "Enrolled and to the Governor at 2:30 p.m.",
       "type": [ "other" ],
       "actor": "executive" },
      { "date": "2009-08-05 00:00:00",
       "action": "Approved by the Governor.",
       "type": [ "other" ],
       "actor": "executive" },
      { "date": "2009-08-06 00:00:00",
       "action": "Chaptered by Secretary of State - Chapter 119, Statutes of 2009.",
       "type": [ "other" ],
       "actor": "Secretary of State" }
     ],
     "alternate_titles": [
      "An act to amend Section 104830 of, and to add Section 104762 to, the Health and Safety Code, relating to oral health."
     ],
     "bill_id": "AB 667",
     "chamber": "lower",
     "created_at": "2010-07-09 17:28:10",
     "documents": [],
     "id": "CAB00004148",
     "level": "state",
     "scraped_subjects": [ "Topical fluoride application." ],
     "session": "20092010",
     "sources": [
      { "url": "http://leginfo.legislature.ca.gov/faces/billNavClient.xhtml?bill_id=200920100AB667" }
     ],
     "sponsors": [
      { "leg_id": "CAL000044", "type": "primary", "name": "Block" }
     ],
     "state": "ca",
     "subjects": [],
     "title": "An act to amend Section 1750.1 of the Business and Professions Code, and to amend Section 104830 of, and to add Section 104762 to, the Health and Safety Code, relating to oral health.",
     "type": [ "bill", "fiscal committee" ],
     "updated_at": "2012-04-06 17:17:37",
     "versions": [
      {
       "url": "http://leginfo.legislature.ca.gov/faces/billNavClient.xhtml?bill_id=200920100AB667",
       "mimetype": "text/html", "doc_id": "CAD00040031", "name": "AB667"
      }
     ],
     "votes": [
      {
       "other_count": 6, "+threshold": "1/2",
       "other_votes": [
        { "leg_id": "CAL000014", "name": "Ashburn" },
        { "leg_id": "CAL000036", "name": "Calderon" },
        { "leg_id": "CAL000010", "name": "Corbett" },
        { "leg_id": "CAL000026", "name": "Harman" },
        { "leg_id": "CAL000021", "name": "Oropeza" },
        { "leg_id": "CAL000005", "name": "Wolk" }
       ],
       "yes_count": 34,
       "yes_votes": [
        { "leg_id": "CAL000004", "name": "Aanestad" },
        { "leg_id": "CAL000039", "name": "Alquist" },
        { "leg_id": "CAL000029", "name": "Benoit" },
        { "leg_id": "CAL000017", "name": "Cedillo" },
        { "leg_id": "CAL000011", "name": "Cogdill" },
        { "leg_id": "CAL000037", "name": "Correa" },
        { "leg_id": "CAL000001", "name": "Cox" },
        { "leg_id": "CAL000007", "name": "DeSaulnier" },
        { "leg_id": "CAL000032", "name": "Denham" },
        { "leg_id": "CAL000038", "name": "Ducheny" },
        { "leg_id": "CAL000023", "name": "Dutton" },
        { "leg_id": "CAL000033", "name": "Florez" },
        { "leg_id": "CAL000009", "name": "Hancock" },
        { "leg_id": "CAL000027", "name": "Hollingsworth" },
        { "leg_id": "CAL000022", "name": "Huff" },
        { "leg_id": "CAL000030", "name": "Kehoe" },
        { "leg_id": "CAL000003", "name": "Leno" },
        { "leg_id": "CAL000016", "name": "Liu" },
        { "leg_id": "CAL000080", "name": "Lowenthal" },
        { "leg_id": "CAL000012", "name": "Maldonado" },
        { "leg_id": null, "name": "Negrete McLeod" },
        { "leg_id": "CAL000034", "name": "Padilla" },
        { "leg_id": "CAL000018", "name": "Pavley" },
        { "leg_id": "CAL000040", "name": "Price" },
        { "leg_id": "CAL000019", "name": "Romero" },
        { "leg_id": "CAL000013", "name": "Runner" },
        { "leg_id": "CAL000031", "name": "Simitian" },
        { "leg_id": "CAL000006", "name": "Steinberg" },
        { "leg_id": "CAL000015", "name": "Strickland" },
        { "leg_id": "CAL000025", "name": "Walters" },
        { "leg_id": "CAL000002", "name": "Wiggins" },
        { "leg_id": "CAL000035", "name": "Wright" },
        { "leg_id": "CAL000028", "name": "Wyland" },
        { "leg_id": "CAL000008", "name": "Yee" }
       ],
       "no_count": 0,
       "motion": "Special Consent #12 AB667 Block By Alquist",
       "chamber": "upper",
       "state": "ca",
       "session": "20092010",
       "sources": [],
       "passed": true,
       "date": "2009-07-09 16:50:00",
       "vote_id": "CAV00009230",
       "type": "other",
       "id": "CAV00009230",
       "bill_id": "CAB00004148",
       "no_votes": []
      }
     ]
    }
