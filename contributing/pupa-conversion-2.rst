Converting Scrapers to pupa (continued)
=======================================

Picking up where we left off in :doc:`pupa-conversion`, we'll now look at how to convert committee, bill, vote, and event scrapers from billy to pupa.


Converting Committees
---------------------

There may not be a committee scraper for your selected state. But good news if there is: committee scrapers are straightforward to convert.

In pupa, committees are just a type of ``Organization``. You've already seen ``Organization`` objects briefly, in the ``__init__.py`` pupa metadata file; the legislative branch itself is a pupa ``Organization``, as are its individual chambers.

Here's how to convert the scraper:

1) Update the imports and class definition:

    ::

        # old
        from billy.scrape.committees import CommitteeScraper, Committee

        # new
        from pupa.scrape import Scraper, Organization

    And::

        # old
        class NCCommitteeScraper(CommitteeScraper):
            jurisdiction = 'nc'

        # new
        class NCCommitteeScraper(Scraper):

2) Add the new class name to metadata file:

    In ``openstates/nc/__init__.py``, add::

        from .committees import NCCommitteeScraper

    And add the scraper instance to the ``scrapers`` in that file::

        scrapers = {
            'people': NCPersonScraper,
            'committees': NCCommitteeScraper,
        }

3) Update the ``scrape`` method:

    As we saw with the people scraper, the ``scrape`` method no longer has required parameters. It should no longer take a ``term`` parameter, and ``chambers`` should become optional.

    `**Example diff:** <https://github.com/openstates/openstates/commit/2b7536bf3aa7ab94d417b24bb27db0a3aaf16bb5#diff-ef744b16368b99cdd23e4c1bd29bd76aR45>`_

    Additionally, the ``scrape`` method should now ``yield`` objects instead of calling ``save_committee``.

    .. note:: Recall that, as before, if the ``scrape`` method dispatches other methods, it should call them with ``yield from``, and they should be converted to ``yield`` objects as needed.

4) Update ``Committee`` references to ``Organization``:

    The old constructor was ``Committee(chamber, committee, subcommittee)``.  Under pupa, constructing a committee looks like::

        committee = Organization(
            name,
            chamber=chamber,  # 'upper' or 'lower'
            classification='committee'
        )

    Notably,

        * the ``committee`` attribute is now ``name``
        * if you were manually accessing ``committee['members']`` for any reason, that information is now in ``_related``; a common use of this is checking whether any members were saved: `like here <https://github.com/openstates/openstates/commit/2b7536bf3aa7ab94d417b24bb27db0a3aaf16bb5#diff-ef744b16368b99cdd23e4c1bd29bd76aL58>`_

    If you're instantiating a subcommittee, you'll need to pass ``parent_id`` as an argument as well. ``parent_id`` can be:

        * another instance of ``Organization``
        * a dictionary, like ``{'name': 'Appropriations', 'classification': 'lower'}`` which will find the House Appropriations committee at import-time

        .. TODO: ^this is sort of a weird edge case, and could probably be handled a lot better in pupa

    **Example diff:** `NC committees conversion <https://github.com/openstates/openstates/commit/2b7536bf3aa7ab94d417b24bb27db0a3aaf16bb5?w=1>`_

5) Running your committee scraper:

    Just like before, we should now be able to run our scraper::

        $ docker-compose run scrape nc committees

    Among the output should be::

        nc (scrape)
          committees: {}
        committees scrape:
          duration:  0:00:06.125869
          objects:
            membership: 1114
            organization: 59

    Check that this organization count is in line with what you expect from the state.

    And at the end the billy output, the number of committees should be the same::

        02:42:17 INFO billy: imported 59 committee files

    At this point, your committee scraper is ready to go. Be sure to commit, and update your PR!


Converting Bills
----------------

Bill scrapers are more complex, but conversion to pupa still follows the same basic principles.

1) Update the imports and class definition:

    ::

        # old
        from billy.scrape.bills import BillScraper, Bill

        # new
        from pupa.scrape import Scraper, Bill

    and::

        # old
        class NCBillScraper(BillScraper):
            jurisdiction = 'nc'

        # new
        class NCBillScraper(Scraper):

2) Using the same pattern as you did with people and committees, add the bill scraper's class name to the state's metadata file.

3) Update the ``scrape`` method:

    The billy scrape method was ``scrape(session, chambers)``, and required both parameters.

    We can change it to look something like::

        def scrape(self, session=None, chamber=None):
            if not session:
                session = self.latest_session()
                self.info('no session specified, using %s', session)

            chambers = [chamber] if chamber else ['upper', 'lower']
            for chamber in chambers:
                yield from self.scrape_chamber(chamber, session)

