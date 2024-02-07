---
title: js_ajax
updated: 2023-10-12 19:27:18Z
---

## Goals

- Working with API's
- Intro to JSON
- Postman
- Making XHRs
- The fetch API
- Working with Axios

## Intro to AJAX

- Stands for Asynchronous Javascript and XML
- Unlike traditional web requests that reload entire web pages, AJAX allows for the retrieval or sending of specific data without refreshing the entire page.
  - Used to make asynchronous requests to load information or to send information.
- When an AJAX request is made, data is sent or received in a concise and efficient manner. Typically, data is exchanged using the JSON format, a lightweight data interchange format.

## Intro to API's

- API stands for Application Programming Interface and is commonly referred to as a WEB API.
  - It acts as an intermediary that allows different software applications to communicate with each other.
    - Examples:
      - **Twilio APIs:** Used for integrating messaging and calling functionalities into applications.
      - **OpenWeatherMap API:** Provides access to weather data, enabling developers to integrate real-time weather information into their applications.
      - **Twitter APIs:** Allows scheduling of tweets, integrates real-time search on websites/apps, tracks user engagement and tweet performance and many much more.
      - **Facebook APIs:** Enable integration with Facebook's features, such as user authentication and sharing capabilities.
      - **Reddit APIs:** Provide access to Reddit's data and functionalities, allowing developers to build applications that interact with the platform.
      - **Crypto APIs:** Cover a range of functionalities related to cryptocurrencies, including price tracking and market data.

## Intro to JSON

- Stands for Javascript Object Notation is a lightweight data interchange format consisting of key-value pairs. It's just pure data and doesn't contains any html, css like any typically website would have..
  - Key has to be a String in `"some_key"` quotes.
  - **APIs return data in JSON format**, which can be transformed into language-specific objects for further processing. For instance, if you receive JSON data, you can convert it using Java, Python, Ruby, or JavaScript, adapting it into a native equivalent object to facilitate seamless integration and manipulation within the respective programming language.  
- **Conversion Methods**
  - **JSON to Object:** Utilize `JSON.parse()` — Takes JSON data as an argument and returns an equivalent JavaScript object.
  - **Object to JSON:** Utilize `JSON.stringify()` -- Convert a valid JavaScript object into JSON format, making it suitable for data interchange.

    ```js
    // JS Object
    const stud = {
        name: "Recluze",
        rollNum: 892028332,
        personalDetails: {
            address: "NYC",
            postalCode: 1928
        },
        degree: "CS",
        semester: 5
    }

    // JS Object ===>>> JSON
    const result = JSON.stringify(stud);
    console.log(result);

    // JSON ===>>> JS Object
    const jsString = JSON.parse(result);
    console.log(jsString);
    ```

## HTTP Verbs

| HTTP Verb | Purpose                                             | Example                           |
|-----------|-----------------------------------------------------|-----------------------------------|
| GET       | Retrieve data from the specified resource           | `GET /api/users`                  |
| POST      | Submit data to be processed to a specified resource | `POST /api/users`                 |
| PUT       | Update or create a resource                         | `PUT /api/users/123`              |
| PATCH     | Partially update a resource                         | `PATCH /api/users/123`            |
| DELETE    | Delete the specified resource                       | `DELETE /api/users/123`           |
| OPTIONS   | Retrieve information about communication options    | `OPTIONS /api/users`              |
| HEAD      | Same as GET but without the response body           | `HEAD /api/users/123`             |
| TRACE     | Performs a message loop-back test                   | `TRACE /path/to/resource`         |
| CONNECT   | Establishes a tunnel to the server                  | `CONNECT example.com:80`          |

## HTTP Status Codes

- 200 - OK
- 300 - Redirection Messages
- 400 - Client Error Responses
- 500 - Server Error Responses

| Status Code | Description                            | Example                                   |
|-------------|----------------------------------------|-------------------------------------------|
| 200         | OK                                     | Successful GET request                   |
| 201         | Created                                | Successful POST request                  |
| 204         | No Content                             | Successful request with no response body -- *HEAD* request|
| 400         | Bad Request                            | Malformed request or invalid parameters  |
| 401         | Unauthorized                           | Authentication required                  |
| 403         | Forbidden                              | Access to the resource is forbidden      |
| 404         | Not Found                              | Resource not found                       |
| 500         | Internal Server Error                  | Generic server error                     |

