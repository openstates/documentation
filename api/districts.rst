.. _districts:

Districts
=========

.. note:: API v2 is now available, please check out `API v2 <https://docs.openstates.org/en/latest/api/v2/>`_.


:ref:`district-search`
    List districts for state (and optionally filtered by chamber).


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


Examples
--------

.. _district-search-example:

District Search
~~~~~~~~~~~~~~~

``openstates.org/api/v1/districts/nc/lower/``

.. code::

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
