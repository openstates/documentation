.. _districts:

Districts
=========

:ref:`district-search`
    List districts for state (and optionally filtered by chamber).
:ref:`district-detail`
    Get geographic boundary for a district.


Methods
-------

.. _district-search:

District Search
~~~~~~~~~~~~~~~

The district search method requires a ``state`` and can optionally also
take a ``chamber`` as part of the URL.

The method returns a list of district objects with the following fields:

-  ``abbr`` State abbreviation.
-  ``boundary_id`` boundary\_id used in :ref:`district-detail`
-  ``chamber`` Whether this district belongs to the upper or lower
   chamber.
-  ``id`` A unique ID for this district (separate from boundary\_id).
-  ``legislators`` List of legislators that serve in this district. (may
   be more than one if ``num_seats`` > 1)
-  ``name`` Name of the district (e.g. '14', '33A', 'Fifth Suffolk')
-  ``num_seats`` Number of legislators that are elected to this seat.
   Generally one, but will be 2 or more if the seat is a multi-member
   district.

**Example:**
:ref:`openstates.org/api/v1/districts/nc/lower/ <district-search-example>`

.. _district-detail:

District Boundary Lookup
~~~~~~~~~~~~~~~~~~~~~~~~

This method returns an full district object, including the boundary
given a ``boundary id``.

The returned object has the following fields:

-  ``abbr`` State abbreviation.
-  ``bbox`` A bounding box composed of a list of two (long, lat) points.
   The first point is the upper left corner, and the second point is the
   lower right.
-  ``boundary_id`` boundary\_id for this boundary.
-  ``chamber`` Whether this district belongs to the upper or lower
   chamber.
-  ``id`` A unique ID for this district (separate from boundary\_id).
-  ``name`` Name of the district (e.g. '14', '33A', 'Fifth Suffolk')
-  ``num_seats`` Number of legislators that are elected to this seat.
   Generally one, but will be 2 or more if the seat is a multi-member
   district.
-  ``region`` A dictionary of the following values:

   -  ``center_lat`` Center latitude of the bounding box.
   -  ``center_lon`` Center longitude of the bounding box.
   -  ``lat_delta`` Equivalent to
      max(\ ``latitude``)-min(\ ``latitude``)
   -  ``lon_delta`` Equivalent to
      max(\ ``longitude``)-min(\ ``longitude``)

-  ``shape`` List of polygons, each of which is a GeoJSON-like list of
   coordinates describing a single polygon.

**Example:**
:ref:`openstates.org/api/v1/districts/boundary/sldl/nc-120/ <district-lookup-example>`

Examples
--------

.. _district-search-example:

District Search
~~~~~~~~~~~~~~~

``openstates.org/api/v1/districts/nc/lower/``

.. code:: json

    [
     { "abbr": "nc",
      "boundary_id": "sldl/nc-1",
      "chamber": "lower",
      "id": "nc-lower-1",
      "legislators": [
       { "full_name": "Bob Steinburg", "leg_id": "NCL000302" }
      ],
      "name": "1",
      "num_seats": 1 },
     { "abbr": "nc",
      "boundary_id": "sldl/nc-12",
      "chamber": "lower",
      "id": "nc-lower-12",
      "legislators": [
       { "full_name": "George Graham", "leg_id": "NCL000281" }
      ],
      "name": "12",
      "num_seats": 1 },
     { "abbr": "nc",
      "boundary_id": "sldl/nc-13",
      "chamber": "lower",
      "id": "nc-lower-13",
      "legislators": [
       { "full_name": "Pat McElraft", "leg_id": "NCL000137" }
      ],
      "name": "13",
      "num_seats": 1 },
     { "abbr": "nc",
      "boundary_id": "sldl/nc-14",
      "chamber": "lower",
      "id": "nc-lower-14",
      "legislators": [
       { "full_name": "George G Cleveland", "leg_id": "NCL000076" }
      ],
      "name": "14",
      "num_seats": 1 },
     { "abbr": "nc",
      "boundary_id": "sldl/nc-15",
      "chamber": "lower",
      "id": "nc-lower-15",
      "legislators": [
       { "full_name": "Phil R Shepard", "leg_id": "NCL000221" }
      ],
      "name": "15",
      "num_seats": 1 },
     ... truncated ...
    ]


.. _district-lookup-example:

District Boundary Lookup
~~~~~~~~~~~~~~~~~~~~~~~~

``openstates.org/api/v1/districts/boundary/sldl/nc-120/``

.. code:: json

    {
     "abbr": "nc",
     "bbox": [
      [ 34.986592, -84.321869 ],
      [ 35.466558, -83.108571 ]
     ],
     "boundary_id": "sldl/nc-120",
     "chamber": "lower",
     "id": "nc-lower-120",
     "name": "120",
     "num_seats": 1,
     "region": {
      "center_lat": 35.226575,
      "center_lon": -83.71522,
      "lat_delta": 0.47996599999999745,
      "lon_delta": 1.2132980000000089
     },
     "shape": [
      [
       [
        [ -84.321797, 34.988965 ],
        [ -84.308201, 35.092843 ],
        [ -84.30696, 35.106162 ],
        [ -84.297721, 35.169478 ],
        [ -84.294723, 35.185594 ],
        [ -84.29024, 35.225572 ],
        [ -84.289921, 35.225585 ],
        [ -84.290061, 35.225257 ],
        [ -84.289621, 35.224677 ],
        [ -84.288516, 35.224391 ],
        [ -84.28712, 35.224877 ],
        [ -84.28512, 35.226577 ],
        [ -84.28322, 35.226577 ],
        [ -84.28152, 35.229277 ],
        [ -84.27792, 35.231477 ],
        [ -84.27702, 35.233177 ],
        [ -84.27662, 35.233277 ],
        ... truncated ..
       ],
       ... truncated ...
      ]
     ]
    }