## Query Strings

- Query strings are **appended to a URL to pass additional information** to the server.
  - **Structure:** It Starts with a question mark `?` and is followed by key-value pairs separated by ampersands `&`. Each key-value pair is in the form `key=value`.
    For example:

    ```js
    https://api.example.com/data?name=John&age=25&city=NewYork
    ```

    - In the provided example
      - `name`, `age`, and `city` are keys.
      - `John`, `25`, and `NewYork` are their respective values.

## HTTP Headers

- HTTP headers provide additional information about the request or response.

### Request Headers

| Header         | Description                                     | Example                                       |
| -------------- | ----------------------------------------------- | --------------------------------------------- |
| User-Agent     | Identifies the user agent making the request    | `User-Agent: Mozilla/5.0 ...`                  |
| Accept         | Informs the server about accepted media types   | `Accept: application/json`                    |
| Authorization  | Contains credentials for client authentication | `Authorization: Bearer <token>`              |

### Response Headers

| Header         | Description                                     | Example                                       |
| -------------- | ----------------------------------------------- | --------------------------------------------- |
| Content-Type   | Specifies the media type of the resource         | `Content-Type: application/json`              |
| Cache-Control  | Directs how caching should be done               | `Cache-Control: max-age=3600` (in seconds)                 |
| Set-Cookie     | Sets a cookie in the browser                    | `Set-Cookie: sessionId=abc123; Path=/; ...`  |

### General Headers

| Header         | Description                                     | Example                                       |
| -------------- | ----------------------------------------------- | --------------------------------------------- |
| Date           | Represents the date and time of the message     | `Date: Tue, 29 Jun 2023 08:45:22 GMT`        |
| Connection     | Specifies control options for the connection    | `Connection: keep-alive`                      |
| Server         | Provides information about the origin server    | `Server: Apache/2.4.41 (Unix)`                |

## XMLHttpRequest

- `XMLHttpRequest` allows making both synchronous and asynchronous HTTP requests. Asynchronous requests are more common and are used to prevent the browser from freezing while waiting for the response.
  - It's the **original way** of sending requests via Javascript.
  - Syntax is:

  ```js
  const xhr = new XMLHttpRequest();
  xhr.open('GET', 'https://api.example.com/data', true); // true for asynchronous
  xhr.send();
  ```

- `XMLHttpRequest` follows an **event-driven model**. We can define event listeners for various events like `load`, `error`, `abort`, etc., to handle different stages of the request lifecycle.

  ```js
  xhr.onload = function() {
    if (xhr.status >= 200 && xhr.status < 300) {
      // Successful response
      console.log(xhr.responseText);
    } else {
      // Handle errors
      console.error('Request failed:', xhr.statusText);
    }
  };
  ```

- `XMLHttpRequest` supports different HTTP methods like GET, POST, PUT, DELETE, etc.

  ```js
  xhr.open('POST', 'https://api.example.com/create', true);
  xhr.setRequestHeader('Content-Type', 'application/json');
  xhr.send(JSON.stringify({ key: 'value' }));
  ```

### Limitations

- `XMLHttpRequest` doesn't supports `Promises`, so we can only work with callbacks.
- `XMLHttpRequest` can be verbose and requires several lines of code for a simple request.

