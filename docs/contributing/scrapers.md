# Contributing to Scrapers

Scrapers are at the core of what Open States does, each state requires
several custom scrapers designed to extract bills, legislators,
committees, and votes from the state website. All together there are
around 200 scrapers, each one essentially independent, which means that
there is always more work to do, but fortunately plenty of prior work to
learn from.

## Checking Out

Fork and clone the main scraper repository:

- Visit <https://github.com/openstates/openstates-scrapers> and
    click the 'Fork' button.

- Clone your fork using your tool of choice or the command line:

        $ git clone git@github.com:yourname/openstates-scrapers.git
        Cloning into 'openstates-scrapers'...

- And remember to `install pre-commit <pre-commit>` hooks:

        $ pre-commit install
        pre-commit installed at .git/hooks/pre-commit

- Be sure to run `poetry install` to fetch the correct version of dependencies.

!!! warning

    Before cloning on a Windows computer, you will need to disable
    line-ending conversion. `git config --global core.autocrlf false` After
    cloning and entering the repo, you'll likely want to set global
    line-ending conversion back to true, and set local conversion to false.

## Repository Overview

At this point you'll have a local `openstates-scrapers` directory.
Within it, you'll find a directory called `scrapers`, lets take a look
at it:

    $ ls scrapers
    __init__.py dc          in          mn          nj          pr          va
    ak          de          ks          mo          nm          ri          vi
    al          fl          ky          ms          nv          sc          vt
    ar          ga          la          mt          ny          sd          wa
    az          hi          ma          nc          oh          tn          wi
    ca          ia          md          nd          ok          tx          wv
    co          id          me          ne          or          ut          wy
    ct          il          mi          nh          pa          utils

This directory has 50+ python modules, one for each state.

Let's look inside one:

    $ ls scrapers/nc
    __init__.py    bills.py     votes.py

Some states' directories will differ a bit, but all will have
`__init__.py` and `bills.py`.

The `__init__.py` file for each state has basic metadata on the state
including a list of sessions.

Other files contain the scrapers, typically named `bills`, `votes`, etc.

At the root, you'll also find a directory called `scrapers_next`. This directory also has python modules for each state.

