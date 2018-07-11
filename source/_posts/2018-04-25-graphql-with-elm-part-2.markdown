---
layout: post
title: GraphQL with Elm Part 2, Parsing Nested JSON
date: 2018-04-25 14:17:04 -0500
comments: true
categories: elm graphql json
---

In this post we will discuss how to turn nested JSON into Elm data types. This post uses a thin `Elm.Http` [wrapper](https://github.com/NoRedInk/elm-decode-pipeline). Using this library helps us concentrate on contructing the necessary Decoders. We will also contruct a more complex query in order to explain how to contruct complex data types. The following query returns JSON for the first two team members of FracturedAtlas.


```haskell
query : String
query =
    """
    query {
      organization(login: "FracturedAtlas") {
        team(slug: "fa-dev") {
          members(first:2) {
            edges {
              node {
                id
                login
              }
            }
          }
        }
      }
    }
    """
```

Swapping out our query will return the following JSON.

```json
{"data":{"organization":{"team":{"members":{"edges":[{"node":{"id":"<user id>","login":"<user name>"}},{"node":{"id":"<another user id>","login":"<user name>"}}]}}}}}
```

The following part took me a long time to grok. The issues I had difficulty resolving are:
1. How do I grab the list of nodes?
1. How to I change my stringified JSON output into Elm data types?

The rest of this post tackles these two issues. 

The `requiredAt` function from Elm Decode Pipeline solves the first problem. 

```haskell
decodeLogin =
    decode Users
            |> requiredAt ["data", "organization", "team", "members", "edges"] (Json.Decode.list decodeNode)
	    -- 'dig' into the JSON and extract the node list

decodeNode =
    decode Node
        |> required "node" decodeUser

decodeUser =
    decode User
        |> required "id" string
        |> required "login" string
```

Let's construct our Elm datatypes based on the JSON response.

```haskell
type alias Model =
    { message : String
    , users : Maybe Users
    }

type alias Users =
    { edges : List Node }

type alias Node =
    { node: User
    }

type alias User =
    { id:  String
    , login : String
    }
```

Now to handle the update function.

```haskell
update : Msg -> Model -> ( Model, Cmd Msg )
update msg model =
    case msg of
        GraphQLQuery (Ok res) ->
            case decodeString decodeLogin res of
                Ok res ->
                    ( { model | users = Just res }, Cmd.none )
                Err error ->
                    ( { model | message = error, users = Nothing }, Cmd.none )

        GraphQLQuery (Err res) ->
            ( { model | users = Nothing }, Cmd.none )
```

I spent numerous hours figuring out how to map the Elm data types to the decoders. In Part 3, we will learn how to display the `User` data type in the browser.
