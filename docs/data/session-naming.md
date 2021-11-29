# Session Naming

States name their sessions drastically differently, and sometimes
inconsistently even within their own site. (49th vs 2008 Regular
Session). As our goal is to help smooth these inconsistencies we put
forward this guide to naming sessions within state metadata. (See
<https://github.com/sunlightlabs/openstates/issues/81> for discussion on
the topic)

## Default Session Names

The `sessions` list within `terms` is dangerous to change as all bill
data is keyed off it. As a rule these should be short and generally
useful for the scraper to make the appropriate decisions on what data to
scrape.

If a state calls its 1st special session in 2010 '2010E1' this is a
perfectly acceptable name for the session in the metadata. Similarly
49th-regular, 2009-Special-B, etc. are fine names. Generally names with
spaces should be avoided simply for ease of construction of URLs, etc.
In states where spaces are already in use it is fine to continue to use
them.

The one caveat is that if a state uses a unique ID that has no bearing
on the session itself such as '7323' for the 2011 session, this *should
not* be used. Instead add some mapping that maps a session name that is
descriptive to their internal ids.

## Session Display Names

Because the most convenient name to refer to a session is often far from
what a user might expect to see upon opening a mobile application, the
`session_details` dict supports a `display_name` key.

Suitable display names are descriptive but also short and obey a given
style.

### General Rules

-   All sessions should be in title case.
-   Fewer than 20 characters is highly preferable.
-   Months should be abbreviated to 3 letters (Jan., Feb., Jun., Dec.)

### Ordinals

If no special sessions are present:

:   -   \[Ordinal\] Legislature

If special sessions are present:

:   -   \[Ordinal\] Regular Session
    -   \[Ordinal\], \[Ordinal\] Special Session

Examples:

> -   82nd Legislature
> -   82nd Regular Session
> -   82nd, 3rd Special Session

### Years

-   \[Year/Year-Range\] Regular Session
-   \[Year/Year-Range\], \[Ordinal\] Special Session
-   \[Mon. Year\] Special Session

Examples:

> -   2010 Regular Session
> -   2011-2012, 4th Special Session
> -   Dec. 2011 Special Session
