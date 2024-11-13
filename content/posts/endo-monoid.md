+++
title = "The Endo Monoid"
author = ["Mekael Turner"]
date = 2023-10-08T00:00:00+08:00
tags = ["functional-programming", "monoid", "javascript"]
draft = false
+++

## Overview {#overview}

Endo can be used as a form of composition by concating functions. In this concatination functions results are fed into the concatted function.

The value is fed into Endo's run function providing opportunities for lazy operations.

I'm purposely being extremely explicit because I make to make sure that I understand each step of these functional implementations.
\## Examples


#### Example 1 {#example-1}

```js
// Endo monoid declaration
// Endo :: (a -> a) -> Endo a
const Endo = run => ({
    run,
    // concat :: Endo a -> Endo a -> Endo a
    concat: other =>
    Endo(x => run(other.run(x)))
})

// Endo.empty :: () -> Endo a
Endo.empty = () => Endo(x => x)

// List monad for putting functions into a list.
// List :: [a] -> List a
const List = list => ({
    list,

    // map :: (a -> b) -> List a -> List b
    map: fn => List(list.map(fn)),

    // fold :: ((b, a) -> b, b) -> List a -> List b
    fold: (fn, initialValue) => List(list.reduce(fn, initialValue)),

    // foldMap :: (a -> b) -> b -> List a -> b
    foldMap: (monoid, empty) => {
        return empty != null
            ? list.reduce((acc, x, i) => acc.concat(monoid(x, i)), empty)
            : list.map(monoid).reduce((acc, x) => acc.concat(x))      },

    // extract :: () -> [a]
    extract: () => list,
})

// Utility functions

// compose :: (b -> c) -> (a -> b) -> a -> c
const compose = f => g => x => f(g(x))

// curry :: ((a, b) -> c) -> a -> b -> c
const curry = f => a => b => f(a, b)

// toUpper :: String -> String
const toUpper = str => str.toUpperCase()

// exclaim :: String -> String
const exclaim = str => `${str}!!`

// to OrgHeader :: String -> String
const toOrgHeader = str => `* ${str}`

// toOrgTagList :: [String] -> String
const toOrgTagList = xs => List(xs)
      .map((tag, index) => !index ? `:${tag}:` : `${tag}:` )
      .fold((accumalator,tag, index) => accumalator.concat(tag) ,'')
      .extract()

// Tag list for header
const tags = ['javascript', 'completed', 'moved']

// addTagsToHeader :: [String] -> String -> String
const addTagsToHeader =
      curry((tagList,str) => `${str} ${tagList}`)

console.log(addTagsToHeader(toOrgTagList(tags))("A Header"))

const result = List([addTagsToHeader(toOrgTagList(tags)),toOrgHeader, toUpper, exclaim]).foldMap(Endo, Endo.empty('')).run('hello')
//> A Header :javascript:completed:moved:

console.log(result)
//> HELLO!! :javascript:completed:moved:
```
