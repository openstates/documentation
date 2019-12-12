.. _text-extraction:

Text Extraction
===============

The :ref:`bill scrapers <contributing-to-scrapers>` scrape the web and pull down metadata, including links to various versions of the bills.  As a later step, we extract the actual text of the bill so that it can be indexed for search and other uses.

Checking out
------------

Fork and clone the text-extraction repository:

  * Visit https://github.com/openstates/text-extraction and click the 'Fork' button.
  * Clone your fork using your tool of choice or the command line::

        $ git clone git@github.com:yourname/text-extraction.git
        Cloning into 'text-extraction'...

  * And remember to :ref:`install pre-commit <pre-commit>`::

        $ pre-commit install
        pre-commit installed at .git/hooks/pre-commit


Repository overview
-------------------

The text extraction code itself is written as a standalone Python script ``text_extract.py``
that uses configuration and utility functions from within ``extract/``.

You'll also notice a directory called ``raw/`` -- this contains a sampling of bills for each state that we can use to test text-extraction.

Typically if you're making changes in the repository you'll be editing files within ``extract/``, we'll come back to that later.

Running text_extract
--------------------

Just like in other repositories, we'll use docker-compose to run the code.  In this case docker-compose is running ``text_extract.py``, an all-in-one tool that has a few useful subcommands::

  Usage: text_extract.py [OPTIONS] COMMAND [ARGS]...

  Options:
    --help  Show this message and exit.

  Commands:
    reindex-state  rebuild the search index objects for a given state
    sample         obtain a sample of bills to extract text from
    status         print a status table showing the current condition of...
    test           run sample on all states, used for CI
    update         update the saved bill text in the database

For the purposes of development, ``sample`` and ``update`` are the only two commands that you'll need to look at.

Let's go ahead and run sample against NC::

  $ docker-compose run --rm text-extract sample nc
  raw/nc/2017-HR 924-Edition 1.pdf => text/nc/2017-HR 924-Edition 1.pdf.txt (1507 bytes)
  raw/nc/2017-HB 1034-Edition 1.pdf => text/nc/2017-HB 1034-Edition 1.pdf.txt (3096 bytes)
  raw/nc/2019-SB 421-Edition 1.pdf => text/nc/2019-SB 421-Edition 1.pdf.txt (961 bytes)
  raw/nc/2019-HB 430-Edition 1.pdf => text/nc/2019-HB 430-Edition 1.pdf.txt (4831 bytes)
  raw/nc/2017-SB 753-Edition 1.pdf => text/nc/2017-SB 753-Edition 1.pdf.txt (719 bytes)
  raw/nc/2019-HB 788-Edition 1.pdf => text/nc/2019-HB 788-Edition 1.pdf.txt (2674 bytes)
  raw/nc/2017-SB 373-Filed.pdf => text/nc/2017-SB 373-Filed.pdf.txt (18538 bytes)
  raw/nc/2019-SB 574-Filed.pdf => text/nc/2019-SB 574-Filed.pdf.txt (1712 bytes)
  raw/nc/2017-SJR 686-Resolution 2017-12.pdf => text/nc/2017-SJR 686-Resolution 2017-12.pdf.txt (15928 bytes)
  raw/nc/2017-HB 1007-Filed.pdf => text/nc/2017-HB 1007-Filed.pdf.txt (6248 bytes)
  nc: processed 10, 0 skipped, 0 missing, 0 empty

The exact output and number of bills will vary across states, but should be pretty similar.

This command just did a lot:

  * Read in the file ``raw/nc.csv`` to get a list of bills to sample.
  * Downloaded those files (assuming this was the first run) to ``raw/nc/`` so future runs will be faster.
  * Used the extraction function(s) defined in ``extract/__init__.py`` for NC to extract text from the given documents.
  * Wrote that output to ``text/nc/`` so you can compare.

You'll also notice that it helpfully prints the number of bytes of text extracted, this is useful as a first check.  Let's go ahead and look at the shortest one, ``text/nc/2017-SB 753-Edition 1.pdf.txt``.  (Your run may differ, pick whichever you prefer.)
::

  $ cat "text/nc/2017-SB 753 Edition 1.pdf.txt"
  A BILL TO BE ENTITLED
  AN ACT PROVIDING THAT THE DEPOSIT OF CURRENCY AND COINS INTO A CASH
  VAULT THAT PHYSICALLY SECURES THE CASH AND ELECTRONICALLY
  RECORDS THE DEPOSIT DAILY IN AN OFFICIAL DEPOSITORY BANK QUALIFIES
  AS A DAILY DEPOSIT UNDER THE LOCAL GOVERNMENT BUDGET AND FISCAL
  CONTROL ACT FOR FRANKLIN AND WAKE COUNTIES AND THE
  MUNICIPALITIES IN THOSE COUNTIES.
  The General Assembly of North Carolina enacts:
  SECTION 1. Section 2 of S.L. 2011-89 reads as rewritten:
  "SECTION 2. This act applies only to the City of Winston-Salem only.Winston-Salem,
  Franklin County and the municipalities in Franklin County, and Wake County and the
  municipalities in Wake County."
  SECTION 2. This act is effective when it becomes law.

This looks complete, but to check, go ahead and open the equivalent source file, in this case ``raw/nc/2017-SB 753-Edition 1.pdf`` and confirm visually that all the text was extracted.  Don't worry about formatting, or the preamble, as we'll often exclude that and just aim for the interesting bits of the text.

Making changes
--------------

Let's say that we discover that a state has started publishing their bills in a new format.  
Perhaps Alabama switches from PDF to HTML.  It'd first be good to add some of these new bills to the sample csv, which you can do manually or by invoking sample with the ``--resample`` flag.::

  docker-compose run --rm text-extract sample --resample al

Running would result in some warnings being printed and some zero byte files. 

To actually handle the HTML documents we'd open up ``extract/__init__.py`` and find the ``CONVERSION_FUNCTIONS`` dictionary, you'll see a line like::

  CONVERSION_FUNCTIONS = {
      "al": {"application/pdf": extract_line_numbered_pdf},
    ...

The way extraction works is by matching a document found in a scrape to an appropriate function, in this case
PDFs will be sent through the ``extract_line_numbered_pdf`` function.

If the new HTML was wrapped in a given element, perhaps with ``<div id="billtext">`` we could just update that line to look like::

  CONVERSION_FUNCTIONS = {
      "al": {
          "application/pdf": extract_line_numbered_pdf,
          "text/html": extractor_for_element_by_id("billtext"),
      },
    ...

And we'd be good to go.

Tips & Tricks
-------------

* Functions already exist for common configurations of PDF, HTML, Word Doc, and even OCR.  Rarely will you need to write a custom function, always look at the options first.
* When dealing with PDFs, most are either handled by ``extract_line_numbered_pdf`` or ``extract_sometimes_numbered_pdf``, the difference is that "sometimes numbered PDF" accounts for cases where 90% or so of bills are numbered, but a few (often resolutions) are not numbered.
