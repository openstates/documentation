# Contributing People Data

Person data is maintained in the
[openstates/people](https://github.com/openstates/people) repository.
This repository contains YAML files with all the information on given
individuals and committees.

!!! info

    Please note that this portion of the project is in the public
    domain in the United States with all copyright waived via a
    [CC0](https://creativecommons.org/publicdomain/zero/1.0/) dedication. By
    contributing you agree to waive all copyright claims.

## Checking out

Fork and clone the people repository:

-   Visit <https://github.com/openstates/people> and click the 'Fork' button.

-   Clone your fork using your tool of choice or the command line:

        $ git clone git@github.com:yourname/people.git
        Cloning into 'people'..

-   Build the environment with `poetry`:

        $ poetry install
        Installing dependencies from lock file
        ...

-   And remember to install `pre-commit` hooks:

        $ pre-commit install
        pre-commit installed at .git/hooks/pre-commit

## Repository overview

The repository consists of a few key components:

-   `settings.yml` Settings for state legislatures, including the number of seats, and current vacancies.
-   `data/` Data files in YAML format on legislators, organized by state & status.

You can use the `os-people` and `os-committees` commands to manage the data:

    poetry run os-people --help

or

    poetry run os-committees --help

## Common tasks

### Updating legislator data by hand

Let's say you call a legislator and find out that they have a new phone
number, contribute back!

See [schema.md](https://github.com/openstates/people/blob/master/schema.md)
for details on the acceptable fields. If you're looking to add a lot of
data but unsure where it fits feel free to ask via an issue and we can
either amend the schema or make a recommendation.

0.  Start a new branch for this work
1.  Make the edits you need in the appropriate YAML file. Please keep
    edits to a minimum (e.g. don't re-order fields)
2.  Submit a PR, please describe how you came across this information to
    expedite review.

### Retiring a legislator

0.  Start a new branch for this work
1.  Run `poetry run os-people retire` with the appropriate legislator file(s)
2.  Review the automatically edited files & submit a PR.

### Updating an entire state's legislators via a scrape

Let's say a North Carolina has had an election & it makes sense to
re-scrape everything for that state.

0.  Start a new branch for this work (e.g. `nc-2021-people-update`)
1.  Scrape data using [Open States' Scrapers](https://github.com/openstates/openstates-scrapers)
2.  Run `poetry run os-people merge nc scrapes/2021-01-01/001` against the generated JSON data from the scrape
4.  Manually reconcile remaining changes, will often require some retirements as well.
5.  Check that data looks clean with `poetry run os-people lint nc --summary`
6.  commit your changes and prepare a PR.

Example of the process:

(In this example, we assume the `people` repo is stored at `~/gitroot/people`)

```bash
:#~/gitroot/openstates-scrapers$ poetry run spatula scrape scrapers_next.de.people.House
INFO:scrapers_next.de.people.House:fetching https://legis.delaware.gov/json/House/GetRepresentatives
INFO:scrapers_next.de.people.LegDetail:fetching https://legis.delaware.gov/LegislatorDetail?personId=13589
INFO:scrapers_next.de.people.LegDetail:fetching https://legis.delaware.gov/LegislatorDetail?personId=332
...
success: wrote 41 objects to _scrapes/2022-06-17/001
:#~/gitroot/openstates-scrapers$ OS_PEOPLE_DIRECTORY=~/gitroot/people poetry run os-people merge de _scrapes/2022-06-17/001/
analyzing 120 existing people and 41 scraped
 perfect match
 perfect match
 perfect match
    other_names: append {'start_date': '', 'end_date': '', 'name': 'Gerald L. Brady'}
    name: Gerald L. Brady => Charles "Bud" M. Freel
    email: Gerald.Brady@delaware.gov => Bud.Freel@delaware.gov
    offices changed from:
	Capitol Office address=411 Legislative Ave. Dover, DE 19901 voice=302-744-4351
  to
	Capitol Office address=411 Legislative Avenue Dover, DE 19901 voice=302-744-4351
    links: append {'url': 'https://legis.delaware.gov/LegislatorDetail?personId=24197', 'note': 'homepage'}
    sources: append {'url': 'https://legis.delaware.gov/LegislatorDetail?personId=24197', 'note': ''}
(m)erge? (r)etire Gerald L. Brady? (s)kip? (a)bort?

Aborted!
```

This example stops at step #4 because the final steps all require manual work. We _can_ use `--retirement <date>` to automatically retire members during this process.
