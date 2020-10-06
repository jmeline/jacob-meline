---
title: Fun Functions
date: "2020-10-05"
description: "Here are some fun functions that I like"
---

```js
// this is a function
const curry = (fns)
  => (...args)
  => fns.reduceRight((f, g) => f(g(args)))
```