### XMLHttpRequest Demo

  ```js
  // 1. Create a new XMLHttpRequest Object
  const req = new XMLHttpRequest();

  // 2. Attach the event-handler callbacks
    // onload ===>> in case of success
    // onerror ===>>> in case of failure / error
      // If we've dependent actions that only needs to be done after response
      // we can create a new XMLHttpRequest object inside onload callback
      // which may lead to a callback hell.
  req.onload = function (){
    console.log(`It worked`);
    console.log(this);  // prints the `req` object
    console.log(`Response: ${this.responseText}`);
    const data = JSON.parse(this.responseText);
    // don't print Objects inside a Template literals as Objects would be 
    // converted into Strings by literals and you will get [object] printed.
    console.log(`JSON Response: `, data); 
  }

  req.onerror = function (){
    console.log(`ERROR!!!`);
    console.log(this); // `this` means the request object`
  }

  // 3. Open the request object and set the method and url
  req.open("GET", "https://swapi.dev/api/people/1/", true); // true for async. req.
  req.setRequestHeader('Accept', 'application/json');
  // 4. Send the request
  req.send();  

  // `req` object contains a lot of information such as response, response text
  // url, status code. But our data lies in response text
  ```

## Fetch API / Function

- Improved way of making requests in JS and is based on  `Promises` making it easier to work with asynchronous code.
  - Provides a simpler and more powerful way to make HTTP requests compared to the older `XMLHttpRequest`.
- **Basic Fetch Example**
  
  ```js
  // Make a GET request to a URL
  fetch('https://swapi.dev/api/people/1')
    .then(response => {
      if (!response.ok) {
        throw new Error('Network response was not ok');
      }
      // Parse the response as JSON (see after code)
      return response.json();
    })
    .then(data => {
      // Work with the JSON data
      console.log('JSON Response (1):', data);
      // We can make another request
      return fetch('https://swapi.dev/api/people/2');
    })
    .then(response => {
      if(!response.ok){
        throw 'Network Error!!!'
      }
      return response.json();
    })
    .then(data => {
      console.log(`JSON Response (2): `, data)
    })
    .catch(error => {
      // Handle errors
      console.error('Fetch error:', error);
    });
  ```
  
  - `fetch` function returns a `Promise` that resolves with the Response object representing the response to the request as soon as headers are received.
  - The body of the response is a `ReadableStream`, which can be processed in various ways, such as calling `json()` on fetch response object for JSON data.

- **Fetch with Async Functions**

  ```js
  const loadStarWarsPeople = async () => {
    const response = await fetch('https://swapi.dev/api/people/1');
    const data = await response.json();
    console.log(data);
  }

  loadStarWarsPeople();
  ```

## AXIOS

- A library for making HTTP requests
  - For server side use, install it using `npm`
    - `npm install axios`
  - For client side usage, we can use JS CDN
    - For client-side usage, include the Axios CDN **before** your main script:
    - `<script src="https://cdn.jsdelivr.net/npm/axios@1.1.2/dist/axios.min.js"></script>`
- Axios have several syntax improvements over the `fetch` such as for our scenario, when we make a get request we don't have to convert the response object to JSON using `json()` method.
  - Axios shines here, for the automatically parsing JSON responses and many more.

### Basic Usage

  ```js
  // After setting up the axios using js cdn
  axios.get('https://swapi.dev/api/people/2')
  .then(res => {
    console.log(res.data )
  })
  .catch(err => console.log(err));
  ```

- It makes the request and parses the data to `JSON` for us
- We didn't have to convert to the `ReadableStream` to `json`

### Asynchronous Function with Axios

  ```js
  const loadStarWarPersonDetails = async (id) => {
    const res = await axios.get(`https://swapi.dev/api/people/${id}/`);
    await console.log('in the then block');
    console.log(res.data);
  }

  loadStarWarPersonDetails(2);
  ```

### Configuring Headers

- Some APIs require you to set up headers in the request to give the desired result. One such API `icanhazdadjoke.com/` returns by default an `text/html` response which isn't something useful.
  - It has an option to return `json` if we setup the headers to `application/json`. Now, we'll be doing that
    - `axios` accepts a `config` parameter which in turn has an `headers` object where we can specify the headers for the request.

  ```js
  const getDadJokes = async () => {
    const config = { headers: {Accept: 'application/json'}};
    const response = await axios.get('https://icanhazdadjoke.com', config);
    console.log(response.data.joke);
  }
  getDadJokes()
  ```

### Manipulating the DOM with Axios

- We can go next level by manipulating the DOM by adding new `li` inside a `ul` and attach an eventhandler to `button`. Whenever a button is clicked `getDadJokes()` will be called and the response will be appended to the `ul` by creating a new `li` and inserting the jokes text inside the newly created `li`.

    ```js
    const addJokes = async () => {
      // we've to wait for the getDadJokes() to complete
      // otherwise, there won't be any jokes in the jokeText
      // instead there'll be only a pending object
      const jokeText = await getDadJokes();
      const jokesUL = document.querySelector('#jokes');
      const newJoke = document.createElement('LI');
      newJoke.append(jokeText);
      jokesUL.append(newJoke);
    }

    const getDadJokes = async () => {
      try{
        const config = { headers: {Accept: 'application/json'}};
        const response = await axios.get('https://icanhazdadjoke.com', config);
        return response.data.joke;
      } catch (error) {
        return 'NO JOKES AVAILABLE!!! SORRY :(';
      }
    }
    ```
