Examples
========

Get basic information for all legislatures
------------------------------------------

`See in GraphiQL <https://openstates.org/graphql?operationName=null&query=%7B%0A%20%20jurisdictions%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20legislativeSessions%20%7B%0A%20%20%20%20%20%20%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20legislature%3A%20organizations(classification%3A%20%22legislature%22%2C%20first%3A%201)%20%7B%0A%20%20%20%20%20%20%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20classification%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20children(first%3A%205)%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20classification%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A>`__

::

    {
      jurisdictions {
        edges {
          node {
            name
            legislativeSessions {
              edges {
                node {
                  name
                }
              }
            }
            legislature: organizations(classification: "legislature", first: 1) {
              edges {
                node {
                  name
                  classification
                  children(first: 5) {
                    edges {
                      node {
                        name
                        classification
                      }
                    }
                  }
                }
              }
            }
          }
        }
      }
    }


Get overview of a legislature's structure
-----------------------------------------

`See in GraphiQL <https://openstates.org/graphql?operationName=null&query=%7B%0A%20%20jurisdiction(name%3A%20%22North%20Dakota%22)%20%7B%0A%20%20%20%20name%0A%20%20%20%20url%0A%20%20%20%20legislativeSessions%20%7B%0A%20%20%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20identifier%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%20%20organizations(classification%3A%20%22legislature%22%2C%20first%3A%201)%20%7B%0A%20%20%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20children(first%3A%2020)%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A>`__

::

    {
      jurisdiction(name: "North Dakota") {
        name
        url
        legislativeSessions {
          edges {
            node {
              name
              identifier
            }
          }
        }
        organizations(classification: "legislature", first: 1) {
          edges {
            node {
              id
              name
              children(first: 20) {
                edges {
                  node {
                    name
                  }
                }
              }
            }
          }
        }
      }
    }



Search for bills that match a given condition
---------------------------------------------

`See in GraphiQL <https://openstates.org/graphql?operationName=null&query=%20%20%20%20%7B%0A%20%20%20%20%20%20search_1%3A%20bills(first%3A%205%2C%20jurisdiction%3A%20%22New%20York%22%2C%20session%3A%20%222017-2018%22%2C%20chamber%3A%20%22lower%22%2C%20classification%3A%20%22resolution%22%2C%20updatedSince%3A%20%222017-01-15%22)%20%7B%0A%20%20%20%20%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20%20%20%20%20identifier%0A%20%20%20%20%20%20%20%20%20%20%20%20title%0A%20%20%20%20%20%20%20%20%20%20%20%20classification%0A%20%20%20%20%20%20%20%20%20%20%20%20updatedAt%0A%20%20%20%20%20%20%20%20%20%20%20%20createdAt%0A%20%20%20%20%20%20%20%20%20%20%20%20legislativeSession%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20identifier%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20jurisdiction%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20actions%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20date%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20description%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20classification%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20documents%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20date%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20note%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20links%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20versions%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20date%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20note%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20links%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20sources%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20note%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A>`__

::

    {
      search_1: bills(first: 5, jurisdiction: "New York", session: "2017-2018", chamber: "lower", classification: "resolution", updatedSince: "2017-01-15") {
        edges {
          node {
            id
            identifier
            title
            classification
            updatedAt
            createdAt
            legislativeSession {
              identifier
              jurisdiction {
                name
              }
            }
            actions {
              date
              description
              classification
            }
            documents {
              date
              note
              links {
                url
              }
            }
            versions {
              date
              note
              links {
                url
              }
            }
            
            sources {
              url
              note
                
            }
          }
        }
      }
    }

Get all information on a particular bill
----------------------------------------

`See in GraphiQL <https://openstates.org/graphql?operationName=null&query=%20%20%20%20%7B%0A%20%20%20%20%20%20b1%3A%20bill(jurisdiction%3A%20%22Hawaii%22%2C%20session%3A%20%222017%20Regular%20Session%22%2C%20identifier%3A%20%22HB%20475%22)%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20identifier%0A%20%20%20%20%20%20%20%20title%0A%20%20%20%20%20%20%20%20classification%0A%20%20%20%20%20%20%20%20updatedAt%0A%20%20%20%20%20%20%20%20createdAt%0A%20%20%20%20%20%20%20%20legislativeSession%20%7B%0A%20%20%20%20%20%20%20%20%20%20identifier%0A%20%20%20%20%20%20%20%20%20%20jurisdiction%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20actions%20%7B%0A%20%20%20%20%20%20%20%20%20%20date%0A%20%20%20%20%20%20%20%20%20%20description%0A%20%20%20%20%20%20%20%20%20%20classification%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20documents%20%7B%0A%20%20%20%20%20%20%20%20%20%20date%0A%20%20%20%20%20%20%20%20%20%20note%0A%20%20%20%20%20%20%20%20%20%20links%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20versions%20%7B%0A%20%20%20%20%20%20%20%20%20%20date%0A%20%20%20%20%20%20%20%20%20%20note%0A%20%20%20%20%20%20%20%20%20%20links%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20sources%20%7B%0A%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20note%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20b2%3A%20bill(id%3A%20%22ocd-bill%2F9c24aaa2-6acc-43ad-883b-ae9f677062e9%22)%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20identifier%0A%20%20%20%20%20%20%20%20title%0A%20%20%20%20%20%20%20%20classification%0A%20%20%20%20%20%20%20%20updatedAt%0A%20%20%20%20%20%20%20%20createdAt%0A%20%20%20%20%20%20%20%20legislativeSession%20%7B%0A%20%20%20%20%20%20%20%20%20%20identifier%0A%20%20%20%20%20%20%20%20%20%20jurisdiction%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20actions%20%7B%0A%20%20%20%20%20%20%20%20%20%20date%0A%20%20%20%20%20%20%20%20%20%20description%0A%20%20%20%20%20%20%20%20%20%20classification%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20documents%20%7B%0A%20%20%20%20%20%20%20%20%20%20date%0A%20%20%20%20%20%20%20%20%20%20note%0A%20%20%20%20%20%20%20%20%20%20links%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20versions%20%7B%0A%20%20%20%20%20%20%20%20%20%20date%0A%20%20%20%20%20%20%20%20%20%20note%0A%20%20%20%20%20%20%20%20%20%20links%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20sources%20%7B%0A%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20note%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A>`__

