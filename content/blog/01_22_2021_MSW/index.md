---
title: Mock Service Worker
date: "2021-01-22"
description: "Get better at testing your Javascript api's"
---

After writing javascript tests for a number of years, I can say hands down testing my api's have been some of the most complicated parts of my testing. It isn't that mocking an api is difficult, it is just that I don't feel confident in my code because my code doesn't actually make the request.

## Service workers

In order to better understand the benefits MSW brings to the table, I feel it is good to provide an introduction into Service workers.

Have you ever had the experience where you've been browsing a website and you are met with "This page could not be loaded"? Playing the no Internet dinosaur game is fun for only so long. We've been there. Isn't there a better way to provide rich offline experiences? This scenario is avoidable because of Service workers. Service workers are a core part of Progressive Web Apps. PWA's simply mean that a website behaviors more like an application. A major benefit of using service workers is their ability to support rich offline experiences.

* JS script that is registered with the browser
* Stays registered even when the browser is offline
* Can load content without a connection
A few notes:
  * Service workers runs on a separate thread, managed by the browser.
    - It doesn't have access to the DOM
    - Programmable network proxy
    -
  * Service workers use promises heavily!
    - waiting for responses of network requests
  * Service workers can only be registered to pages that are served over HTTPS. (mitigates man-in-the-middle-attacks)
    - served over localhost is fine.
  * Work with modern browsers (Sorry IE)


#### MSW

1) testing apps
