# State-Specific Scraper Info

## California MySQL

California is a unique state that takes a couple of extra steps to get
working locally.

California provides MySQL dumps of their data, and in order to use those
we start up a local MySQL instance and read from that.

To download the data for the current session:

    docker-compose run --rm ca-download

(You can append --year YYYY to instead select data for a given year.)

This will start a local MySQL image as well, that image will need to
stay up for the next step, which is running a scrape like normal:

    docker-compose run --rm ca-scrape ca bills --fast

## State API Keys

Unfortunately, some states find it necessary to require API Keys (or
other credentials) to access their best data.

Despite the difficulties this creates for contributors, in the interest
of ensuring we have the best possible data we've made the decision that
we will use this data where possible.

Our policy:

-   We will maintain (when possible) two copies of credentials, one for
    development and one for production. (Thus minimizing the chance that
    a mistake made w/ a development key will jeopardize our ability to
    scrape.)
-   We encourage developers to get an API key of their own, but if
    necessary we can share our testing key in limited circumstances.

Currently only a few states require API keys:

###  New York

<http://legislation.nysenate.gov/static/docs/html/index.html>

-   Request Form: <http://legislation.nysenate.gov/>
-   Set in environment prior to running scrape: `NEW_YORK_API_KEY`

### Indiana

<http://docs.api.iga.in.gov/api.html>

-   API Key Request Process: Email Bob Amos (<bob.amos@iga.in.gov>
    or <bamos@iga.in.gov>), and include your name, address, phone,
    email address and company. Also indicate that you have read the
    terms of service at the link above.
-   Set in environment prior to running scrape: `INDIANA_API_KEY`
-   As a side note, Indiana also requires a [user-agent string](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent),
    so set that in your environment as well, prior to running
    scrape: `USER_AGENT`

### Virginia 

<https://lis.virginia.gov/SiteInformation/ftp.html>

-   API Credentials Request Process: To acquire access, please
    contact the Virginia Legislative Information System help desk
    at (804) 786-9631 for a user id.
-   Set in environment prior to running scrape: `VIRGINIA_FTP_USER`,
    `VIRGINIA_FTP_PASSWORD`

### District of Columbia

<https://lims.dccouncil.us/api/help/index.html>

-   API Key Request Form:
    <https://lims.dccouncil.us/developerRegistration>
-   Set in environment prior to running scrape: `DC_API_KEY`
