Legacy CSV Data
===============

These files are a CSV transformation of the data available in the Legacy JSON archives.  The CSV files do not reflect extra fields that the JSON files are able to include. This is due to an inherent limitation of the CSV format. If extra fields are
required you should use the JSON downloads or API.

Format
------

For each state we provide a .zip file containing several CSV files.

The following CSV files exist:

legislators.csv
~~~~~~~~~~~~~~~

Basic information on legislators.

    * `leg_id`: Open States ID for legislator
    * `full_name`
    * `first_name`
    * `middle_name`
    * `last_name`
    * `suffixes`
    * `nickname`
    * `active`: True or False
    * `state`: if active, state for current role
    * `chamber`: if active, chamber for current role
    * `district`: if active, district for current role
    * `party`: if active, party for current role
    * `votesmart_id`: ID on votesmart.org
    * `transparencydata_id`: ID on Influence Explorer
    * `photo_url`
    * `created_at`: timestamp of when object was created in our system
    * `updated_at`: timestamp of when object was last updated in our system



legislator_roles.csv
~~~~~~~~~~~~~~~~~~~~~
Links legislators (by id) to any roles that they've had.

    * `leg_id`: Open States ID for legislator
    * `type`: type of role (likely to be 'member' or 'committee member')
    * `term`: term for which legislator held role
    * `district`: if role is member, will be district served
    * `chamber`: chamber of legislature (upper/lower)
    * `state`: if role is member, will be state legislator was active in
    * `party`: if role is member, will be legislator's party
    * `committee_id`: if role is committee member, will be Open States ID for committee
    * `committee`: if role if committee member, will be name of committee (parent if subcommittee)
    * `subcommittee`: if role if committee member, will be name of subcommittee
    * `start_date`: optional, start date for role
    * `end_date`: optional, end date for role


committees.csv
~~~~~~~~~~~~~~
List of committees (see legislator_roles.csv) for membership info.

    * `id`: Open States ID for committee
    * `state`
    * `chamber`: may be upper, lower, or joint
    * `committee`: name of committee (parent if subcommitee)
    * `subcommittee`: name of subcommittee (blank if committee)
    * `parent_id`: Open States ID for parent committee if this is a subcommitee



bills.csv
~~~~~~~~~
Basic details on all bills for a given state.

    * `bill_id`: state-assigned bill id (eg. HB 271 or SR 10)
    * `state`
    * `session`
    * `chamber`: upper or lower
    * `title`: state-given title of bill
    * `created_at`: timestamp of when object was created in our system
    * `updated_at`: timestamp of when object was last updated in our system
    * `type`: bill types, pipe delimited if multi-valued
    * `subjects`: bill subjects, pipe delimited if multi-valued

bill_actions.csv
~~~~~~~~~~~~~~~~

Listing of actions taken on all bills.

    * `state`: uniquely identifies bill together with session, chamber, and bill_id
    * `session`: uniquely identifies bill together with state, chamber, and bill_id
    * `chamber`: uniquely identifies bill together with state, session, and bill_id
    * `bill_id`:  uniquely identifies bill together with session, chamber, and chamber
    * `date`: date that action took place
    * `action`: state-given name of action
    * `actor`: often upper/lower, can be other values
    * `type`: type of action

bill_sponsors.csv
~~~~~~~~~~~~~~~~~~
Listing of sponsors across all bills.

    * `state`: uniquely identifies bill together with session, chamber, and bill_id
    * `session`: uniquely identifies bill together with state, chamber, and bill_id
    * `chamber`: uniquely identifies bill together with state, session, and bill_id
    * `bill_id`:  uniquely identifies bill together with session, chamber, and chamber
    * `type`: type of sponsor (primary/cosponsor/etc)
    * `name`: given name of sponsor
    * `leg_id`: Open States Legislator ID for sponsor


bill_votes.csv
~~~~~~~~~~~~~~

A listing of all votes on all bills.

    * `state`: uniquely identifies bill together with session, chamber, and bill_id
    * `session`: uniquely identifies bill together with state, chamber, and bill_id
    * `chamber`: uniquely identifies bill together with state, session, and bill_id
    * `bill_id`:  uniquely identifies bill together with session, chamber, and chamber
    * `vote_id`: Open States Vote ID for this vote
    * `vote_chamber`: chamber vote took place in (upper/lower)
    * `motion`
    * `date`
    * `type`: type of vote
    * `yes_count`: number of 'yes' votes
    * `no_count`: number of 'no' votes
    * `other_count`: number of 'other' votes


bill_legislator_votes.csv
~~~~~~~~~~~~~~~~~~~~~~~~~
Pairing of vote ids with how a specific legislator voted.

    * `vote_id`: Open States Vote ID, matches `vote_id` from bill_votes.csv
    * `leg_id`: Open States Legislator ID
    * `name`: name of legislator
    * `vote`: yes/no/other


