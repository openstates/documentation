Other Notes
===========

There are a few other things to be aware of while using the API:

.. _date-format:

Fuzzy Date Format
-----------------

Unless otherwise noted (most notably ``createdAt`` and ``updatedAt`` all date objects are "fuzzy".  Instead of being expressed as an exact date, it is possible a given date takes any of the following formats:

* YYYY
* YYYY-MM
* YYYY-MM-DD
* YYYY-MM-DD HH:MM:SS   (if times are allowed)


.. _name-matching:

Name Matching
-------------

TODO - explain distinction between raw name and resolved name where it occurs
