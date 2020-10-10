---
title: React - Fundamentals
date: "2020-10-05"
description: "Things I've learned from Kent's React Fundamental's workshop"
---

 Going back to the basics with React in order to gain a better foundation on how React works.

JSX elements are syntactic sugar for calling:

```js
React.createElement(component, props, ...children)
```

Code written in JSX such as the following:

```jsx
function hi({ name }) {
  return <div> hello {name} </div>
}
```

Is converted to the following thanks to [babel](https://babeljs.io/repl/#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&spec=false&loose=false&code_lz=GYVwdgxgLglg9mABACxgCgN6LAQwLYCmiAvgJSIYBQiiATgVCLUgDwAmMAbgHzIEA2_OBVyFiLAPQceAbkrFKQA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=false&targets=&version=7.11.6&externalPlugins=):

```js
function hi({
  name
}) {
  return /*#__PURE__*/
    React.createElement(
      "div", null, "hello ", name);
}
```



