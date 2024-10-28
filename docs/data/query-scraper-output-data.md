# How to query and examine data file output created by scrapers

This document is aimed to help with scraper development and data debugging.

Open States scrapers typically produce a set of output data files in the JSON format. Each file represents a primary
entity that the scraper has produced, such as a Bill, an Event or a Vote Event. When working on scraper code, it can
be really helpful to examine this output for data quality issues.

## Simple examination

The JSON format is relatively easy to view in any text editor or Integrated Development Environment (IDE). This makes
it easy to examine a specific output file.

And you can take it a step further by using a tool like [jq](https://jqlang.github.io/jq/) to query the JSON document
so that you only see the attributes that are relevant to you. For example:

```shell
cat vote_event_ff4ae3ef-656f-11ef-8b4c-732c295f582c.json | jq .counts
```

## Querying a full set of scraper output files

But what if you want to check data quality issues that might be distributed across the output, which may consist of
thousands of data points? You can put together shell scripts/commands that iterate through and find certian files,
then parse them with `jq`. But this can be tricky, especially if you are not familiar with advanced shell scripting.

The good news is that, if you are familiar with SQL, there is a way to treat a set of scraper output files like a
database and use SQL to query that database. A tool called [DuckDB](https://duckdb.org/) makes this possible.

Let's assume I've run one or more scrapers for Delaware. I open my terminal and look at the scraper output directory,
`_data/de`. There are over 3000 JSON files in that folder, so it would be painful to spot check lots of files:

```
jesse@work:~/repo/openstates/openstates-scrapers/scrapers/_data/de$ ls -alh | grep ".json" | wc -l
3760
```

However, installing the DuckDB tool allows me to use it inside that folder to query the Bills data:

```
jesse@work:~/repo/openstates/openstates-scrapers/scrapers/_data/de$ duckdb
v0.10.2 1601d94f94
Enter ".help" for usage hints.
Connected to a transient in-memory database.
Use ".open FILENAME" to reopen on a persistent database.
D SELECT * FROM read_json('bill_*.json');
```

Running `duckdb` in this way opens a "transient" database, meaning that anything you do will be gone once you close
the `duckdb` terminal (by using the `ctrl+d` keystroke). If you want to save anything you create in your session,
instead open it with a file that will save those Views/Tables: `duckdb scraper_output.db`

You can see the data schema that DuckDB has identified from the JSON files by using `DESCRIBE`:

```
D DESCRIBE SELECT * FROM read_json('bill_*.json');
┌─────────────────────┬──────────────────────────────────────────────────────────────────────────────────────────────┬─────────┬─────────┬─────────┬─────────┐
│     column_name     │                                         column_type                                          │  null   │   key   │ default │  extra  │
│       varchar       │                                           varchar                                            │ varchar │ varchar │ varchar │ varchar │
├─────────────────────┼──────────────────────────────────────────────────────────────────────────────────────────────┼─────────┼─────────┼─────────┼─────────┤
│ legislative_session │ VARCHAR                                                                                      │ YES     │         │         │         │
│ identifier          │ VARCHAR                                                                                      │ YES     │         │         │         │
│ title               │ VARCHAR                                                                                      │ YES     │         │         │         │
│ from_organization   │ VARCHAR                                                                                      │ YES     │         │         │         │
│ classification      │ VARCHAR[]                                                                                    │ YES     │         │         │         │
│ subject             │ JSON[]                                                                                       │ YES     │         │         │         │
│ abstracts           │ STRUCT(note VARCHAR, abstract VARCHAR)[]                                                     │ YES     │         │         │         │
│ other_titles        │ STRUCT(note VARCHAR, title VARCHAR)[]                                                        │ YES     │         │         │         │
│ other_identifiers   │ JSON[]                                                                                       │ YES     │         │         │         │
│ actions             │ STRUCT(description VARCHAR, date DATE, organization_id VARCHAR, classification VARCHAR[], …  │ YES     │         │         │         │
│ sponsorships        │ STRUCT("name" VARCHAR, classification VARCHAR, entity_type VARCHAR, "primary" BOOLEAN, per…  │ YES     │         │         │         │
│ related_bills       │ JSON[]                                                                                       │ YES     │         │         │         │
│ versions            │ STRUCT(note VARCHAR, links STRUCT(url VARCHAR, media_type VARCHAR)[], date VARCHAR, classi…  │ YES     │         │         │         │
│ documents           │ STRUCT(note VARCHAR, links STRUCT(url VARCHAR, media_type VARCHAR)[], date VARCHAR, classi…  │ YES     │         │         │         │
│ citations           │ STRUCT("publication" VARCHAR, citation VARCHAR, citation_type VARCHAR, effective JSON, exp…  │ YES     │         │         │         │
│ sources             │ STRUCT(url VARCHAR, note VARCHAR)[]                                                          │ YES     │         │         │         │
│ extras              │ JSON                                                                                         │ YES     │         │         │         │
│ _id                 │ UUID                                                                                         │ YES     │         │         │         │
├─────────────────────┴──────────────────────────────────────────────────────────────────────────────────────────────┴─────────┴─────────┴─────────┴─────────┤
│ 18 rows                                                                                                                                          6 columns │
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```

You'll notice that related entities like Bill Actions, Bill Sponsorships, etc... are embedded in the data as `STRUCT`
data: `actions`, `sponsorships`, etc.. This is typically structured as a list of `STRUCT` data, which is like a dict
or object (`STRUCT(property_a VARCHAR, property_b VARCHAR)[]`). We can drill down into those.

### Use views to drill into the data

To make this data structure simpler to reason about, we can use Views. Here's a set of recommended views you can try.
Please note that these views are used in many of the suggested queries below. In order to execute a query that uses
a view, you need to create the view first!

```sql
-- Bills
CREATE VIEW bills AS
SELECT *
FROM read_json('bill_*.json');
CREATE VIEW bill_actions AS
SELECT legislative_session, identifier AS bill_identifier, _id AS bill_id, unnest(actions, recursive := true)
FROM bills;
CREATE VIEW bill_action_classifications AS
SELECT legislative_session, bill_identifier, bill_id, unnest(classification) AS classification
FROM bill_actions;
CREATE VIEW bill_sponsorships AS
SELECT legislative_session, identifier AS bill_identifier, _id AS bill_id, unnest(sponsorships, recursive := true)
FROM bills;
CREATE VIEW bill_versions AS
SELECT legislative_session, identifier AS bill_identifier, _id AS bill_id, unnest(versions, recursive := true)
FROM bills;
CREATE VIEW bill_version_links AS
SELECT legislative_session, bill_identifier, bill_id, unnest(links, recursive := true)
FROM bill_versions;

-- Vote Events
CREATE VIEW vote_events AS
SELECT *
FROM read_json('vote_event_*.json');
CREATE VIEW vote_event_votes AS
SELECT legislative_session,
       bill_identifier,
       identifier AS vote_identifier,
       _id        AS vote_id,
       unnest(votes, recursive := true)
FROM vote_events;
CREATE VIEW vote_event_counts AS
SELECT legislative_session,
       bill_identifier,
       identifier AS vote_identifier,
       _id        AS vote_id,
       unnest(counts, recursive := true)
FROM vote_events;

-- Events
CREATE VIEW events AS
SELECT *
FROM read_json('event*.json');
CREATE VIEW event_media AS
SELECT name AS event_name, _id AS event_id, unnest(media)
FROM events;
CREATE VIEW event_participants AS
SELECT name AS event_name, _id AS event_id, unnest(participants, recursive := true)
FROM events;
CREATE VIEW event_agenda AS
SELECT name AS event_name, _id AS event_id, unnest(agenda, recursive := true)
FROM events;
CREATE VIEW event_agenda_related_entities AS
SELECT event_name, event_id, unnest(related_entities, recursive := true)
FROM event_agenda;
CREATE VIEW event_sources AS
SELECT name AS event_name, _id AS event_id, unnest(sources, recursive := true)
FROM events;
```

To view Views in your DuckDB database: `SHOW TABLES;`

### Useful queries

These queries will assume you've created the Views listed above! Execute the Views queries above before trying these!

General utility: describe the shape of a given dataset:

```sql
DESCRIBE SELECT * FROM bills;
DESCRIBE SELECT * FROM vote_events;
```

### Bills

#### Bills that are missing a property that is a list (abstracts, actions, sponsorships, etc.)

```sql
SELECT (len(abstracts) > 0) AS has_abstract, COUNT(*)
FROM bills
GROUP BY 1;
SELECT (len(sponsorships) > 0) AS has_sponsorship, COUNT(*)
FROM bills
GROUP BY 1;
SELECT (len(related_bills) > 0) AS has_related_bills, COUNT(*)
FROM bills
GROUP BY 1;
```

#### Distribution of bill classifications

```sql
SELECT classification, COUNT(*)
FROM bills
GROUP BY classification;
```

#### Distribution of bill action classifications

```sql
SELECT classification, COUNT(*)
FROM bill_action_classifications
GROUP BY 1;
```

#### Bill actions that are unclassified

```sql
SELECT (classification = []) AS action_is_unclassified, COUNT(*)
FROM bill_actions
GROUP BY 1;
```

#### Bill version media type distribution

```sql
SELECT media_type, COUNT(*)
FROM bill_version_links
GROUP BY 1;
```

### Vote Events

#### Vote Events over time (by month)

```sql
SELECT date_trunc('month', start_date), COUNT(*)
FROM vote_events
GROUP BY 1,
ORDER BY 1;
```

#### Vote events with zero or unexpected number of votes

```sql
SELECT len(votes) AS num_votes, COUNT(*) AS events_with_this_number
FROM vote_events
GROUP BY 1
ORDER BY 1 DESC;
```

or check via count values for vote events where all counts are zero

```sql
SELECT vote_id, SUM(value)
FROM vote_event_counts
GROUP BY 1
HAVING SUM(value) = 0;
```

#### Vote event counts check distribution for anomalies

```sql
SELECT
option, value, COUNT (*)
FROM vote_event_counts
GROUP BY 1, 2
ORDER BY 1, 2 DESC;
```

### Events

#### Date-sorted lists of events to compare to source

Basic event info

```sql
SELECT start_date, all_day, name, location.name, status, classification, description
FROM events
ORDER BY start_date;
```

Event info with source URL

```sql
SELECT e.start_date, e.all_day, e.name, e.location.name, e.status, e.classification, e.description, s.url
FROM events e
LEFT JOIN event_sources s ON e._id = s.event_id
ORDER BY start_date;
```

#### Check distribution of related entity counts

```sql
SELECT len(media), COUNT(*)
FROM events
GROUP BY 1
ORDER BY 2 DESC;
SELECT len(documents), COUNT(*)
FROM events
GROUP BY 1
ORDER BY 2 DESC;
SELECT len(links), COUNT(*)
FROM events
GROUP BY 1
ORDER BY 2 DESC;
SELECT len(participants), COUNT(*)
FROM events
GROUP BY 1
ORDER BY 2 DESC;
SELECT len(agenda), COUNT(*)
FROM events
GROUP BY 1
ORDER BY 2 DESC;
SELECT len(sources), COUNT(*)
FROM events
GROUP BY 1
ORDER BY 2 DESC;
```

#### Event Agenda Related Entities

Distribution of entity types

```sql
SELECT entity_type, COUNT(*)
FROM event_agenda_related_entities
GROUP BY 1;
```

List of events with related entities, sorted by event date

```sql
SELECT e.start_date, re.event_name, re.name, entity_type
FROM event_agenda_related_entities re
         INNER JOIN events e ON re.event_id = e._id
ORDER BY e.start_date;
```