::

    {
      b1: bill(jurisdiction: "Hawaii", session: "2017 Regular Session", identifier: "HB 475") {
        id
        identifier
        title
        classification
        updatedAt
        createdAt
        legislativeSession {
          identifier
          jurisdiction {
            name
          }
        }
        actions {
          date
          description
          classification
        }
        documents {
          date
          note
          links {
            url
          }
        }
        versions {
          date
          note
          links {
            url
          }
        }
        sources {
          url
          note
        }
      }
      b2: bill(id: "ocd-bill/9c24aaa2-6acc-43ad-883b-ae9f677062e9") {
        id
        identifier
        title
        classification
        updatedAt
        createdAt
        legislativeSession {
          identifier
          jurisdiction {
            name
          }
        }
        actions {
          date
          description
          classification
        }
        documents {
          date
          note
          links {
            url
          }
        }
        versions {
          date
          note
          links {
            url
          }
        }
        sources {
          url
          note
        }
      }
    }



Get information about a specific legislator
--------------------------------------------

`See in GraphiQL <https://openstates.org/graphql?operationName=null&query=%20%20%20%20%7B%0A%20%20%20%20%20%20person(id%3A%22ocd-person%2Fdd05bd23-fe49-4e65-bfff-62db997e56e0%22)%7B%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20contactDetails%20%7B%0A%20%20%20%20%20%20%20%20%20%20note%0A%20%20%20%20%20%20%20%20%20%20type%0A%20%20%20%20%20%20%20%20%20%20value%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20otherNames%20%7B%0A%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20sources%20%7B%0A%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20currentMemberships%20%7B%0A%20%20%20%20%20%20%20%20%20%20organization%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A>`__

::

    {
      person(id:"ocd-person/dd05bd23-fe49-4e65-bfff-62db997e56e0"){
        name
        contactDetails {
          note
          type
          value
        }
        otherNames {
          name
        }
        sources {
          url
        }
        currentMemberships {
          organization {
            name
          }
        }
      }
    }


Get legislators for a given state/chamber
-----------------------------------------------

`See in GraphiQL <https://openstates.org/graphql?operationName=null&query=%20%20%20%20%7B%0A%20%20%20%20%20%20people(memberOf%3A%22ocd-organization%2Fddf820b5-5246-46b3-a807-99b5914ad39f%22%2C%20first%3A%20100)%20%7B%0A%20%20%20%20%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20party%3A%20currentMemberships(classification%3A%22party%22)%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20organization%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20links%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20sources%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20url%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20chamber%3A%20currentMemberships(classification%3A%5B%22upper%22%2C%20%22lower%22%5D)%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20post%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20label%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20organization%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20classification%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20parent%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A>`__

``ocd-organization/ddf820b5-5246-46b3-a807-99b5914ad39f`` is the id of the Alabama Senate chamber.

:: 

    {
      people(memberOf:"ocd-organization/ddf820b5-5246-46b3-a807-99b5914ad39f", first: 100) {
        edges {
          node {
            name
            party: currentMemberships(classification:"party") {
              organization {
                name
                
              }
            }
            links {
              url
            }
            sources {
              url
            }
            chamber: currentMemberships(classification:["upper", "lower"]) {
              post {
                label
              }
              organization {
                name
                classification
                parent {
                  name
                }
              }
            }
          }
        }
      }
    }


Search for legislators that represent a given area
---------------------------------------------------

`See in GraphQL <https://openstates.org/graphql?operationName=null&query=%7B%0A%20%20people(latitude%3A%2040.7460022%2C%20longitude%3A%20-73.9584642%2C%20first%3A%20100)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20chamber%3A%20currentMemberships(classification%3A%5B%22upper%22%2C%20%22lower%22%5D)%20%7B%0A%20%20%20%20%20%20%20%20%20%20post%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20label%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20organization%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20classification%0A%20%20%20%20%20%20%20%20%20%20%20%20parent%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A>`__

::

    {
      people(latitude: 40.7460022, longitude: -73.9584642, first: 100) {
        edges {
          node {
            name
            chamber: currentMemberships(classification:["upper", "lower"]) {
              post {
                label
              }
              organization {
                name
                classification
                parent {
                  name
                }
              }
            }
          }
        }
      }
    }
