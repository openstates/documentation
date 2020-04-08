Testing Scrapers
================

One of the first things people new to the project tend to notice is that there aren't a lot of tests in the scrapers.

Over the years we've evolved a de facto policy of somewhat discouraging tests, which is definitely an unusual stance to take and warrants explanation.

Intentionally Fragile Scrapers
------------------------------

When it comes to scrapers, there are two major types of breakage:

1) the scraper collects bad information and inserts it into the database
2) the scraper encounters an error and quits without importing data

Given a choice, the second is greatly preferable. Once bad data makes it into the database, it can be difficult to detect and remove.  On the other hand, the second can be triggered to alert us immediately and someone can evaluate the proper fix.

The best way to favor the second over first is to write "intentionally fragile" scrapers.  That is, scrapers that raise an exception when they see unexpected input.  

While it is possible to try to write a resilient scraper that recovers, by nature these scrapers are more likely to produce the first kind of error, and so we encourage scraper writers to be conservative in what errors are suppressed.

Here's an example of an overly permissive scraper::

    party_abbr = doc.xpath('//span[@class="partyabbr"])
    if party_abbr == 'D':
        party = 'Democratic'
    elif party_abbr == 'R':
        party = 'Republican'
    else:
        # haven't seen this yet, but let's just keep things moving
        party = party_abbr

The following would be preferred::

    party_abbr = doc.xpath('//span[@class="partyabbr"])
    party = {'D': 'Democratic', 'R': 'Republican'}[party_abbr]

This code would raise a ``KeyError`` the first time a new party is found.
This forces someone to take a look, fix the scraper with an entry for the new party, and then the scraper will be able to run again with correct data.


Testing Scrapers Is Hard
------------------------

On most software projects a failing test means that something is broken, and passing tests should mean that things are working just fine.

In our experience however, the majority of the "breaks" that occur in scrapers are due to upstream site changes.

In the past the fragile nature of scrapers has led to people writing a lot of bad tests, which is where our stance of somewhat discouraging tests has come from.  An example of a bad test::

    def extract_name(doc):
        return doc.xpath('//h2[@class="legislatorName"]').text_content().strip()


    def test_extract_name():
        # probably a snapshot of the page at some point in time
        EXAMPLE_LEGISLATOR_HTML = '...' 

        doc = lxml.html.fromstring(EXAMPLE_LEGISLATOR_HTML)
        assert extract_name(doc) == 'Erica Example'


With a test like this:
    * As soon as the HTML changes, the scraper will start failing, but the tests will still pass.
    * The scraper will then be updated, breaking the test.
    * The test HTML will be updated, fixing the test.

But since the initial scraper breakage isn't predicted by a failing test, this type of test really doesn't serve us any purpose and just results in extra code to maintain every time the scraper needs a slight change.

Other Strategies
----------------

Of course this isn't to say that we just abandon the idea of testing, altogether.

If you're more comfortable writing tests, say you're parsing a particularly nasty PDF and want to run it against some test data: a test might make sense there as a way to be confident in your own code, by all means, write a test.

We also have some other strategies to help ensure data quality:

Validate Scraper Output
~~~~~~~~~~~~~~~~~~~~~~~

Scraper output is verified against JSON schemas that protect against common regressions (missing sources, invalid formatted districts, etc.) - most of these tests can be written effectively against scraper output across the board, and in doing so also applies universally across all 50 states.

We also aim for our underlying libraries like `openstates-core <https://github.com/openstates/openstates-core>`_ to be as well tested as possible.  (To be 100% clear, our lax testing philosophy only applies to site-specific scraper code, not these support libraries.)

Run Scrapers Regularly
~~~~~~~~~~~~~~~~~~~~~~

In a sense, the scrapers are tested every night by being run.  This is why the intentionally fragile approach is so important; those failures are in essence the same as integration test failures.  Of course, this doesn't tell us if the scraper is picking up bad data, etc., but combined with validation we can be fairly confident in our data.

Test Utilities
~~~~~~~~~~~~~~

One area we can definitely improve upon is our use of (and then thorough testing of) common functions.  Right now (largely because of the great variety of authors, etc.) many scrapers do similar things like conversion of party abbreviations and whitespace normalization in slightly different ways.  We should be making a push to use common utility functions and thoroughly test those.
