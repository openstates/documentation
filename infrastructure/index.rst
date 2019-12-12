Open States Infrastructure
==========================

.. warning::
  This section is out of date and will be rewritten soon.

Open States has grown to be a rather complex project over the years.  The purpose of this documentation is to help explain how Open States works end-to-end to better help contributors and project members grasp the entirety of the system.

Overview
--------

A great deal of Open States' infrastructure falls within the `billy <https://docs.openstates.org/projects/billy/>`_ project.  Here's roughly how it works:

* A scraper is written that utilizes ``billy.scrape``'s Python helpers.
* When invoked via ``billy-update``, this scraper writes JSON files to disk.
* These JSON files are then imported by ``billy.importers`` into MongoDB.
* A denormalization step takes place allowing us to have aggregate info, ``billy.reports``.

At this point the data has been scraped, validated, post-processed, imported, and we've generated some statistics like how many bills were updated, etc.

From here, the data is served out via a Django project.  ``billy`` also helps with this, in the form of three applications:

``billy.web.public``
    Essentially the site you see when you browse `OpenStates.org <https://openstates.org>`_.
``billy.web.api``
    Open States API v1
``billy.web.admin``
    This is a custom-built admin, truthfully just a series of views that project maintainers have found useful over time.  There are some tools for manual data entry/reconciliation as well as error reporting.

    **Note:** Now that the project is more community-driven and doesn't have a full-time staff, this approach is somewhat outdated and we'll be looking to improve access so that non-staff can help with some of the tasks included in the admin.