4) Update the usage of ``Bill`` and its methods:

    There are a lot of small changes from billy to pupa regarding bills; here are examples of the main alterations needed:

    Altering the names of the constructor's arguments::

        # old
        Bill(session, chamber, bill_id, title, type=bill_type)

        # new
        Bill(
            bill_id,
            legislative_session=session,  # A session name from the metadata's `legislative_sessions`
            chamber=chamber,  # 'upper' or 'lower'
            title=title,
            classification=bill_type  # eg, 'bill', 'resolution', 'joint resolution', etc.
        )

    Adding versions and documents::

        # old
        bill.add_version(name, url, mimetype='text/html')

        # new
        bill.add_version_link(
            note,  # Description of the version from the state; eg, 'As introduced', 'Amended', etc.
            url,
            media_type='text/html'  # Still a MIME type
        )

        # And analogous for documents, using `add_document_link()`

    .. note:: If there is an ``on_duplicate`` parameter, most likely you'll want to replace it with ``on_duplicate='ignore'``, but it may be worth discussion on the Open States Slack or on the pupa GitHub ticket.

    Adding sponsors::

        # old
        bill.add_sponsor(type, name, chamber=chamber)

        # new
        bill.add_sponsorship(
            name,  # Sponsor's name
            classification=spon_type,  # The state's classification; eg, 'co-sponsor', 'co-author', 'primary'
            entity_type='person',  # If the bill is sponsored by a committee, this should be 'organization' instead
            primary=is_primary  # Boolean value
        )

    Adding actions::

        # old
        bill.add_action(actor, action, date, type=action_type)

        # new
        bill.add_action(
            description,  # Action description, from the state
            date,  # `YYYY-MM-DD` format
            chamber=actor,  # 'upper' or 'lower'
            classification=action_type  # Options explained in the next section
        )

    Adding votes::

        # old
        bill.add_vote(vote)

        # new - yield vote from scrape
        yield vote

    See "Converting Votes" for details on converting a ``Vote`` into a ``VoteEvent``.

    .. TODO: add_companion?

5) Fix action categorization:

    If you try to run the scraper at this point, you'll get an error that the action types fail validation.

    The `billy action types <http://docs.openstates.org/en/latest/policies/categorization.html#action-types>`_ have been normalized in Open Civic Data, and the new types are `documented there <http://docs.opencivicdata.org/en/latest/scrape/bills.html>`_.

    To ease this transition, you can run this utility script to perform an in-place conversion from billy action types to pupa action types.

    **To be on the safe side, commit your code prior to running this script, in case it malfunctions unexpectedly.**

    ::

        $ ./scripts/convert-actions.sh openstates/nc/bills.py

    You'll also want to remove any categorization of actions as ``'other'``, simply opting for ``None`` instead.

    At this point, your bill scraper should be ready to go.

    **Example diff:** `NC bill conversion <https://github.com/openstates/openstates/commit/f8cc29b>`_


Converting Votes
----------------

Votes are a relatively easy process. There are two major changes:

* The class is now named ``VoteEvent`` instead of ``Vote``.
* Instead of using ``'other'`` for all votes that aren't a 'yes' or a 'no', types like ``'excused'``, ``'absent'`` and ``'not voting'`` have been added.

1) Update imports and class definition:

    ::
        # old
        from billy.scrape.bills import VoteScraper, Vote

        # new
        from pupa.scrape import Scraper, VoteEvent

    and::

        # old
        class NCVoteScraper(VoteScraper):
            jurisdiction = 'nc'

        # new
        class NCVoteScraper(Scraper):

2) Just like we've done before, add the new class instance to metadata.

3) Update ``scrape`` method:

    The logic here will be almost identical to what you did in the bill scraper. Note that we need it to scrape the latest session's votes by default.

4) Update usage of ``Vote`` to ``VoteEvent``:

    The old ``Vote`` constructor took a ton of parameters::

        # old
        Vote(chamber, date, motion, passed,
             yes_count, no_count, other_count, type='other', **kwargs)

    Sometimes there'd be even more parameters stuffed into the ``kwargs``, like::

        # also old
        Vote(chamber, date, motion, passed,
             yes_count, no_count, other_count, type='other',
             bill_id=bill_id, bill_chamber=bill_chamber, session=session
             )

    Be careful, too, since many of the older scrapers pass these parameters in by position alone; it's easy to make mistakes in their order when converting.

    ``VoteEvent`` requires all parameters to be passed by keyword::

        # new
        VoteEvent(
            chamber=chamber,  # 'upper' or 'lower'
            start_date='2017-03-04', # 'YYYY-MM-DD' format
            motion_text=motion,
            result=passed, # String value, 'pass' or 'fail'
            classification='passage',  # Can also be 'other'

            # Provide a Bill instance to link with the VoteEvent...
            bill=bill_instance,
            # or pass in bill informaion if a Bill instance isn't available.
            legislative_session=bill_session,
            bill=bill_id,
            bill_chamber=bill_chamber
        )

    Instead of linking a ``Bill`` to a ``VoteEvent`` by calling ``bill.add_vote(vote)``, a ``Bill`` instance or identifying bill information
    is passed directly to the ``VoteEvent`` constructor.

    You'll notice that in the instantiation of the class we didn't pass
    ``yes_count``, ``no_count``, ``other_count``.  Instead we'll set these using the ``set_count`` method::

        vote.set_count('yes', yes_count)
        vote.set_count('no', no_count)

        # if possible, we'll split 'other' out into more specific values
        vote.set_count('absent', absent_count)
        vote.set_count('not voting', not_voting_count)

    Individual legislators's votes are added to the ``VoteEvent`` in the same way as in billy, with the only exception being ``.other``::

        # these haven't changed between billy and pupa
        vote.yes(yes_voter_name)
        vote.no(no_voter_name)

        # old
        vote.other(other_voter_name)
        # new
        vote.vote('not voting', not_voting_name)
        vote.vote('absent', absentee_name)

    Our example state of NC was a bit more complex to change, but nonetheless here's the **example diff:** `NC vote conversion <https://github.com/openstates/openstates/commit/61aaa4eb>`_