Downloads
---------

Latest Update: **November 3, 2018**

The latest legacy CSV files can be obtained from a URL in the format: 
``https://data.openstates.org/legacy/csv/ak.zip``

Just substitute the postal code of your state for the `ak` portion of the URL.


* `Alabama <https://data.openstates.org/legacy/csv/al.zip>`_
* `Alaska <https://data.openstates.org/legacy/csv/ak.zip>`_
* `Arizona <https://data.openstates.org/legacy/csv/az.zip>`_
* `Arkansas <https://data.openstates.org/legacy/csv/ar.zip>`_
* `California <https://data.openstates.org/legacy/csv/ca.zip>`_
* `Colorado <https://data.openstates.org/legacy/csv/co.zip>`_
* `Connecticut <https://data.openstates.org/legacy/csv/ct.zip>`_
* `Delaware <https://data.openstates.org/legacy/csv/de.zip>`_
* `District of Columbia <https://data.openstates.org/legacy/csv/dc.zip>`_
* `Florida <https://data.openstates.org/legacy/csv/fl.zip>`_
* `Georgia <https://data.openstates.org/legacy/csv/ga.zip>`_
* `Hawaii <https://data.openstates.org/legacy/csv/hi.zip>`_
* `Idaho <https://data.openstates.org/legacy/csv/id.zip>`_
* `Illinois <https://data.openstates.org/legacy/csv/il.zip>`_
* `Indiana <https://data.openstates.org/legacy/csv/in.zip>`_
* `Iowa <https://data.openstates.org/legacy/csv/ia.zip>`_
* `Kansas <https://data.openstates.org/legacy/csv/ks.zip>`_
* `Kentucky <https://data.openstates.org/legacy/csv/ky.zip>`_
* `Louisiana <https://data.openstates.org/legacy/csv/la.zip>`_
* `Maine <https://data.openstates.org/legacy/csv/me.zip>`_
* `Maryland <https://data.openstates.org/legacy/csv/md.zip>`_
* `Massachusetts <https://data.openstates.org/legacy/csv/ma.zip>`_
* `Michigan <https://data.openstates.org/legacy/csv/mi.zip>`_
* `Minnesota <https://data.openstates.org/legacy/csv/mn.zip>`_
* `Mississippi <https://data.openstates.org/legacy/csv/ms.zip>`_
* `Missouri <https://data.openstates.org/legacy/csv/mo.zip>`_
* `Montana <https://data.openstates.org/legacy/csv/mt.zip>`_
* `Nebraska <https://data.openstates.org/legacy/csv/ne.zip>`_
* `Nevada <https://data.openstates.org/legacy/csv/nv.zip>`_
* `New Hampshire <https://data.openstates.org/legacy/csv/nh.zip>`_
* `New Jersey <https://data.openstates.org/legacy/csv/nj.zip>`_
* `New Mexico <https://data.openstates.org/legacy/csv/nm.zip>`_
* `New York <https://data.openstates.org/legacy/csv/ny.zip>`_
* `North Carolina <https://data.openstates.org/legacy/csv/nc.zip>`_
* `North Dakota <https://data.openstates.org/legacy/csv/nd.zip>`_
* `Ohio <https://data.openstates.org/legacy/csv/oh.zip>`_
* `Oklahoma <https://data.openstates.org/legacy/csv/ok.zip>`_
* `Oregon <https://data.openstates.org/legacy/csv/or.zip>`_
* `Pennsylvania <https://data.openstates.org/legacy/csv/pa.zip>`_
* `Puerto Rico <https://data.openstates.org/legacy/csv/pr.zip>`_
* `Rhode Island <https://data.openstates.org/legacy/csv/ri.zip>`_
* `South Carolina <https://data.openstates.org/legacy/csv/sc.zip>`_
* `South Dakota <https://data.openstates.org/legacy/csv/sd.zip>`_
* `Tennessee <https://data.openstates.org/legacy/csv/tn.zip>`_
* `Texas <https://data.openstates.org/legacy/csv/tx.zip>`_
* `Utah <https://data.openstates.org/legacy/csv/ut.zip>`_
* `Vermont <https://data.openstates.org/legacy/csv/vt.zip>`_
* `Virginia <https://data.openstates.org/legacy/csv/va.zip>`_
* `Washington <https://data.openstates.org/legacy/csv/wa.zip>`_
* `West Virginia <https://data.openstates.org/legacy/csv/wv.zip>`_
* `Wisconsin <https://data.openstates.org/legacy/csv/wi.zip>`_
* `Wyoming <https://data.openstates.org/legacy/csv/wy.zip>`_
