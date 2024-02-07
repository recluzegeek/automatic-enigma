---
title: express_RESTful_routes
updated: 2023-10-22 15:30:15
---

## Debug Express

- Debugging may be useful for this section `DEBUG=express:* node index.js`

## GET vs POST

There are two main HTTP request methods: **GET and POST**.

- **GET** requests are typically used to retrieve or get information.
  - They are commonly used for reading web pages, bookmarking, and searching.
    - Data is sent in the query string of the URL, which has limitations in terms of data size.
11
- **POST** requests are used to submit data that is important for the request.
  - Data is included in the request body, not in the query string.
    - This method allows for sending more data, including various formats like JSON, and is suitable for creating, updating, or deleting resources.

- While **GET** requests are conventionally used for retrieval and **POST** for submission, these are not strict rules. In practice, it's essential to follow the intended usage patterns.
  - **GET** requests should not have side effects on the server (e.g., no changes to data), while **POST** requests are suitable for actions that create, update, or delete data.

  - **POST** requests offer more flexibility in terms of data size and type, making them versatile for various tasks.
2
    |                  | Key Points of GET                                | Key Points of POST                           |
    |------------------|---------------------------------------------------|---------------------------------------------|
    | **Usage**            | Used to retrieve information                      | Used to post data to the server              |
    | **Data Transport**   | Data is sent via query string                     | Data is sent via request body, not a query string |
    | **Data Visibility**  | Information is plainly visible in the URL!        | Data is not visible in the URL              |
    | **Data Flexibility** | Limited amount of data can be sent                 | Can send any sort of data (including JSON)  |
    | **UseCases** | Search terms, filtering or sort by queries                 | Posting data that may or may not be secretive |

## Handling POST Requests in Express

- In Express, handling `POST` requests is quite similar to handling `GET` requests. However, there are some fundamental differences to keep in mind.

### Basic Concept

- When handling a `POST` request, we typically receive data from an HTML form. This data is sent as part of the request body via `req.body`.
  - In contrast, for a `GET` request, data is usually sent as query parameters appended to the URL, and it's accessible via `req.query`.

### Setting Up Express

- Before diving into handling `POST` requests, we need to set up Express application to parse the request data properly. Here's how you can configure Express to handle both form data and JSON data:

  ```js
  const express = require("express");
  const app = express();
  const port = 3000;

  // Middleware for parsing form data
  app.use(express.urlencoded({ extended: true }));

  // Middleware for parsing JSON data
  app.use(express.json());

  // Your route handlers go here

  app.listen(port, () => {
    console.log(`Listening on port ${port}`);
  });
  ```

#### Handling GET Requests

- In a `GET` request, the data is usually included in the URL's query parameters. Here's an example of handling a GET request for a resource called `/tacos`

  ```js
  app.get("/tacos", (req, res) => {
    const { meat, qty } = req.query;
    res.send(`Okay, here are your ${qty} ${meat} tacos received via the GET request.`);
  });
  ```

#### Handling POST Requests

- For `POST` requests, the data is sent in the request body. Here's an example of handling a POST request for the same `/tacos` resource.

  ```js
  app.post("/tacos", (req, res) => {
    const { meat, qty } = req.body;
    res.send(`Okay, here are your ${qty} ${meat} tacos received via the POST request.`);
  });
  ```

### Using Thunder Client

To test your `POST` request, use the Thunder Client extension in Visual Studio Code:

- Install and open Thunder Client.
- Create a `POST` request with the URL `http://localhost:3000/tacos`.
- Set the Content-Type header to `application/json`.
- In the request body, provide JSON data, such as:
  
  ```json
  {
    "meat": "CHICKEN",
    "qty": 32
  }
  ```

- Send the request and check the response.  

#### Complete Code Demonstration

  ```js
  const express = require("express");
  const path = require("path");
  const app = express();
  const port = 3000;

  app.use(express.urlencoded({ **extended**: true })); // for parsing form data
  app.use(express.json()); // for parsing application/json

  app.get("/tacos", (req, res) => {
    const { meat, qty } = req.query;
    res.send(`ok, here is your  ${qty} ${meat} tacos received on the get request...`);
  });

  app.post("/tacos", (req, res) => {
    const { meat, qty } = req.body;
    res.send(`ok, here is your ${qty} ${meat} tacos on the post request...`);
  });

  app.listen(port, (req, res) => {
    console.log(`listening on port ${port}`);
  });
  ```

## RESTful Routes

In Express, RESTful routes refer to a convention for defining routes to perform CRUD (Create, Read, Update, Delete) operations on resources. These routes correspond to the HTTP methods (GET, POST, PUT, DELETE) and are used to create a standardized API for interacting with data.

- `GET` requests:
  - `GET /resource`: Fetch all items.
  - `GET /resource/:id`: Fetch a specific item by ID.
- `POST` requests:
  - `POST` /resource: Create a new item.

- `PUT/PATCH` requests:
  - `PUT /resource/:id` or `PATCH /resource/:id`: Update a specific item by ID.

- `DELETE` requests:
  - `DELETE /resource/:id`: Delete a specific item by ID.

### `method_override`

- HTML `forms` can only submit `GET` or `POST` requests, limiting the ability to use other HTTP methods like `PUT` or `DELETE`. To work around this limitation, the `_method parameter` can be included in the form data to mimic other HTTP methods.

  ```html
  <form method="POST" action="/resource/:id?_method=DELETE">
    <!-- Other form fields -->
    <button type="submit">Delete</button>
  </form>
  ```

- Express provides middleware to support method override by examining the `_method` parameter and allowing the corresponding HTTP method to override the actual method:

  ```js
  const express = require('express');
  const methodOverride = require('method-override');

  const app = express();

  // Implementing method override
  app.use(methodOverride('_method'));

  // Routes
  app.delete('/resource/:id', (req, res) => {
    // Delete logic
  });

  // Other routes for GET, POST, PUT, etc.

  app.listen(3000, () => {
    console.log('Server is running on port 3000');
  });
  ```
