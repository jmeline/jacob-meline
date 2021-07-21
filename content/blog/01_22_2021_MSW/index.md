---
title: Mock Service Worker
date: "2021-01-22"
description: "Get better at testing your Javascript api's"
---

After writing javascript tests for a number of years, I can say hands down testing my api's have been some of the most complicated parts of my testing. It isn't that mocking an api is difficult, it is just that I don't feel confident in my code because my code doesn't actually make the request.

## MSW

This library is pretty neat as you can use it to both handle requests/responses while developing an application locally or when unittesting your web applications. This tool is framework agnostic and is easy to setup.

Take a look at the [react example](https://github.com/mswjs/examples/tree/master/examples/rest-react)

### Initial setup

Create a home for your mocks. 

Create a couple files:
  * ```handler.js``` - this will contain your request handlers
  * ```browser.js``` - this will contain your local development browser setup
  * ```server.js``` -  this will contain your mocking server setup for your unittests

### Setup for developing locally ([docs](https://mswjs.io/docs/getting-started/integrate/browser))

Since MSW relies on the service worker running within the browser, a service worker must be added to your public directory. This is where you application is started from. If your application was created via ```create-react-app```, this will be your ```public``` folder

We first need to run the following npm init command to create the service worker that MSW will use.

```bash
$ npx msw init public/ --save
```

Within browser.js, add the following setup:

```js
import { setupWorker } from 'msw'
import { handlers } from './handlers'

export const worker = setupWorker(...handlers)
```

setupWorker will utilize the service worker created by the previous command

Modify your react's ```index.js ``` and include this before the ReactDom.render call

```js
// imports removed for brevity

// Start the mocking conditionally.
if (process.env.NODE_ENV === 'development') {
  const { worker } = require('./mocks/browser')
  worker.start()
}

ReactDom.render(<App />. document.getElementById('root'))
```


