# Other Notes

There are a few other things to be aware of while using the API:

## Explore the Graph

GraphQL is still quite new, so we figured it might be good to provide
some helpful tips on how to think about the data and how you'll use the
API.

First, it is probably well worth your time to play around in GraphiQL to
explore the API and data. It was heavily used when developing the API
and writing tests, and is a very powerful tool, particularly when you
make use of the self-documenting nature of the graph.

When you're thinking about how to query don't necessarily try to
replicate your old API calls exactly. For example, perhaps you were
grabbing all bills that met a given criteria and then grabbing all
sponsors contact details. This can now be done in one call by traversing
from the `bills-root` root node into the `BillSponsorshipNode` and then up to the
`PersonNode` and finally to the `ContactDetailNode` This may sound
complex at first, but once you get the hang of it, it really does unlock
a ton of power and will make your apps more powerful and efficient.

## Pagination

In several places (such as the `bills-root` and `BillNode`'s `votes`) we mention that nodes are paginated.

What this means in practice is that instead of getting back the
underlying node type, say `BillNode`, directly, you'll get back
`BillConnectionNode` or similar. (In practice there are connection node
types for each paginated type, but all work the same way in our case.)

### Arguments

Each paginated endpoint accepts any of four parameters:

-   `first` - given an integer N, only return the first N nodes
-   `last` - given an integer N, only return the last N nodes
-   `after` - combined with `first`, will return first N nodes after a
    given "cursor"
-   `before` - combined with `last`, will return last N nodes before a
    given "cursor"

So typically you'd paginate using `first`, obtaining a cursor, and then
calling the API again with a combination of `first` and `after`.

The same process could be carried out with `last` and `before` to
paginate in reverse.

### Responses

Let's take a look at everything that pagination makes available:

    {
      bills(first:20) {
        edges {
          node {
            title
          }
          cursor
        }
        pageInfo {
          hasNextPage
          hasPreviousPage
          endCursor
          startCursor
        }
        totalCount
      }
    }

You'll see that the connection node has three nodes: `edges`,
`pageInfo`, and `totalCount`

-   

    `edges` - a list of objects that each have a `node` and `cursor` attribute:

    :   -   `node` - the underlying node type, in our case `BillNode`
        -   `cursor` - an opaque cursor for this particular item, it can
            be used with the `before` and `after` parameters each
            paginated node accepts as arguments.

-   

    `pageInfo` - a list of helpful pieces of information about this page:

    :   -   `hasNextPage` - boolean that is true if there is another
            page after this
        -   `hasPreviousPage` - boolean that is true if there is a page
            before this
        -   `endCursor` - last cursor in the set of edges, can be used
            with `after` to paginate forward
        -   `startCursor` - first cursor in the set of edges, can be
            used with `before` to paginate backwards

-   `totalCount` - total number of objects available from this
    connection

### In Practice

Let's say you want to get all of the people matching a given criteria:

You'd start with a query for all people matching your criteria,
ensuring to set the page size to no greater than the maximum:

    {
        people(memberOf: "Some Organization", first: 100) {
            edges {
                node {
                    name
                }
            }
            pageInfo {
                hasNextPage
                endCursor
            }
        }
    }

Let's say we got back a list of 100 edges and our `pageInfo` object
looked like:

    {
        "hasNextPage": true,
        "endCursor": "ZXJyYXlxb20uZWN0aW9uOjA="
    }

So you'd make another call:

    {
        people(memberOf: "Some Organization", first: 100, after:"ZXJyYXlxb20uZWN0aW9uOjA=" ) {
            edges {
                node {
                    name
                }
            }
            pageInfo {
                hasNextPage
                endCursor
            }
        }
    }

And let's say in this case you got back only 75 edges, and our
`pageInfo` object looks like:

    {
        "hasNextPage": false,
        "endCursor": "AXjYylxX2bu1wxa9uunnb="
    }

We'd stop iteration at this point, of course, if hasNextPage had been
true, we'd continue on until it wasn't.

## Renaming fields

A really useful trick that is often overlooked is that you can rename
fields when retrieving them, for example:

    {
      republicans: people(memberOf: "Republican", first: 5) {
        edges {
          node {
            full_name: name
          }
        }
      }
    }

Would give back:

    {
      "data": {
        "republicans": {
          "edges": [
            {
              "node": {
                "full_name": "Michelle Udall"
              }
            },
            {
              "node": {
                "full_name": "Kimberly Yee"
              }
            },
            {
              "node": {
                "full_name": "Regina E. Cobb"
              }
            },
            {
              "node": {
                "full_name": "Michelle B. Ugenti-Rita"
              }
            },
            {
              "node": {
                "full_name": "David Livingston"
              }
            }
          ]
        }
      }
    }

Note that we're both renaming a top-level node here as well as a piece
of data within the query.

You can also use this to query the same root node twice (doing so
without renaming isn't allowed since it results in a name conflict).

For example:

    {
      republicans: people(memberOf: "Republican", first: 5) {
        edges {
          node {
            full_name: name
          }
        }
      }
      democrats: people(memberOf: "Democratic", first: 5) {
        edges {
          node {
            full_name: name
          }
        }
      }
    }

## Fuzzy Date Format {#date-format}

Unless otherwise noted (most notably `createdAt` and `updatedAt` all
date objects are "fuzzy". Instead of being expressed as an exact date,
it is possible a given date takes any of the following formats:

-   YYYY
-   YYYY-MM
-   YYYY-MM-DD
-   YYYY-MM-DD HH:MM:SS (if times are allowed)

Action/Vote times are all assumed to be in the state capitol's time
zone.

Times related to our updates such as updatedAt and createdAt are in UTC.

## Name Matching

In several places such as bill sponsorships and votes you'll notice
that we have a raw string representing a person or organization as well
as a place for a link to the appropriate `OrganizationNode` or `PersonNode`.

Because of the way we collect the data from states, we always collect
the raw data and later make an attempt to (via a mix of automated
matching and manual fixes) connect the reference with data we've
already collected.

In many cases these linkages will not be provided, but with some
upcoming new tools to help us improve this matching we'll be able to
dramatically improve the number of matched entities in the near future.
