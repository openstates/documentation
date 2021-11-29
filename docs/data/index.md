# Understanding the Data

Open States data adheres to a schema that has evolved over our 11+ years
of working with legislative data. Our goal is to provide as much
uniformity across states as possible while still allowing for the wide
diversity of legislative processes between the states.

These docs both catalog the schema and attempt to explain some of those
choices, particularly where they might be surprising.

## Main Concepts

The main concepts are defined below:

Jurisdiction
   Essentially just another word for "State" in our context. (Includes
   DC and Puerto Rico.)

Session (aka LegislativeSession)
   A period of time in a legislature where the same members serve
   together, typically punctuated by elections. All bills in a session
   will be uniquely numbered. (e.g. HB 1 in the 2017 session is
   typically not the same bill as in the 2019 session)

Bill
   Represents all types of legislation whether it is a bill, resolution,
   etc.

Vote (aka VoteEvent)
   A vote among members of the legislature, typically an entire chamber
   but can also be a committee vote.

Person (aka Legislator)
   Any person that is associated with the legislature.

Organization
   A generic term used to represent a few different concepts:
   legislatures, chambers, committees, and political parties.

Post
   A particular role within an organization, typically used to represent
   a seat in the legislature. (e.g. the District 4 post in the North
   Carolina Senate Organization)

Membership
   Ties a Person to a Post for a duration of time.

(You'll notice these concepts mostly correspond to the v2 GraphQL root nodes.)

