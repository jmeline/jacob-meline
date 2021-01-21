---
title: Functional Programming - Curry functions
date: "2020-10-05"
description: "Learn about currying!"
---

##What is Currying?

Currying is an advanced technique of working with functions. Functions that take more than one argument are reduced down into a collection functions that each take one argument and return one argument.

Take function f which takes in arguments a,b,c. We'll denote the curried f as fc

```js
const f = (a,b,c) => { /* … */ }
const fc = a => b => c => { /* … */ }

```

Curried functions allow for interesting implementations:


1) Functions can create new functions

This simple add function can be used to create new functions

```js

const add = a => b => a + b
const add3 = add(3)
console.log(add3(7)) // 10
```

```js
// this is a function
const curry = (fns)
  => (...args)
  => fns.reduceRight((f, g) => f(g(args)))
```

