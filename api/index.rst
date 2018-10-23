Open States API v1
==================

.. note:: A beta of API v2 is now available, please check out `API v2 <http://docs.openstates.org/en/latest/api/v2/>`_.

Open States provides a JSON API for accessing state legislative
information.

Basics
------

-  All API calls are URLs in the form ``https://openstates.org/api/v1/METHOD/``
-  Responses are `JSON <http://json.org>`_ unless otherwise specified.
-  If an error occurs the response will be a plain text error message
   with an appropriate HTTP error code (404 if object is not found, 401
   if authentication fails, etc.).
-  To use the API you must `register for an API key <https://openstates.org/api/register/>`_.
-  Once activated, pass your API key via the ``apikey`` query paramter or the ``X-API-KEY`` header.
-  All changes to the API will be announced on the `Open States Discourse
   <https://discourse.openstates.org>`_. It is recommended you subscribe if you're using the API.
-  For Python users, there's an official
   `pyopenstates <http://docs.openstates.org/projects/pyopenstates/en/latest/>`__ package
   available.

Data Types
----------

Open States provides data about six core data types.

:ref:`metadata`
    Details on what data is available, including terms, sessions, and state-specific names for things.
:ref:`bills`
    Details on bills & resolutions, including actions & votes.
:ref:`legislators`
    Details on legislators, including contact details.
:ref:`districts`
    Details on districts and their boundaries.
committees
    Committees endpoints have been deprecated and will no longer return current data as of October 2018.
events 
    Events endpoints have been deprecated and will no longer return current data as of 2018.

.. toctree::
   :maxdepth: 2
   :hidden:

   metadata
   bills
   legislators
   districts

Methods
-------

=================================   ==================================================  =====================================================================================
Method                               URL pattern                                         Description
=================================   ==================================================  =====================================================================================
:ref:`metadata-overview`             /metadata/                                          Get list of all states with data available and basic metadata about their status.
:ref:`state-metadata`                /metadata/`state`/                                  Get detailed metadata for a particular state.
:ref:`bill-search`                   /bills/                                             Search bills by (almost) any of their attributes, or full text.
:ref:`bill-detail`                   /bills/`state`/`session`/`bill_id`/                 Get full detail for bill, including any actions, votes, etc.
:ref:`legislator-search`             /legislators/                                       Search legislators by their attributes.
:ref:`legislator-detail`             /legislators/`leg_id`/                              Get full detail for a legislator, including all roles.
:ref:`legislator-geo`                /legislators/geo/?lat=latitude&long=longitude       Lookup all legislators that serve districts containing a given point.
:ref:`district-search`               /districts/`state`/[`chamber`/]                     List districts for state (and optionally filtered by chamber).
:ref:`district-detail`               /districts/boundary/`boundary_id`/                  Get geographic boundary for a district.
=================================   ==================================================  =====================================================================================


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
