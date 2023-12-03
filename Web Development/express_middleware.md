# Express Middleware

- Middleware functions run during request/response lifecycle.
  - Middleware functions in Express are special functions that can interact with the incoming request (`req`) and the outgoing response (`res`).
    - These functions also have access to a callback function called `next` that allows them to **pass control to the next matching step** in the request-handling process.
- Middleware functions can be used for tasks such as **logging, authentication, error handling**, parsing request bodies, and more. They are **executed sequentially** in the order they are defined.

## Morgan

**Morgan** is a popular logging middleware for Express. It simplifies the process of logging HTTP requests and responses. It automatically logs important information about incoming requests, such as the HTTP method, status code, URL, response time, and more.

```js
const express = require('express');
const morgan = require('morgan');

const app = express();

// Use Morgan middleware for logging
app.use(morgan('dev')); // 'tiny' and 'common' are other predefined formats

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

- `const morgan = require('morgan');` &mdash; Import the Morgan middleware.
- `app.use(morgan('dev'));` &mdash; Use the Morgan middleware with the 'dev' format, which provides concise output for logging.

## Defining our Own Middleware

- While defining our own middleware in Express, we're essentially creating functions that have access to the `request`, `response`, and the `next` function in the middleware stack.
- In Express middleware, the `next` function is crucial for passing control to the next middleware function in the stack. 
  - However, it's essential to understand that once `next()` is called, the **subsequent code after it in the current middleware continues to execute**. 
  - If you want to terminate the current middleware and avoid the execution of code that follows `next()`, you should use `return next()`.
Here's a basic example of defining custom middleware in Express:

```javascript
const express = require('express');
const app = express();

// Custom middleware function
const myMiddleware = (req, res, next) => {
  console.log('This is my custom middleware!');
  // You can modify req or res here if needed
    // next(); // Call next to pass control to the next middleware in the stack
  return next();
  console.log("This won't be executed");
};

app.use((req, res, next) => {
    console.log('This is my Second Middleware');
    next(); // Call next to pass control to the next middleware in the stack
    console.log("This will execute");
})

// Use the custom middleware for all routes
app.use(myMiddleware);

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

In this example:

1. We define a custom middleware function `myMiddleware`.
2. Inside `myMiddleware`, we log a message and then call `next()` to pass control to the next middleware.
3. We use `app.use(myMiddleware)` to apply our custom middleware to all routes.

### Setting up 404 Page

- Setting up a **404 page** in Express involves creating a middleware function that handles requests for routes that are not found.
  - This middleware is typically placed at the end of the middleware stack so that it only gets **executed if no other route or middleware handles the request**.

```js
const express = require('express');
const app = express();

// Your other routes and middleware go here...

// Custom 404 middleware at the end of file
app.use((req, res, next) => {
  res.status(404).send('404 - Not Found');
});

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

## Protecting Routes

To protect specific routes in an Express application, use middleware to check whether a user is authenticated before allowing access to those routes. Here's a simple example using a fictional authentication mechanism:

```javascript
const express = require('express');
const app = express();

// Custom authentication middleware
const authenticate = (req, res, next) => {
  // Check if the user is authenticated 
  const isAuthenticated = /* logic to check authentication */ true;

  if (isAuthenticated) {
    // User is authenticated, proceed to the next middleware or route handler
    next();
  } else {
    // User is not authenticated, send an unauthorized response
    res.status(401).send('Unauthorized. You need a password to access this site.');
  }
};

// Route that requires authentication
app.get('/dashboard', authenticate, (req, res) => {
  res.send('Welcome to the Dashboard!');
});

// Other routes that do not require authentication
app.get('/', (req, res) => {
  res.send('Home Page');
});

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

In this example:

- The `/dashboard` route is protected and requires authentication. The `authenticate` middleware is used to check if the user is authenticated before allowing access to this route.
- The `/` route is not protected and can be accessed without authentication.
