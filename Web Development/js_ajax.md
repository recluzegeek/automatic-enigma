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
        **Twilio APIs:** Used for integrating messaging and calling functionalities into applications.
        **OpenWeatherMap API:** Provides access to weather data, enabling developers to integrate real-time weather information into their applications.
        **Twitter APIs:** Allows scheduling of tweets, integrates real-time search on websites/apps, tracks user engagement and tweet performance and many much more.
        **Facebook APIs:** Enable integration with Facebook's features, such as user authentication and sharing capabilities.
        **Reddit APIs:** Provide access to Reddit's data and functionalities, allowing developers to build applications that interact with the platform.
        **Crypto APIs:** Cover a range of functionalities related to cryptocurrencies, including price tracking and market data.

## Intro to JSON

- Stands for Javascript Object Notation is a lightweight data interchange format consisting of key-value pairs. It's just pure data and doesn't contains any html, css like any typically website would have..
  - Key has to be a String in `"some_key"` quotes.
  - APIs return data in JSON format, which can be transformed into language-specific objects for further processing. For instance, if you receive JSON data, you can convert it using Java, Python, Ruby, or JavaScript, adapting it into a native equivalent object to facilitate seamless integration and manipulation within the respective programming language.  
- **Conversion Methods**
  - **JSON to Object:** Utilize `JSON.parse()` — a static method in JavaScript—by passing JSON data as an argument. This method returns an equivalent JavaScript object.
  - **Object to JSON:** Use `JSON.stringify()` to convert a valid JavaScript object into JSON format, making it suitable for data interchange.

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
