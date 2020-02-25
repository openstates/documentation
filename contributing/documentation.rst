Contributing Documentation
===========================

If you notice any issues on these docs, or just want to make them better the source is in the `openstates/documentation <https://github.com/openstates/documentation>`_ repository.

This is a fairly standard `Sphinx <https://www.sphinx-doc.org/en/master/>`_ project, 

Checking out
------------

Fork and clone the documentation repository:

  * Visit https://github.com/openstates/documentation and click the 'Fork' button.
  * Clone your fork using your tool of choice or the command line::

        $ git clone git@github.com:yourname/documentation.git
        Cloning into 'documentation'..

Building Docs Locally
---------------------

Step 1) Install poetry if you haven't already (https://python-poetry.org/docs/)

Step 2) Run ``poetry install`` to build virtualenv

Step 3) Run ``poetry run make html`` to build docs.
