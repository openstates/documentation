Open States provides a JSON API for accessing state legislative
information.

Basics
------

-  All API calls are URLs in the form ``http://openstates.org/api/v1/METHOD/``
-  Responses are `JSON <http://json.org>`__ unless otherwise specified.
-  If an error occurs the response will be a plain text error message
   with an appropriate HTTP error code (404 if object is not found, 401
   if authentication fails, etc.).
-  An API key is required to be passed as request parameter ``apikey``.
   A key can be obtained via http://services.sunlightlabs.com/
-  All changes to the API will be announced on the `Open States Google
   Group <http://groups.google.com/group/fifty-state-project/>`__. It is
   recommended you subscribe if you're using the API.
-  For Python users, there's an official
   `python-sunlight <http://python-sunlight.readthedocs.org>`__ package
   available with full support.

Data Types
----------

Open States provides data about six core data types.

-  `State Metadata <metadata.html#metadata-fields>`__ - Details on what
   data is available, including terms, sessions, and state-specific
   names for things.
-  `Bills <bills.html#bill-fields>`__ - Details on bills & resolutions,
   including actions & votes.
-  `Legislators <legislators.html#legislator-fields>`__ - Details on
   legislators, including contact details.
-  `Committees <committees.html#committee-fields>`__ - Details on
   committees as they currently stand.
-  `Events <events.html#event-fields>`__ - Details on upcoming events
   such as committee meetings and hearings.
-  `Districts <districts.html#district-fields>`__ - Details on districts
   and their boundaries.

Methods
-------

.. raw:: html
   <table>

   <tr> <th> Method </th> <th> URL pattern </th> <th> Description </th> </tr>

   <tr>
    <td> [Metadata Overview](metadata.html#methods/metadata-overview) </td>
    <td> /metadata/ </td>
    <td> Get list of all states with data available and basic metadata about their status.  </td>
   </tr>
   <tr>
    <td> [State Metadata](metadata.html#methods/state-metadata) </td>
    <td> /metadata/`state`/ </td>
    <td> Get detailed metadata for a particular state. </td>
   </tr>
   <tr>
    <td> [Bill Search](bills.html#methods/bill-search) </td>
    <td> /bills/ </td>
    <td> Search bills by (almost) any of their attributes, or full text.  </td>
   </tr>
   <tr>
    <td> [Bill Detail](bills.html#methods/bill-detail) </td>
    <td> /bills/`state`/`session`/`bill_id`/ </td>
    <td> Get full detail for bill, including any actions, votes, etc. </td>
   </tr>
   <tr>
    <td> [Legislator Search](legislators.html#methods/legislator-search) </td>
    <td> /legislators/ </td>
    <td> Search legislators by their attributes.  </td>
   </tr>
   <tr>
    <td> [Legislator Detail](legislators.html#methods/legislator-detail) </td>
    <td> /legislators/`leg_id`/ </td>
    <td> Get full detail for a legislator, including all roles. </td>
   </tr>
   <tr>
    <td> [Geo Lookup](legislators.html#methods/geo-lookup) </td>
    <td> /legislators/geo/?lat=`latitude`&long=`longitude` </td>
    <td> Lookup all legislators that serve districts containing a given point. </td>
   </tr>
   <tr>
    <td> [Committee Search](committees.html#methods/committee-search) </td>
    <td> /committees/ </td>
    <td> Search committees by any of their attributes.  </td>
   </tr>
   <tr>
    <td> [Committee Detail](committees.html#methods/committee-detail) </td>
    <td> /committees/`committee_id`/ </td>
    <td> Get full detail for committee, including all members. </td>
   </tr>
   <tr>
    <td> [Event Search](events.html#methods/event-search) </td>
    <td> /events/ </td>
    <td> Search events by state and type.  </td>
   </tr>
   <tr>
    <td> [Event Detail](events.html#methods/event-detail) </td>
    <td> /event/`event_id`/ </td>
    <td> Get full detail for event. </td>
   </tr>
   <tr>
    <td> [District Search](districts.html#methods/district-search) </td>
    <td> /districts/`state`/[`chamber`/] </td>
    <td> List districts for state (and optionally filtered by chamber).  </td>
   </tr>
   <tr>
    <td> [District Boundary Lookup](districts.html#methods/district-boundary-lookup) </td>
    <td> /districts/boundary/`boundary_id`/ </td>
    <td> Get geographic boundary for a district. </td>
   </tr>
   </table>


Requesting A Custom Fieldset
----------------------------

On essentially every method in the API it is possible to specify a
custom subset of fields on an object by specifying a ``fields``
parameter.

There are two use cases that this functionality aims to serve:

First, if you are writing an application that loads a lot of data but
only uses some of it, specifying a limited subset of fields can reduce
response time and bandwidth. We've seen this approach be particuarly
useful for mobile applications where bandwidth is at a premium.

An example would be a legislator search with
``fields=first_name,last_name,leg_id`` specified. All legislator objects
returned will only have the three fields that you requested.

Second, you can actually specify a set of fields that includes fields
excluded in the default response.

For instance, if you are conducting a bill search, it typically does not
include sponsors, though many sites may wish to use sponsor information
without making a request for the full bill (which is typically much
larger as it includes versions, votes, actions, etc.).

A bill search that specifies ``fields=bill_id,sponsors,title,chamber``
will include the full sponsor listing in addition to the standard
bill\_id, title and chamber fields.

Extra Fields
------------

You may notice that the fields documented are sometimes a subset of the
fields actually included in a response.

Many times as part of our scraping process we take in data that is
available for a given state and is either not available or does not have
an analog in other states. Instead of artificially limiting the data we
provide to the smallest common subset we make this extra data available.

To make it clear which fields can be relied upon and which are perhaps
specific to a state or subset of states we prefix non-standard fields
with a ``+``.

If you are using the API to get data for multiple states, it is best to
restrict your usage to the fields documented here. If you are only
interested in data for a small subset of our available states it might
make sense to take a more in depth look at the API responses for the
state in question to see what extra data we are able to provide.