Inside a state, you'll find `people` and potentially `committee` scrapers written using 
[spatula](https://jamesturk.github.io/spatula/). The plan is to port all scrapers to this framework and have 
`scrapers_next` replace the `scraper` directory.

## Running Your First Scraper

Let's run your state's bills scraper (substitute your state for 'nc' below) :

    $ docker-compose run --rm scrape nc bills --fastmode --scrape

The parameters you pass after `docker-compose run --rm scrape` are
passed to `os-update`. Here we're saying that we're running NC's
scrapers, and that we want to do it in "fast mode". By default,
`os-update` imports results into a postgres database; the `--scrape`
flag skips that step.

The following arguments are optional: To bring up a list of the optional arguments in the CLI use `-h`, `--help`\
    `-h`, `--help`          show this help message and exit\
    `--debug`               open debugger on error\
    `--loglevel {LOGLEVEL}` set log level. options are: `DEBUG|INFO|WARNING|ERROR|CRITICAL (default is INFO)`\
    `--scrape`              only run scrape post-scrape step\
    `--import`              only run import post-scrape step\
    `--nonstrict`           skip validation on save\
    `--fastmode`            use cache and turn off throttling\
    `--datadir {SCRAPED_DATA_DIR}` data directory\
    `--cachedir {CACHE_DIR}`  cache directory\
    `-r {SCRAPELIB_RPM}`    scraper rpm\
    `--rpm {SCRAPELIB_RPM}` scraper rpm\                        
    `--timeout {SCRAPELIB_TIMEOUT}` scraper timeout\
    `--no-verify`           skip tls verification\
    `--retries {SCRAPELIB_RETRIES}` scraper retries\
    `--retry_wait {SCRAPELIB_RETRY_WAIT_SECONDS}` scraper retry wait\
    `--realtime` loads bills in realtime to database, this requires configuring an AWS S3 bucket and using the lambda function: [openstates-realtime](https://github.com/openstates/openstates-realtime) 

You'll see the *run plan*, which is what the update aims to capture; in
this case we're scraping the state website's data into JSON files:

    nc (scrape)
      bills: {}

Then legislative posts and organizations get created, which is mostly
boilerplate:

    08:46:35 INFO openstates: save jurisdiction North Carolina as jurisdiction_ocd-jurisdiction-country:us-state:nc-government.json
    08:46:35 INFO openstates: save organization North Carolina General Assembly as organization_01d6327c-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO openstates: save organization Executive Office of the Governor as organization_01d63560-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO openstates: save organization Senate as organization_01d636e6-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO openstates: save post 1 as post_01d63a06-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO openstates: save post 2 as post_01d63b96-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO openstates: save post 3 as post_01d63cea-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO openstates: save post 4 as post_01d63e34-72d2-11e7-8df8-0242ac130003.json
    08:46:35 INFO openstates: save post 5 as post_01d63f74-72d2-11e7-8df8-0242ac130003.json

And then the actual data scraping begins, defaulting to the most recent
legislative session:

    08:46:36 INFO openstates: no session specified, using 2017
    08:46:36 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/SimpleBillInquiry/displaybills.pl?Session=2017&tab=Chamber&Chamber=Senate
    08:46:38 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/BillLookUp/BillLookUp.pl?Session=2017&BillID=S1
    08:46:39 INFO openstates: save bill SR 1 in 2017 as bill_03c7edb4-72d2-11e7-8df8-0242ac130003.json
    08:46:39 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/BillLookUp/BillLookUp.pl?Session=2017&BillID=S2
    08:46:39 INFO openstates: save bill SJR 2 in 2017 as bill_044a5fc4-72d2-11e7-8df8-0242ac130003.json
    08:46:39 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/BillLookUp/BillLookUp.pl?Session=2017&BillID=S3
    08:46:40 INFO openstates: save bill SB 3 in 2017 as bill_04e8c66e-72d2-11e7-8df8-0242ac130003.json
    08:46:40 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/BillLookUp/BillLookUp.pl?Session=2017&BillID=S4
    08:46:41 INFO openstates: save bill SB 4 in 2017 as bill_05781f08-72d2-11e7-8df8-0242ac130003.json
    08:46:41 INFO scrapelib: GET - http://www.ncga.state.nc.us/gascripts/BillLookUp/BillLookUp.pl?Session=2017&BillID=S5

Depending on the scraper you run, this part takes a while. Some scrapers
can take hours to run depending on the number of bills and speed of the
state's website.

!!! note

    It is often desirable to bail out of running the whole scrape (Ctrl-C)
    after it has gotten a bit of data, instead of letting it run the entire
    scrape.

To review the data you just fetched, you can browse the \_data/nc/
directory and inspect the JSON files. If you're trying to make a small
fix this is often sufficient, you can confirm that the scraped data
looks correct and move on.


!!! note
    It is of course possible that the scrape fails. If so, there's a good
    chance that isn't your fault, especially if it starts to run and then
    errors out. Scrapers do break, and there's no guarantee North Carolina
    didn't change their legislator page yesterday, breaking our tutorial
    here.

    If that's the case and you think the issue is with the scraper, feel
    free to get in touch with us or [file an
    issue](https://github.com/openstates/openstates/issues).


At this point you're ready to run scrapers and contribute fixes. Hop
onto [our GitHub ticket queue](https://github.com/openstates/openstates/issues), pick an issue
to solve, and then submit a Pull Request!

## Importing Data

Optionally, if you'd like to see how your scraped data imports into the
database, perhaps to diagnose an issue that is happening after the
scrape, pop over to
`getting a working database <working-database>` to see how to get a local database that you can import data
into.

Once that's done, make sure that the db image from openstates.org is running:

    $ docker ps
    CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                    NAMES
    27fe691ad7c5        mdillon/postgis:11-alpine   "docker-entrypoint.s…"   3 hours ago         Up 3 hours          0.0.0.0:5405->5432/tcp   openstatesorg_db_1

Your output will vary, but if you don't see something named
openstatesorg_db running you should run this command (from the
openstates.org directory, not your scraper directory):

    $ docker-compose up -d db

Now, when you want to run imports, you can drop the `--scrape` portion
of the command you've been running. Or if you just want to test the
import of already scraped data you can replace it with `--import`.

An import looks something like this:

    $ docker-compose run --rm scrape fl bills --fast
    ... (truncated) ...
    23:03:34 ERROR openstates: cannot resolve pseudo id to Person: ~{“name”: “Grant, M.“}
    23:03:36 ERROR openstates: cannot resolve pseudo id to Person: ~{“name”: “Rodrigues, R.“}
    fl (import)
      bills: {}
    import:
      bill: 0 new 0 updated 2620 noop
      jurisdiction: 0 new 0 updated 1 noop
      vote_event: 21 new 12 updated 533 noop

The errors about unresolved psuedo-ids can safely be ignored, as long as
you see the final run report the data you scraped is available in your
database.

The number of objects of each type that were created & updated are
available for spot checking, as well as the total number of items that
were seen that already exactly matched what was in the database. These
can be useful stats as you try to see if your local changes to a scraper
have the impact you expect.

## Running Spatula Scrapers

Let's run a people scraper:

    $ poetry run spatula scrape scrapers_next.nc.people.SenList

The command to run these scrapers is structured differently, as the parameters
are set by giving the exact location of the function you want to run: `directory.state.file.function`. 

!!! note
    Function names do vary and scrapes for legislators are commonly split by chamber, so make sure to check you're 
    passing the right function in your command.

The actual data scraping should look something like:

    INFO:scrapers_next.nc.people.SenList:fetching https://www.ncleg.gov/Members/MemberTable/S
    INFO:scrapers_next.nc.people.LegDetail:fetching https://www.ncleg.gov/Members/Biography/S/430
    INFO:scrapers_next.nc.people.LegDetail:fetching https://www.ncleg.gov/Members/Biography/S/431
    INFO:scrapers_next.nc.people.LegDetail:fetching https://www.ncleg.gov/Members/Biography/S/432
    INFO:scrapers_next.nc.people.LegDetail:fetching https://www.ncleg.gov/Members/Biography/S/433
    INFO:scrapers_next.nc.people.LegDetail:fetching https://www.ncleg.gov/Members/Biography/S/434

To review the data you scraped, you can inspect the JSON files in the dated directory within `_scrapes/`. Each time you
run a scrape, a new numbered folder will be within the dated directory, so you can compare older data to new easily.

!!! note
    If a scrape fails, it's likely an issue with the scraper. Feel free to get in touch with us or [file an
    issue](https://github.com/openstates/openstates/issues).

[Spatula](https://jamesturk.github.io/spatula/) is incredibly powerful with lots of flexibility and useful [CLI 
commands](https://jamesturk.github.io/spatula/cli/) that are worth checking out as well.

At this point you're ready to run spatula scrapers and contribute fixes. Hop
onto [our GitHub ticket queue](https://github.com/openstates/openstates/issues), pick an issue
to solve, and then submit a Pull Request!
