Data Types
==========

Starting at the base nodes, data in the API is represented as interconnected nodes of various types.  This page provides an overview of the nodes.  Another good way to get aquainted with the layout is to use the `GraphiQL browser <http://alpha.openstates.org/graphql>`_ (click Docs in the upper right corner).

Jurisdictions & Sessions
------------------------

.. _JurisdictionNode:

JurisdictionNode
~~~~~~~~~~~~~~~~

A Jurisdiction is the `Open Civic Data <https://opencivicdata.org>`_ term for the top level divisions of the US.  Open States is comprised of 52 jurisdictions, one for each state, and two more for D.C. and Puerto Rico.

Each JurisdictionNode has the following attributes & nodes available:

* ``id`` - ocd-jurisdiction identifier, these are permanent identifiers assigned to each Jurisdiction
* ``name`` - human-readable name for the jurisdiction (e.g. Kansas)
* ``url`` - URL of official website for jurisdiction
* ``featureFlags`` - reserved for future use
* ``legislativeSessions`` - Paginated list (see :ref:`pagination`) of `LegislativeSessionNode`_ belonging to this jurisdiction's legislature.
* ``organizations`` - Paginated list of `OrganizationNode`_ belonging to this jurisdiction.

See also: `Open Civic Data Jurisdiction reference <http://docs.opencivicdata.org/en/latest/data/jurisdiction.html>`_
    
.. _LegislativeSessionNode:

LegislativeSessionNode
~~~~~~~~~~~~~~~~~~~~~~

A legislative session is a convening of the legislature, either a primary or special session.

Each LegislativeSessionNode has the following attributes and nodes available:

