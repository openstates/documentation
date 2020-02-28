.. _RootNodes: 

Root Nodes
==========

As seen in the introduction, when constructing a query you will start your query
at one (or more) root nodes.  The following root nodes are available:

jurisdictions
-------------

Get a list of all jurisdictions.

This will return a list of :ref:`JurisdictionNode` objects, one for each state (plus Puerto Rico and DC).

**Pagination**: This endpoint accepts the usual :ref:`pagination` parameters, but pagination is not required.


people
------

Get a list of all people matching certain criteria.

This will return a list of :ref:`PersonNode` objects, one for each person matching your query.

**Pagination**: This endpoint accepts the usual :ref:`pagination` parameters, and you must limit your results to no more than 100 using either the "first" or "last" parameter.

Parameters:
~~~~~~~~~~~

``name``
    Limit response to people who's name contains the provided string.

    Includes partial matches & case-insensitive matches.

``memberOf``
    Limit response to people that have a currently active membership record for an organization.  The value passed to memberOf can be an ocd-organization ID or a name (e.g. 'Republican' or 'Nebraska Legislature').

``everMemberOf``
    Limit response to people that have any recorded membership record for an organization.  Operates as a superset of memberOf.
    
    Specifying ``memberOf`` and ``everMemberOf`` in the same query is invalid.

``district``
    When specifying either memberOf or everMemberOf, limits to people who's membership represented the district with a given label. 
    (e.g. memberOf: "Nebraska Legislature", district: "7")

    Specifying ``district`` without ``memberOf`` or ``everMemberOf`` is invalid.

``latitude`` and ``longitude``
    Limit to people that are currently representing the district(s) containing the point specified by the provided coordinates.

    Must be specified together.


.. _bills-root:

bills
-----

Get a list of all bills matching certain criteria.

This will return a list of :ref:`BillNode` objects, one for each person matching your query.

**Pagination**: This endpoint accepts the usual :ref:`pagination` parameters, and you must limit your results to no more than 100 using either the "first" or "last" parameter.

Parameters:
~~~~~~~~~~~

``jurisdiction``
    Limit to bills associated with given jurisdiction, parameter can either be a human-readable jurisdiction name or an ocd-jurisdiction ID. 

``chamber``
    Limit to bills originating in a given chamber.  (e.g. upper, lower, legislature)

``session``
    Limit to bills originating in a given legislative session.  This parameter should be the desired session's ``identifier``.  (See :ref:`LegislativeSessionNode`).

``classification``
    Limit to bills with a given classification (e.g. "bill" or "resolution")

``subject``
    Limit to bills with a given subject (e.g. "Agriculture")

``searchQuery``
    Limit to bills that contain a given term.  (Experimental until 2020!)

``updatedSince``
    Limit to bills that have had data updated since a given time (UTC).

    Time should be in the format YYYY-MM-DD[THH:MM:SS].

``actionsSince``
    Limit to bills that have had actions since a given time (UTC).

    Time should be in the format YYYY-MM-DD.


jurisdiction
------------

Look up a single jurisdiction by name or ID.

This will return a single :ref:`JurisdictionNode` object with the provided name or ID parameter.

Parameters:
~~~~~~~~~~~

``name``
    The human-readable name of the jurisdiction, such as 'New Hampshire'.
``id``
    The ocd-jurisdiction ID of the desired jurisdiction, such as 'ocd-jurisdiction/country:us/state:nh'.

You are required to provide one of the two available parameters.

person
------

Look up a single person by ocd-person ID.

This will return a single :ref:`PersonNode` by ID.

Parameters:
~~~~~~~~~~~

``id``
    ocd-person ID for the desired individual.

organization
------------

Look up a single organization by ocd-organization ID.

This will return a single :ref:`OrganizationNode` by ID.

Parameters:
~~~~~~~~~~~

``id``
    ocd-organization ID for the desired individual.

bill
----

Look up a single bill by ID, URL, or (jurisdiction, session, identifier) combo.

This will return a single :ref:`BillNode` object with the specified bill.

Parameters:
~~~~~~~~~~~

``id``
    The ocd-bill ID of the desired bill, such as 'ocd-jurisdiction/country:us/state:nh'.
``openstatesUrl``
    The URL of the desired bill, such as 'https://openstates.org/nc/bills/2019/HB760/'.
``jurisdiction``, ``session``, ``identifier``
    Must be specified together to fully identify a bill.

    As is true elsewhere, jurisdiction may be specified by name (New Hampshire) or ocd-jurisdiction ID (ocd-jurisdiction/country:us/state:nh).

    Session is specified by legislative session identifier (e.g. 2018 or 49).

    Identifier is the exact identifier of the desired bill, such as "HB 327".

You are required to provide one either ``id`` or the other parameters to fully specify a bill.  Use ``bills`` if you are looking for something more broad.
