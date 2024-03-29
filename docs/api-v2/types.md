# Data Types

Starting at the base nodes, data in the API is represented as
interconnected nodes of various types. This page provides an overview of
the nodes.

Another good way to get acquainted with the layout is to use the
[GraphiQL browser](https://openstates.org/graphql) (click Docs in the
upper right corner).

## Jurisdictions & Sessions

### JurisdictionNode

A Jurisdiction is the [Open Civic Data](https://opencivicdata.org) term
for the top level divisions of the US. Open States is comprised of 52
jurisdictions, one for each state, and two more for D.C. and Puerto
Rico.

Each JurisdictionNode has the following attributes & nodes available:

-   `id` - ocd-jurisdiction identifier, these are permanent identifiers
    assigned to each Jurisdiction

-   `name` - human-readable name for the jurisdiction (e.g. Kansas)

-   `url` - URL of official website for jurisdiction

-   `featureFlags` - reserved for future use

-   `legislativeSessions` - Paginated list (see `pagination`) of
    [LegislativeSessionNode](#legislativesessionnode) belonging to this
    jurisdiction's legislature.

-   

    `organizations` - Paginated list of [OrganizationNode](#organizationnode) belonging to this jurisdiction.

    :   -   it is also possible to filter the list of children using the
            `classification` parameter

-   `lastScrapedAt` - Time when last scrape finished.

See also: [Open Civic Data Jurisdiction
reference](http://docs.opencivicdata.org/en/latest/data/jurisdiction.html)

### LegislativeSessionNode

A legislative session is a convening of the legislature, either a
primary or special session.

Each LegislativeSessionNode has the following attributes and nodes
available:

-   `jurisdiction` - [JurisdictionNode](#jurisdictionnode) which this
    session belongs to.
-   `identifier` - short identifier by which this session is referred to
    (e.g. 2017s1 or 121)
-   `name` - formal name of session (e.g. "2017 Special Session #1"
    or "121st Session"
-   `classification` - "primary" or "special"
-   `startDate` - start date of session if known
-   `endDate` - end date of session if known

### DivisionNode

Divisions represent particular geopolitical boundaries. Divisions exist
for states as well as their component districts and are tied closely to
political geographies.

-   `id` - [Open Civic Data Division
    ID](http://docs.opencivicdata.org/en/latest/ocdids.html#division-ids)
-   `name` - human-readable name for the division
-   `redirect` - link to another DivisionNode, only present if division
    has been replaced
-   `country` - country code (will be "us") for all Open States
    divisions
-   `createdAt` - date at which this object was created in our system
-   `updatedAt` - date at which this object was last updated in our
    system
-   `extras` - JSON string with optional additional information about
    the object

## People & Organizations

### PersonNode

People, typically legislators and their associated metadata.

Note that most fields are optional beyond name as often we don't have a
reliable given/family name or birthDate for instance.

-   `id` - [Open Civic Data Person
    ID](http://docs.opencivicdata.org/en/latest/ocdids.html)

-   `name` - primary name for the person

-   `sortName` - alternate name to sort by (if known)

-   `familyName` - hereditary name, essentially a "last name" (if
    known)

-   `givenName` - essentially a "first name" (if known)

-   `image` - full URL to official image of legislator

-   `birthDate` - see `date-format`

-   `deathDate` - see `date-format`

-   `identifiers` - list of other known identifiers,
    [IdentifierNode](#identifiernode)

-   `otherNames` - list of other known names, [NameNode](#namenode)

-   `links` - official URLs relating to this person,
    [LinkNode](#linknode)

-   `contactDetails` - ways to contact this person (via email, phone,
    etc.), [contactdetailnode](#contactdetailnode)

-   

    `currentMemberships` - currently active memberships [MembershipNode](#membershipnode)

    :   -   can be filtered with the `classification` parameter to only
            get memberships to certain types of
            [OrganizationNode](#organizationnode)

-   

    `oldMemberships` - inactive memberships [MembershipNode](#membershipnode)

    :   -   can be filtered with the `classification` parameter to only
            get memberships to certain types of
            [OrganizationNode](#organizationnode)

-   `sources` - URLs which were used in compiling Open States' information on this subject, [LinkNode]

-   `createdAt` - date at which this object was created in our system

-   `updatedAt` - date at which this object was last updated in our
    system

-   `extras` - JSON string with optional additional information about
    the object

See also:

-   [Popolo's person](http://popoloproject.com/specs/person.html)
-   [Open Civic Data OCDEP 5](http://docs.opencivicdata.org/en/latest/proposals/0005.html)

### OrganizationNode

Organizations that comprise the state legislatures and their associated
metdata.

A typical bicameral legislature is comprised of a top-level organization
(classification=legislature), two chambers (classification=upper &
lower), and any number of committees (classification=committee).

Each Organization is comprised of the following attributes and nodes:

-   `id` - [Open Civic Data Organization ID](http://docs.opencivicdata.org/en/latest/ocdids.html)

-   `name` - primary name for the person

-   `image` - full URL to official image for organization

-   `classification` - the type of organization as described above

-   `foundingDate` - see `date-format`

-   `dissolutionDate` - see `date-format`

-   `parent` - parent OrganizationNode if one exists

-   

    `children` - paginated list of child OrganizationNode objects

    :   -   it is also possible to filter the list of children using the
            `classification` parameter

-   `currentMemberships` - list of all current members of this
    Organization

-   `identifiers` - list of other known identifiers for this
    organization, [IdentifierNode](#identifiernode)

-   `otherNames` - list of other known names for this organization,
    [NameNode](#namenode)

-   `links` - official URLs relating to this person,
    [LinkNode](#linknode)

-   `sources` - URLs which were used in compiling Open States'
    information on this subject, [LinkNode]

-   `createdAt` - date at which this object was created in our system

-   `updatedAt` - date at which this object was last updated in our
    system

-   `extras` - JSON string with optional additional information about
    the object

See also:

-   [Popolo's
    organization](http://popoloproject.com/specs/organization.html)
-   [Open Civic Data OCDEP
    5](http://docs.opencivicdata.org/en/latest/proposals/0005.html)

### MembershipNode

A MembershipNode represents a connection between a
[personnode](#personnode) and a [organizationnode](#organizationnode). A
membership may optionally also reference a particular
[postnode](#postnode), such as a particular seat within a given chamber.

Each membership has the following attributes and nodes:

-   `id` - [Open Civic Data Membership
    ID](http://docs.opencivicdata.org/en/latest/ocdids.html)
-   `personName` the raw name of the person that the membership
    describes (see `name-matching`
-   `person` - [personnode](#personnode)
-   `organization` - [organizationnode](#organizationnode)
-   `post` - [postnode](#postnode)
-   `label` - label assigned to this membership
-   `role` - role fulfilled by this membership
-   `startDate` - start date of membership if known
-   `endDate` - end date of membership if known
-   `createdAt` - date at which this object was created in our system
-   `updatedAt` - date at which this object was last updated in our
    system
-   `extras` - JSON string with optional additional information about
    the object

See also:

-   [Popolo's
    membership](http://popoloproject.com/specs/membership.html)
-   [Open Civic Data OCDEP
    5](http://docs.opencivicdata.org/en/latest/proposals/0005.html)

### PostNode

A PostNode represents a given position within an organization. The most
common example would be a seat such as Maryland's 4th House Seat.

It is worth noting that some seats can have multiple active memberships
at once, as noted in `maximumMemberships`.

Each post has the following attributes and nodes:

-   `id` - [Open Civic Data Post
    ID](http://docs.opencivicdata.org/en/latest/ocdids.html)
-   `label` - label assigned to this post (e.g. 3)
-   `role` - role fulfilled by this membership (e.g. 'member')
-   `division` - related [divisionnode](#divisionnode) if this role has
    a relevant division
-   `startDate` - start date of membership if known
-   `endDate` - end date of membership if known
-   `maximumMemberships` - typically 1, but set higher in the case of
    multi-member districts
-   `createdAt` - date at which this object was created in our system
-   `updatedAt` - date at which this object was last updated in our
    system
-   `extras` - JSON string with optional additional information about
    the object

See also:

-   [Popolo's post](http://popoloproject.com/specs/post.html)
-   [Open Civic Data OCDEP
    5](http://docs.opencivicdata.org/en/latest/proposals/0005.html)

## Bills & Votes

### BillNode

A BillNode represents any legislative instrument such as a bill or
resolution.

Each node has the following attributes and nodes available:

-   `id` - Internal ocd-bill identifier for this bill.
-   `legislativeSession` - link to
    [LegislativeSessionNode](#legislativesessionnode) this bill is from
-   `identifier` - primary identifier for this bill (e.g. HB 264)
-   `title` - primary title for this bill
-   `fromOrganization` - organization (typically upper or lower chamber)
    primarily associated with this bill
-   `classification` - list of one or more bill types such as "bill"
    or "resolution"
-   `subject` - list of zero or more subjects assigned by the state
-   `abstracts` - list of abstracts provided by the state,
    [BillAbstractNode](#billabstractnode)
-   `otherTitles` - list of other titles provided by the state,
    [BillTitleNode](#billtitlenode)
-   `otherIdentifiers` - list of other identifiers provided by the
    state, [BillIdentifierNode](#billidentifiernode)
-   `actions` - list of actions (such as introduction, amendment,
    passage, etc.) that have been taken on the bill,
    [BillActionNode](#billactionnode)
-   `sponsorships` - list of bill sponsors,
    [BillSponsorshipNode](#billsponsorshipnode)
-   `relatedBills` - list of related bills as provided by the state,
    [RelatedBillNode](#relatedbillnode)
-   `versions` - list of bill versions as provided by the state,
    [BillDocumentNode](#billdocumentnode)
-   `documents` - list of related documents (e.g. legal analysis, fiscal
    notes, etc.) as provided by the state,
    [BillDocumentNode](#billdocumentnode)
-   `votes` - paginated list of [VoteEventNode](#voteeventnode) related
    to the bill
-   `sources` - URLs which were used in compiling Open States'
    information on this subject, [linknode](#linknode)
-   `openstatesUrl` - URL to bill page on OpenStates.org
-   `createdAt` - date at which this object was created in our system
-   `updatedAt` - date at which this object was last updated in our
    system
-   `extras` - JSON string with optional additional information about
    the object

### BillAbstractNode

Represents an official abstract for a bill, each BillAbstractNode has
the following attributes:

-   `abstract` - the abstract itself
-   `note` - optional note about origin/purpose of abstract
-   `date` - optional date associated with abstract

### BillTitleNode

Represents an alternate title for a bill, each BillTitleNode has the
following attributes:

-   `title` - the alternate title
-   `note` - optional note about origin/purpose of this title

### BillIdentifierNode

Represents an alternate identifier for a bill, each BillIdentifierNode
has the following attributes:

-   `identifier` - the alternate identifier
-   `scheme` - a name for the identifier scheme
-   `note` - optional note about origin/purpose of this identifier

### BillActionNode

Represents an action taken on a bill, each BillActionNode has the
following attributes and nodes:

-   `organization` - [OrganizationNode](#organizationnode) where this
    action originated, will typically be either upper or lower chamber,
    or perhaps legislature as a whole.
-   `description` - text describing the action as provided by the
    jurisdiction.
-   `date` - date action took place (see `date-format`)
-   `classification` - list of zero or more normalized action types (see
    `action-categorization`)
-   `order` - integer by which actions can be sorted, not intended for
    display purposes
-   `extras` - JSON string providing extra information about this action
-   `vote` - if there is a known associated vote, pointer to the
    relevant [VoteEventNode](#voteeventnode)
-   `relatedEntities` - a list of
    [RelatedEntityNode](#relatedentitynode) with known entities
    referenced in this action

### RelatedEntityNode

Represents an entity that is related to a
[BillActionNode](#billactionnode).

-   `name` - raw (source-provided) name of entity
-   `entityType` - either organization or person
-   `organization` - if `entityType` is 'organization', the resolved
    [OrganizationNode](#organizationnode)
-   `person` - if `entityType` is 'person', the resolved
    [PersonNode](#personnode)

See `name-matching` for details on how `name` relates to `organiation` and `person`.

### BillSponsorshipNode

Represents a sponsor of a bill.

-   `name` - raw (source-provided) name of sponsoring person or
    organization
-   `entityType` - either organization or person
-   `organization` - if `entityType` is 'organization', the resolved
    [OrganizationNode](#organizationnode)
-   `person` - if `entityType` is 'person', the resolved
    [PersonNode](#personnode)
-   `primary` - boolean, true if sponsorship is considered by the
    jurisdiction to be "primary" (note: in many states multiple
    primary sponsors may exist)
-   `classification` - jurisdiction-provided type of sponsorship, such
    as "author" or "cosponsor". These meanings typically vary across
    states, which is why we provide `primary` as a sort of indicator of
    the degree of sponsorship indicated.

See `name-matching` for details on how `name` relates to `organiation` and `person`.

### RelatedBillNode

Represents relationships between bills.

-   `identifier` - identifier of related bill (e.g. SB 401)
-   `legislativeSession` - identifier of related session (in same jurisdiction)
-   `relationType` - type of relationship such as "companion", "prior-session", "replaced-by", or "replaces"
-   `relatedBill` - if the related bill is found to exist in our data, link to the [BillNode](#billnode)

### BillDocumentNode

Representation of `documents` and `versions` on bills. A given document
can have multiple links representing different manifestations (e.g.
HTML, PDF, DOC) of the same content.

-   `note` - note describing the purpose of the document or version
    (e.g. Final Printing)
-   `date` - optional date associated with the document
-   `links` - list of one or more `MimetypeLinkNode` with actual URLs to
    bills.

### MimetypeLinkNode

Represents a single manifestation of a particular document.

-   `mediaType` - media type (aka MIME type) such as application/pdf or
    text/html
-   `url` - URL to official copy of the bill
-   `text` - text describing this particular manifestation (e.g. PDF)

### VoteEventNode

Represents a vote taken on a bill.

-   `id` - Internal ocd-vote identifier for this bill.
-   `identifier` - Identifier used by jurisdiction to uniquely identify
    the vote.
-   `motionText` - Text of the motion being voted upon, such as "motion
    to pass the bill as amended."
-   `motionClassification` - List with zero or more classifications for
    this motion, such as "passage" or "veto-override"
-   `startDate` - Date on which the vote took place. (see
    `date-format`
-   `result` - Outcome of the vote, 'pass' or 'fail'.
-   `organization` - Related [OrganizationNode](#organizationnode) where
    vote took place.
-   `billAction` - Optional linked [BillActionNode](#billactionnode).
-   `votes` - List of [PersonVoteNode](#personvotenode) for each
    individual's recorded vote. (May not be present depending on
    jurisdiction.)
-   `counts` - List of [VoteCountNode](#votecountnode) with sums of each
    outcome (e.g. yea/nay/abstain).
-   `sources` - URLs which were used in compiling Open States'
    information on this subject, [LinkNode]
-   `createdAt` - date at which this object was created in our system
-   `updatedAt` - date at which this object was last updated in our
    system
-   `extras` - JSON string with optional additional information about
    the object

See also: [Open Civic Data vote
format](http://docs.opencivicdata.org/en/latest/data/vote.html).

### PersonVoteNode

Represents an individual person's vote (e.g. yea or nay) on a given
bill.

-   `option` - Option chosen by this individual. (yea, nay, abstain,
    other, etc.)
-   `voterName` - Raw name of voter as provided by jurisdiction.
-   `voter` - Resolved [PersonNode](#personnode) representing voter.
    (See `name-matching`
-   `note` - Note attached to this vote, sometimes used for explaining
    an "other" vote.

### VoteCountNode

Represents the sum of votes for a given `option`.

-   `option` - Option in question. (yea, nay, abstain, other, etc.)
-   `value` - Number of individuals voting this way.

## Other Nodes

### IdentifierNode

Represents an alternate identifier, each with the following attributes:

-   `identifier` - the alternate identifier
-   `scheme` - a name for the identifier scheme

### NameNode

Represents an alterante name, each with the following attributes:

-   `name` - the alternate name
-   `note` - note about usage/origin of this alternate name
-   `startDate` - date at which this name began being valid (blank if
    unknown)
-   `endDate` - date at which this name stopped being valid (blank if
    unknown or still active)

### LinkNode

Represents a single link associated with a person or used as a source.

-   `url` - URL
-   `text` - text describing the use of this particular URL

### ContactDetailNode

Used to represent a contact method for a given person.

-   `type` - type of contact detail (e.g. voice, email, address, etc.)
-   `value` - actual phone number, email address, etc.
-   `note` - used to group contact data by location (e.g. Home Address,
    Office Address)
-   `label` - human-readable label for this contact detail
