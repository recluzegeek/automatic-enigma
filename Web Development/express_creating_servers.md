---
title: express_creating_servers
updated: 2023-10-18 21:04:48Z
---

## Installing Express

- Initialize a `package.json` via `npm init`
- Install express via `npm install express` or `npm i express`

### Creating and Starting Server

```js
// import express and get an instance of express
const express = require("express");
const app = express();
// different port on different server
const port = 3000;

// Define routes and middleware

// start the server
app.listen(port, () => {
  console.log(`Server started on http://localhost:${port}`);
});
```

## Understanding Request & Response Objects

- When working with Express, each incoming `request` triggers a callback function that automatically receives two parameters: the `request` object, representing the incoming data, and the `response` object, used to send a response back to the requester.
- **Request Object**
  - The `request` object is created by Express, representing the incoming HTTP request. It contains information about the client's request, such as parameters, headers, and the request body.
  - Express parses the incoming HTTP request into a JavaScript object and passes it as the first argument to the callback function.
- **Response Object**
  - The `response` object is also created by Express and is used to send a `response` back to the client. It provides methods to send data in various formats such as HTML, JSON, or plain text.
- **Order of Route Execution**
  - Express processes routes in the order they are defined in the file. Once it finds a matching route, it stops processing for additional matches. This **emphasizes the importance of defining more specific routes before more general ones.**
  - In Express, once a `response` is sent for a particular route, it concludes the handling of that specific `request`, and subsequent code or `responses` won't affect it.
- **Response Headers**
  - Express automatically sets `response` headers based on the type of `response` being sent. For example, if the `response` is a string, it sets the content type to text. If the `response` includes HTML tags, it sets the content type to HTML.

## Routing

- Refers to taking some incoming request and a path and matching / handling it to the responsible `code response`
  - Different content for different incoming requests
  - `app.get()` - expects two things, a path and the callback function
  - All other HTTP verbs behaves similar.

    ```js
    const express = require('express');
    const app = express();
    const port = 3000;

    // Basic route for the root path
    app.get('/', (req, res) => {
        console.log("Incoming Request:");
        console.log(req);

        // Send an HTML response
        res.send('<h1>This is HomePage...</h1>');
    });

    app.get('/cats', (req, res) => {
        res.send(`MEOW!!!`);
    });

    app.get('/dogs', (req, res) => {
        res.send(`WOOF!!!`);
    });

    app.get('*', (req, res) => {
        res.send(`<h3>I don't know this Path!</h3>`)
    })

    // Middleware for handling incoming requests
    app.use((req, res, next) => {
        console.log("New Incoming Request...");
        // Call the next middleware in the stack for 
        // proper middleware chain execution.
        next();
    });

    // Start the server
    app.listen(port, () => {
        console.log(`Server started on http://localhost:${port}`);
    });
    ```

## Express Path Parameters

- In Express, path parameters allow you to extract values from the URL, enabling dynamic routing.
  - Path parameters are placeholders in a route path that capture values from the actual URL. These parameters are useful for creating dynamic routes that can handle varying data.

### Syntax for Path Parameters

- Path parameters are specified in the route path using a colon syntax, like `:parameterName`. For example, `/users/:userId`.

### Accessing Path Parameters

- In the route handler, you can access the values of path parameters using `req.params.parameterName`. For instance, if the parameter is `userId`, you would use `req.params.userId`.

### Type Conversion

- Values extracted from **path parameters are typically strings**. If you <u>expect a numeric value</u>, make sure to convert it to the desired type using functions like `parseInt` or `parseFloat`.

### Path Parameter Naming

- Choose meaningful and consistent names for path parameters to enhance code readability. For instance, use `/users/:userId` instead of `/users/:id`.

### Code Demonstration

- Suppose we have a Reddit-like application with posts, and we want to create a route that fetches details about a specific post based on its ID.

  ```js
  const express = require('express');
  const app = express();
  const port = 3000;

  app.get('/', (req, res) => {
    res.send(`<h1>This is Homepage!!!</h1>`);
  });

  app.get('/about', (req, res) => {
    res.send(`<h2>About Us!</h2>`);
  });

  app.get('/r/:subreddit', (req, res) => {
    const { subreddit } = req.params;
    res.send(`You're viewing subreddit ${subreddit}`);
  });

  app.get('/r/:subreddit/post/:id', (req, res) => {
    const { subreddit, id } = req.params;
    res.send(`<h3>You're viewing Post Number ${id} on sub-reddit ${subreddit}`);
  });

  app.get('*', (req, res) => {
    res
      .status(404)
      .send(`<h1>I don't know this Path....</h1>`);
  });

  app.listen(port, () => {
    console.log(`listening on port ${port}`);
  });
  ```

## Working with Query Strings

- Query strings are a way to pass data to a web server as part of a URL.
  > They are commonly used in HTTP requests to send additional information or parameters to the server.
  >
  >> Query strings appear at the end of a URL and are separated from the base URL by a question mark (`?`).
- In Express, the `req.query` object contains key-value pairs of the query string parameters.
  - In the provided example, the route `/search` is defined to handle GET requests, and it expects a query parameter named q (e.g., `/search?q=node&color=black`).

  ```js
  // adding search route using query string
  app.get('/search', (req, res) => {
    // if we've a 'query string' of the form
    // https://server.com/search?q=node&color=black
    // key: q , value: node
    // key: color , value: black
    const { q } = req.query;
    console.log(q);
    res.send(`Showing search results for: ${q}`);
  });
  ```

## Nodemon

- Monitor for any changes in the `node.js` application and automatically restart the server.
  - Helpful during development. We doesn't have to restart the server manually. nodemon will do it for us on every file saving operation.
  - Install it using `npm i -g nodemon`
  