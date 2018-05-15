Open States GraphQL API
=======================

**This is still an experimental API, please keep in mind:**

- Backwards compatibility is not at all guaranteed yet, we are working to refine the graph structure before locking it in.
- If you are using the API please `visit our Discourse <https://discourse.openstates.org/>`_ and make use of `our issue tracker <https://github.com/openstates/new-openstates.org/issues>`_ to file comments/issues.
- API keys are not yet required, but will be in coming weeks.  Please go ahead and `register for an API key <https://openstates.org/api/register/>`_.
- Once activated, you'll pass your API key via the ``X-API-KEY`` header.  (No harm in doing this now if you're setting up to use the API programatically.)
- You can also check out our `introductory blog post <TKTK>`_ for more details.


Basics
------

This is a `GraphQL <http://graphql.org/>`_ API, and some of the concepts may seem unfamiliar at first.

There is in essence, only one endpoint: http://alpha.openstates.org/graphql.

This endpoint, when accessed in a browser, will provide an interface that allows you to experiment with queries in the browser, it features autocomplete and a way to browse the full graph (click the 'Docs' link in the upper right corner).

A GraphQL query mirrors the structure of the data that you'd like to obtain.  For example, to obtain a list of legislators you'd pass something like::

    {
        people {
            edges {
                node {
                    name
                }
            }
        }
    }


.. note::

    If you are using the API programatically it is recommended you send the data as part of the POST body, e.g.::

        curl -X POST http://alpha.openstates.org/graphql -d "query={people{edges{node{name}}}}"

Of course, if you try this you'll see it doesn't work since there are some basic limits on how much data you can request at once.  We paginate with the ``first``, ``last``, ``before`` and ``after`` parameters to a root node.  So let's try that again::


    {
        people(first: 3) {
            edges {
                node {
                    name
                }
            }
        }
    }

And you'd get back JSON like::

    {
      "data": {
        "people": {
          "edges": [
            {
              "node": {
                "name": "Lydia Brasch"
              }
            },
            {
              "node": {
                "name": "Matt Williams"
              }
            },
            {
              "node": {
                "name": "Merv Riepe"
              }
            }
          ]
        }
      }
    }


Ah, much better.  Nodes also can take other parameters to filter the returned content.  Let's try the "name" filter which restricts our search to people named Lydia::


    {
        people(first: 3, name: "Lydia") {
            edges {
                node {
                    name
                }
            }
        }
    }

Results in::

    {
      "data": {
        "people": {
          "edges": [
            {
              "node": {
                "name": "Lydia Brasch"
              }
            },
            {
              "node": {
                "name": "Lydia Graves Chassaniol"
              }
            },
            {
              "node": {
                "name": "Lydia C. Blume"
              }
            }
          ]
        }
      }
    }

It is also possible to request data from multiple root nodes at once, for example::

    {
        people(first: 1) {
            edges {
                node { name }
            }
        }
        bills(first: 1) {
            edges {
                node { title }
            }
        }
    }

Would give back something like::

    {
      "data": {
        "people": {
          "edges": [
            {
              "node": {
                "name": "Lydia Brasch"
              }
            }
          ]
        },
        "bills": {
          "edges": [
            {
              "node": {
                "title": "Criminal Law - Animal Abuse Emergency Compensation Fund - Establishment"
              }
            }
          ]
        }
      }
    }

You may notice something here, that you get back just the data you need.  This is extremely powerful, and lets you do the equivalent of many traditional API calls in a single query.


Full-fledged Example
--------------------

Let's take a look at a more useful example::

    {
      bill(jurisdiction: "New York", session: "2017-2018", identifier: "S 5772") {
        title
        actions {
          description
          date
        }
        votes {
          edges {
            node {
              counts {
                value
                option
              }
              votes {
                voterName
                voter {
                  id
                  contactDetails {
                    value
                    note
                    type
                  }
                }
                option
              }
            }
          }
        }
        sources {
          url
        }
        createdAt
        updatedAt
      }
    }

