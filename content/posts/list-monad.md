+++
title = "List Monad in JavaScript"
author = ["Mekael Turner"]
date = 2023-10-07T00:00:00+08:00
tags = ["functional-programming", "monads", "javascript"]
draft = false
+++

## Thoughts {#thoughts}

I'm writing this more as a way to document and organize my thoughts around the list monad.

This is a very important monad.


## The Code {#the-code}

<a id="code-snippet--list-monad"></a>
```js
export const List = list => ({
  list,
  map: fn => List(list.map(fn)),
  fold: (fn, initialValue) => List(list.reduce(fn, initialValue)),
  foldMap(monoid, empty) {
    const mappedList = list.reduce(fn, initialValue);
    const result = mappedList.reduce(
      (accumalator, monoidValue) => accumalator.concat(monoidValue),
      empty,
    );
    return List(result);
  },
  extract: () => list,
});
```


### Example {#example}

Here is a sample implementation of this list monad.

```js
List([1, 2, 3, 4]).map(x => x + 1)
//> [2,3,4,5]

List(["red", "Blue", "Yellow", "Green"]).(str => str.toUpperCase())
//> ["RED", "BLUE", "YELLOW", "GREEN"]
```


### Github {#github}

A current version of my implementation of this list monad can be found [here](https://github.com/mekkamagnus/functional-library-javascript/blob/main/list.js).
