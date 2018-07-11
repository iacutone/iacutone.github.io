---
layout: post
title: Understanding Union Types in Elm
date: 2018-04-16 18:19:13 -0500
comments: true
categories: elm
---

Watching [Making Impossible States Impossible](https://www.youtube.com/watch?v=IcgmSRJHu_8) and reading [Higher Level Coding with Elm Union Types](https://becoming-functional.com/higher-level-coding-with-elm-union-types-2502d1f5a615) were enlightening in my Elm development. Now, I look for places were I can refine my data model by using union types. Union types allow me to replace conditionals with pattern matching case statements. This pattern is much cleaner and easier to understand. The following is an example of how I used union types to refactor a hamburger.

## Elm Model

```haskell
-- BEFORE

type Msg
    = DisplayHamburgerItems

type alias Model = 
    { hamburger_open : Bool
    }

-- AFTER

type Msg
    = DisplayHamburgerItems Hamburger

type Hamburger
    = Open
    | Closed


type alias Model = 
    { hamburger : Hamburger
    }
```

## Elm Update
```haskell
update : Msg -> Model -> ( Model, Cmd Msg )
update msg model =

    -- BEFORE

    case msg of
        DisplayHamburgerItems ->
            if model.hamburger_open == True then
                let
                    items = List.append model.display_hamburger ["About", "Contact", "Menu"]
                in
                    ( { model | display_hamburger = items, hamburger_open = False }, Cmd.none )
            else
                ( { model | display_hamburger = [], hamburger_open = True }, Cmd.none 


    -- AFTER

        DisplayHamburgerItems msg ->
            case msg of
                Open ->
                    let
                        items = List.append model.display_hamburger ["About", "Contact", "Menu"]
                    in
                        ( { model | hamburger = Open, display_hamburger = items }, Cmd.none )
                Closed ->
                    ( { model | hamburger =  Closed, display_hamburger = [] }, Cmd.none )
```

## Elm View
```haskell
view : Model -> Html Msg
view model =

    -- BEFORE

    i [ onClick (DisplayHamburgerItems) ] []

    -- AFTER

    i [ onClick (DisplayHamburgerItems (toggleHamburger model.hamburger)) ] []

viewHamburgerItems : Model -> Html Msg
viewHamburgerItems model =

    -- BEFORE

    if model.hamburger_open == False then
        div [] (List.map item model.display_hamburger)
    else
        div [] []

    -- AFTER

    case model.hamburger of
        Open ->
            div [] (List.map item model.display_hamburger)
        Closed ->
            div [] []
```

It is easier to reason about `Hamburger` with `Open` and `Closed` types than checking against a `Bool`. Pattern matching on the union type is expressive and is really helpful when the union types grow in complexity.


Full code [here](https://github.com/iacutone/menus).
