---
title: React - Fundamentals
date: "2020-10-05"
description: "Things I've learned from Kent's React Fundamental's workshop"
---

### Introduction to native react calls

 Going back to the basics with React in order to gain a better foundation on how React works.

JSX elements are syntactic sugar for calling:

```jsx
React.createElement(component, props, ...children)
```

Code written in JSX such as the following:

```jsx
function Hi({ name }) {
  return <div> hello {name} </div>
}
```

Is converted to the following thanks to [babel](https://babeljs.io/repl/#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&spec=false&loose=false&code_lz=GYVwdgxgLglg9mABACxgCgN6LAQwLYCmiAvgJSIYBQiiATgVCLUgDwAmMAbgHzIEA2_OBVyFiLAPQceAbkrFKQA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=false&targets=&version=7.11.6&externalPlugins=):

```jsx
function Hi({
  name
}) {
  return /*#__PURE__*/
    React.createElement(
      "div", null, "hello ", name);
}
```

Pretty neat huh?

### How do you use it to build react applications?

Well, let's be honest here for a second. This function is cool and all but it becomes pretty hard to follow when we begin to build more components. For example, how would I represent this html in React.


```html
<html>
  <body>
    <div id="main">
      <div class="main-class">
        <div>Hello</div>
      </div>
      <div>World</div>
    </div>
  </body>
</html>
```

Becomes (really not pretty and it isn't sustainable for large react apps.)

```html
<html>
  <body>
    <div id="root" />
  </body>
  <script>

    const helloDiv = React.createElement("div", {
      children: "Hello"
    })

    const mainClassDiv = React.createElement("div", {
      className: "main-class",
      children: helloDiv
    })

    const worldDiv = React.createElement("div", {
      children: "World"
    })

    const mainDiv = React.createElement("div", {
      id: "main",
      children: [
        mainClassDiv,
        worldDiv
      ]
    })
    ReactDOM.render(mainDiv, document.getElementById("root"))
  </script>
```