* ``jurisdiction`` - `JurisdictionNode`_ which this session belongs to.
* ``identifier`` - short identifier by which this session is referred to (e.g. 2017s1 or 121)
* ``name`` - formal name of session (e.g. "2017 Special Session #1" or "121st Session"
* ``classification`` - "primary" or "special"
* ``startDate`` - start date of session if known
* ``endDate`` - end date of session if known

DivisionNode
~~~~~~~~~~~~

Divisions represent particular geopolitical boundaries.  Divisions exist for states as well as their component districts and are tied closely to political geographies.

* ``id`` - `Open Civic Data Division ID <http://docs.opencivicdata.org/en/latest/ocdids.html#division-ids>`_ 
* ``name`` - human-readable name for the division
* ``redirect`` - link to another DivisionNode, only present if division has been replaced
* ``country`` - country code (will be "us") for all Open States divisions
* ``createdAt`` - date at which this object was created in our system
* ``updatedAt`` - date at which this object was last updated in our system
* ``extras`` - JSON string with optional additional information about the object


People & Organizations
----------------------

.. _PersonNode:

PersonNode
~~~~~~~~~~

People, typically legislators and their associated metadata.

Note that most fields are optional beyond name as often we don't have a reliable given/family name or birthDate for instance.

* ``id`` - `Open Civic Data Person ID <http://docs.opencivicdata.org/en/latest/ocdids.html>`_ 
* ``name`` - primary name for the person
* ``sortName`` - alternate name to sort by (if known)
* ``familyName`` - hereditary name, essentially a "last name" (if known)
* ``givenName`` - essentially a "first name" (if known)
* ``image`` - full URL to official image of legislator
* ``birthDate`` - see :ref:`date-format`
* ``deathDate`` - see :ref:`date-format`
* ``identifiers`` - list of other known identifiers, `IdentifierNode`_
* ``otherNames`` - list of other known names, `NameNode`_
* ``links`` - official URLs relating to this person, `LinkNode`_
* ``contactDetails`` - ways to contact this person (via email, phone, etc.), `contactdetailnode`_
* ``currentMemberships`` - currently active memberships `MembershipNode`_
    * can be filtered with the ``classification`` parameter to only get memberships to certain types of `OrganizationNode`_
* ``sources`` - URLs which were used in compiling Open States' information on this subject, `LinkNode`
* ``createdAt`` - date at which this object was created in our system
* ``updatedAt`` - date at which this object was last updated in our system
* ``extras`` - JSON string with optional additional information about the object

See also:

* `Popolo's person <http://popoloproject.com/specs/person.html>`_
* `Open Civic Data OCDEP 5 <http://docs.opencivicdata.org/en/latest/proposals/0005.html>`_


.. _OrganizationNode:

OrganizationNode
~~~~~~~~~~~~~~~~

Organizations that comprise the state legislatures and their associated metdata. 

A typical bicameral legislature is comprised of a top-level organization (classification=legislature), two chambers (classification=upper & lower), and any number of committees (classification=committee). 

Each Organization is comprised of the following attributes and nodes:

* ``id`` - `Open Civic Data Organization ID <http://docs.opencivicdata.org/en/latest/ocdids.html>`_ 
* ``name`` - primary name for the person
* ``image`` - full URL to official image for organization
* ``classification`` - the type of organization as described above
* ``foundingDate`` - see :ref:`date-format`
* ``dissolutionDate`` - see :ref:`date-format`
* ``parent`` - parent OrganizationNode if one exists
* ``children`` - paginated list of child OrganizationNode objects
    * it is also possible to filter the list of children using the ``classification`` parameter
* ``identifiers`` - list of other known identifiers for this organization, `IdentifierNode`_
* ``otherNames`` - list of other known names for this organization, `NameNode`_
* ``links`` - official URLs relating to this person, `LinkNode`_
* ``sources`` - URLs which were used in compiling Open States' information on this subject, `LinkNode`
* ``createdAt`` - date at which this object was created in our system
* ``updatedAt`` - date at which this object was last updated in our system
* ``extras`` - JSON string with optional additional information about the object


See also:

* `Popolo's organization <http://popoloproject.com/specs/organization.html>`_
* `Open Civic Data OCDEP 5 <http://docs.opencivicdata.org/en/latest/proposals/0005.html>`_


MembershipNode
~~~~~~~~~~~~~~

A MembershipNode represents a connection between a `personnode`_ and a `organizationnode`_.  A membership may optionally also reference a particular `postnode`_, such as a particular seat within a given chamber.

Each membership has the following attributes and nodes:

* ``id`` - `Open Civic Data Membership ID <http://docs.opencivicdata.org/en/latest/ocdids.html>`_ 
* ``personName`` the raw name of the person that the membership describes (see :ref:`name-matching`)
* ``person`` - `personnode`_
* ``organization`` - `organizationnode`_
* ``post`` - `postnode`_
* ``label`` - label assigned to this membership
* ``role`` - role fulfilled by this membership
* ``startDate`` - start date of membership if known
* ``endDate`` - end date of membership if known
* ``createdAt`` - date at which this object was created in our system
* ``updatedAt`` - date at which this object was last updated in our system
* ``extras`` - JSON string with optional additional information about the object


See also:

* `Popolo's membership <http://popoloproject.com/specs/membership.html>`_
* `Open Civic Data OCDEP 5 <http://docs.opencivicdata.org/en/latest/proposals/0005.html>`_


PostNode
~~~~~~~~

A PostNode represents a given position within an organization.  The most common example would be a seat such as Maryland's 4th House Seat.

It is worth noting that some seats can have multiple active memberships at once, as noted in ``maximumMemberships``.

Each post has the following attributes and nodes:

* ``id`` - `Open Civic Data Post ID <http://docs.opencivicdata.org/en/latest/ocdids.html>`_ 
* ``label`` - label assigned to this post (e.g. 3)
* ``role`` - role fulfilled by this membership (e.g. 'member')
* ``division`` - related `divisionnode`_ if this role has a relevant division
* ``startDate`` - start date of membership if known
* ``endDate`` - end date of membership if known
* ``maximumMemberships`` - typically 1, but set higher in the case of multi-member districts
* ``createdAt`` - date at which this object was created in our system
* ``updatedAt`` - date at which this object was last updated in our system
* ``extras`` - JSON string with optional additional information about the object

See also:

* `Popolo's post <http://popoloproject.com/specs/post.html>`_
* `Open Civic Data OCDEP 5 <http://docs.opencivicdata.org/en/latest/proposals/0005.html>`_


Bills & Votes
-------------

.. _BillNode:

BillNode
~~~~~~~~

A BillNode represents any legislative instrument such as a bill or resolution.

Each node has the following attributes and nodes available:

* ``id`` - Internal ocd-bill identifier for this bill.
* ``legislativeSession`` - link to `LegislativeSessionNode`_ this bill is from
* ``identifier`` - primary identifier for this bill (e.g. HB 264)
* ``title`` - primary title for this bill
* ``fromOrganization`` - organization (typically upper or lower chamber) primarily associated with this bill
* ``classification`` - list of one or more bill types such as "bill" or "resolution"
* ``subject`` - list of zero or more subjects assigned by the state
* ``abstracts`` - list of abstracts provided by the state, `BillAbstractNode`_
* ``otherTitles`` - list of other titles provided by the state, `BillTitleNode`_
* ``otherIdentifiers`` - list of other identifiers provided by the state, `BillIdentifierNode`_
* ``actions`` - list of actions (such as introduction, amendment, passage, etc.) that have been taken on the bill, `BillActionNode`_
* ``sponsorships`` - list of bill sponsors, `BillSponsorshipNode`_
* ``relatedBills`` - list of related bills as provided by the state, `RelatedBillNode`_
* ``versions`` - list of bill versions as provided by the state, `BillDocumentNode`_
* ``documents`` - list of related documents (e.g. legal analysis, fiscal notes, etc.) as provided by the state, `BillDocumentNode`_
* ``votes`` - paginated list of `VoteEventNode`_ related to the bill
* ``sources`` - URLs which were used in compiling Open States' information on this subject, `linknode`_
* ``createdAt`` - date at which this object was created in our system
* ``updatedAt`` - date at which this object was last updated in our system
* ``extras`` - JSON string with optional additional information about the object


BillAbstractNode
~~~~~~~~~~~~~~~~

BillTitleNode
~~~~~~~~~~~~~

BillIdentifierNode
~~~~~~~~~~~~~~~~~~

BillActionNode
~~~~~~~~~~~~~~

RelatedEntityNode
~~~~~~~~~~~~~~~~~

BillSponsorshipNode
~~~~~~~~~~~~~~~~~~~

RelatedBillNode
~~~~~~~~~~~~~~~

BillDocumentNode
~~~~~~~~~~~~~~~~

MimetypeLinkNode
~~~~~~~~~~~~~~~~

VoteEventNode
~~~~~~~~~~~~~

PersonVoteNode
~~~~~~~~~~~~~~

VoteCountNode
~~~~~~~~~~~~~~


Assorted Nodes
--------------

IdentifierNode
~~~~~~~~~~~~~~

NameNode
~~~~~~~~

LinkNode
~~~~~~~~

ContactDetailNode
~~~~~~~~~~~~~~~~~

