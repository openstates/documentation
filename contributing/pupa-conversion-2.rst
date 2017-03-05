Converting Scrapers to pupa (continued)
=======================================

Picking up where we left off in :doc:`pupa-conversion`, we'll now look at how to convert committee, bill, vote, and event scrapers from billy to pupa.

Converting Committees
---------------------

There may not be a committee scraper, but good news if there is- it is one of the easier ones to convert.

In pupa committees are just a type of ``Organization``, which we dealt with before when filling out our metadata.

Here are the steps:

1) Update imports and class definition

    ::

        from billy.scrape.committees import CommitteeScraper, Committee

    becomes::

        from pupa.scrape import Scraper, Organization

    And:: 

        class NCCommitteeScraper(CommitteeScraper):
            jurisdiction = 'nc'

    becomes::

        class NCCommitteeScraper(Scraper):

2) Add new class name to metadata:

    In ``openstates/nc/__init__.py`` we add::

        from .committees import NCCommitteeScraper

    And add it to the scrapers config::

        scrapers = {
            'people': NCPersonScraper,
            'committees': NCCommitteeScraper,
        }

3) Update ``scrape()`` method:

    As we saw with the legislator scraper, it should no longer take ``term``, and ``chambers`` should become optional.

    You can `check the example diff <https://github.com/openstates/openstates/commit/2b7536bf3aa7ab94d417b24bb27db0a3aaf16bb5#diff-ef744b16368b99cdd23e4c1bd29bd76aR45>`_ for onehow it was done when converting the NC scraper.

    Additionally, it should be converted to ``yield`` objects instead of calling ``save_committee``.

    .. note:: Recall that as before, if this method dispatches to others, it should call them with ``yield from`` and they should be converted to
         yield as needed.

4) Update ``Committee`` references to ``Organization``:

    The old constructor was ``Committee(chamber, committee, subcommittee)``.  Now constructing a committee ``Organization`` looks like::

        committee = Organization(name, chamber=chamber, classification='committee')

    If you're dealing with a subcommittee you need to pass ``parent_id``.  ``parent_id`` can be:

        * another ``Organization`` instance
        * a dictionary like ``{'name': 'Appropriations', 'classification': 'lower'}`` which will find the House Appropriations committee at import time

        .. TODO: ^this is sort of a weird edge case, and could probably be handled a lot better in pupa

    Things to change:

        * the ``.committee`` attribute is now ``.name``
        * if you were manually accessing ``committee['members']`` for any reason that information is now in ``_related``  (A common use case is checking if any members were saved: `like here <https://github.com/openstates/openstates/commit/2b7536bf3aa7ab94d417b24bb27db0a3aaf16bb5#diff-ef744b16368b99cdd23e4c1bd29bd76aL58>`_.

    But for the most part that's it.

    **example diff:** `NC committees conversion <https://github.com/openstates/openstates/commit/2b7536bf3aa7ab94d417b24bb27db0a3aaf16bb5?w=1>`_

5) Running your committee scraper

    Just like before, we should now be able to run our scraper::

        $ docker-compose run scrape nc committees

    This will just run our new work, among the output will be::

        nc (scrape)
          committees: {}
        committees scrape:
          duration:  0:00:06.125869
          objects:
            membership: 1114
            organization: 59

    Check that the organization number seems in line with what you expected.

    And at the end the billy output should match::

        02:42:17 INFO billy: imported 59 committee files

    At this point your committee scraper is most likely ready to go.  Be sure to update your PR and let a reviewer know.
