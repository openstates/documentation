Boundary JSON
=============

A static API for obtaining simplified GeoJSON describing the boundaries of legislative districts.

If you obtain an Open Civic Data Division ID (e.g. ``ocd-division/country:us/state:ks/sldl:1`` you can convert it to a URL like ``https://data.openstates.org/boundaries/2018/ocd-division/country:us/state:ks/sldl:1.json``.

The URL consists of a few parts:

* ``https://data.openstates.org/boundaries/`` - base URL
* ``2018`` - division issue year, currently only 2018 supported (divisions do not change most years)
* ``ocd-division/country:us/state:ks/sldl:1`` - Open Civic Data Division ID
* ``.json`` - end URL with `.json`


File Format
-----------

* ``division_id`` - Open Civic Data Division ID, same as URL.
* ``year`` - Issue year for Boundary, same as URL.
* ``centroid`` - GeoJSON Point representing the centroid of this boundary.
* ``extent`` - Coordinates for upper-left & lower-right extent of boundary.
* ``shape`` - Simplified GeoJSON MultiPolygon for display purposes.  This data has been simplified (losing some detail to gain speed when drawing/processing) and should be used for display but not intersection/point-in-polygon testing.
* ``metadata`` - Metadata that was provided by issuer, typically a bit of Census data.  No particular guarantee is made about what is here.


Obtaining ocd-division IDs
--------------------------

If you're using Open States data you'll see ocd-division IDs in a few places:

* The division_id & boundary_id keys in the :ref:`districts` endpoint of API v1.
* the id attribute on :ref:`divisionnode` which can be accessed via Post.
