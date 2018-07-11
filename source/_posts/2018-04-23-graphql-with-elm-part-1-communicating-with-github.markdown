---
layout: post
title: GraphQL with Elm Part 1, Communicating with GitHub
date: 2018-04-23 10:40:40 -0500
comments: true
categories: elm graphql github
---

A co-worker and are using GitHub's GraphQL API for a side project. I am writing a series of posts around what we learned from creating the application entitled 'GraghQL and Elm'. This post will detail how to communicate with the Github GraphQL API with Elm. All code related to these posts can be found [here](https://github.com/iacutone/github-elm).

GitHub has extensive [documentation](https://developer.github.com/v4/) on communicating with their GraphQL server. In this post we will write a simple query and display the JSON from Github. In the next post, we will use Elm Decoders in order to turn the JSON into Elm data. 

The [GitHub GraphQL Explorer](https://developer.github.com/v4/explorer/) is useful for crafting queries. Let's write a query that fetches a user's id:

```json
{
  user(login:"iacutone") {
    id
  }
}
```

With the aforementioned JSON, let's contruct a request to GitHub in Elm!

```haskell
query : String
query =
    """
    query {
      {
        user(login:"iacutone") {
          id
        }
      }
    }
    """

baseUrl : String
baseUrl =
    "https://api.github.com/graphql"

bearerToken : String
bearerToken =
    "Bearer <your GitHub token here>"

request : Http.Request String
request =
    Http.request
	{ method = "POST"
	, headers = [Http.header "Authorization" bearerToken]
	, url = baseUrl
	, body = Http.jsonBody (Encode.object [("query", Encode.string query)])
	, expect = Http.expectString
	, timeout = Nothing
	, withCredentials = False
	}
```

After posting the query to GitHub, your Elm update function will receive the following response:

```json
{
  "data": {
    "user": {
      "id": "MDQ6VXNlcjE1NjMyMDE="
    }
  }
}
```

I find it helpful to define this response as an Elm `String` type and view it in the browser. If there is an error with the query, you can see the results in the browser.

```haskell
-- MODEL

type alias Model =
    { response : String
    }

-- UPDATE

type Msg
    = GraphQLQuery (Result Http.Error String)

update : Msg -> Model -> ( Model, Cmd Msg )
update msg model =
    case msg of
        GraphQLQuery (Ok res) ->
            ( { model | response = res }, Cmd.none )

        GraphQLQuery (Err res) ->
            ( { model | response = toString res }, Cmd.none )

initialModel : Model
initialModel =
    { response = ""
    }

init : (Model, Cmd Msg)
init =
    ( initialModel, Http.send GraphQLQuery request )

main : Program Never Model Msg
main =
    Html.program
        { init = init 
        , view = view
        , update = update
        , subscriptions = \_ -> Sub.none
        }

-- VIEW

view : Model -> Html Msg
view model =
    div [] [ text model.response ]
```
The code above will display a stringfied version of our query. In Part 2, we will transform the Elm `String` data object into something more useful with Elm decoders.