There's a lot going on there, let's break it down::

    bill(jurisdiction: "New York", session: "2017-2018", identifier: "S 5772") {

We're hitting the ``bill`` root node, which takes 3 parameters.  This should get us to a single bill from New York.

::

    title

This is going to give us the title, just like we saw before.

::

    actions {
       description
       date
    }

Here we're going into a child node, in this case all of the actions taken on the bill.
For each action we're requesting a the date & description.

::

    votes {
      edges {
        node {

Here too we're going into a child node, but note that this time we use that "edges" and "node" pattern that we see on root level nodes.  Certain child nodes in the API have the ability to be paginated or further limited, and votes happen to be one of them.  In this case however we're not making use of that so we'll just ignore this. 

(A full discussion of this pattern is out of scope but check out the `Relay pagination specification for more detail <https://facebook.github.io/relay/graphql/connections.htm>`_ for more.)

::

      counts {
        value
        option
      }
      votes {
        voterName
        voter {
          id
          contactDetails {
            value
            note
            type
          }
        }
        option
      }
    }

Here we grab a few more fields, including child nodes of each vote on our Bill.

First, we get a list of counts (essentially pairs of outcomes + numbers e.g. (yes, 31), (no, 5))

We also get individual legislator votes by name, and we traverse into another object to get the Open States ID and contact details for the voter.  (Don't sweat the exact data model here, there will be more on the structure once we get to the actual graph documentation.)
 

::

    sources {
      url
    }
    createdAt
    updatedAt

And back up at the top level, we grab a few more pieces of information about the Bill.

And now you've seen a glimpse of the power of this API.   We were able to get back theexact fields we wanted on a bill, contact information on the legislators that have voted on the bill, and more.

Our result looks like this::

    {
      "data": {
        "bill": {
          "title": "Relates to bureaus of administrative adjudication",
          "actions": [
            {
              "description": "REFERRED TO LOCAL GOVERNMENT",
              "date": "2017-04-28"
            },
            {
              "description": "COMMITTEE DISCHARGED AND COMMITTED TO RULES",
              "date": "2017-06-19"
            },
            {
              "description": "ORDERED TO THIRD READING CAL.1896",
              "date": "2017-06-19"
            },
            {
              "description": "RECOMMITTED TO RULES",
              "date": "2017-06-21"
            }
          ],
          "votes": {
            "edges": [
              {
                "node": {
                  "counts": [
                    {
                      "value": 25,
                      "option": "yes"
                    },
                    {
                      "value": 0,
                      "option": "no"
                    },
                    {
                      "value": 0,
                      "option": "other"
                    }
                  ],
                  "votes": [
                    {
                      "voterName": "John J. Bonacic",
                      "voter": {
                        "id": "ocd-person/da013cd5-dc67-4e65-a310-73aa32ad1f7c"
                        "contactDetails": [
                            {
                              "value": "bonacic@nysenate.gov",
                              "note": "Capitol Office",
                              "type": "email"
                            },
                            {
                              "value": "Room 503\nAlbany, NY 12247",
                              "note": "District Office",
                              "type": "address"
                            },
                            {
                              "value": "518-455-3181",
                              "note": "District Office",
                              "type": "voice"
                            },
                            ...etc...
                         ]
                      },
                      "option": "yes"
                    },
                    {
                      "voterName": "Neil D. Breslin",
                      "voter": {
                        "id": "ocd-person/4b710aee-1b99-42e0-90e2-d41338e8c5df"
                        "contactDetails": [ ...etc... ],
                      },
                      "option": "yes"
                    },
                    {
                      "voterName": "David Carlucci",
                      "voter": {
                        "id": "ocd-person/1b0feab9-02a7-4bcc-b089-3ab23286da68"
                        "contactDetails": [ ...etc... ],
                      },
                      "option": "yes"
                    },
                  ]
                }
              },
              ...etc...
            ]
          },
          "sources": [
            {
              "url": "http://legislation.nysenate.gov/api/3/bills/2017-2018/S5772?summary=&detail="
            },
            {
              "url": "http://www.nysenate.gov/legislation/bills/2017/S5772"
            },
            {
              "url": "http://assembly.state.ny.us/leg/?default_fld=&bn=S5772&Summary=Y&Actions=Y&Text=Y"
            }
          ],
          "createdAt": "2017-07-15 05:08:15.848526+00:00",
          "updatedAt": "2017-07-15 05:08:15.848541+00:00"
        }
      }
    }

Further Details
---------------

.. toctree::
   :maxdepth: 2

   root-nodes
   types
   other
   examples
   changelog
