---
title: js_async_javascript
updated: 2023-10-12 13:23:41Z
---

## Goals

- Call Stack
- Understanding WebAPI's
- Callback Hell
- Working with Promises
- Creating our own Promises
- Async Functions

## Call Stack

- A computer science student knows, what exactly is a call stack. It keeps track of function calls in the code, ensuring they are executed in the correct order.
  - We can visualize the JS call stack using Chrome / Firefox debugger tool or some other online site like `loupe`.

1. **Function Calls**: When a function is invoked, a new frame is added to the call stack. This frame represents the function call and includes information like the function's parameters and local variables

    ```javascript
    foo(); // Call to foo adds a frame to the call stack

    function foo() {
      // function body
    }
    ```

    - In this example, calling `foo()` adds a frame to the call stack to keep track of the execution of the `foo` function.

1. **Execution**: The function at the top of the stack is executed. If this function calls another function, a new frame is added to the stack to represent the newly called function.

    ```javascript
    foo(); // Call to foo, which then calls bar

    function foo() {
      bar(); // Adds a frame for bar to the stack
    }

    function bar() {
      console.log("Inside bar");
    }
    ```

    - Here, when `foo` is called, a frame for `foo` is added to the stack. When `foo` calls `bar`, a new frame for `bar` is added on top of the stack.

1. **Completion**: When a function completes its execution, its frame is removed from the stack. This happens in a Last-In-First-Out (LIFO) manner, meaning the last function added is the first one to be removed.

    ```javascript
    foo(); // Call to foo, which then calls bar


    function foo() {
      bar(); // Adds a frame for bar to the stack
      // foo continues executing after bar completes (removed from call stack)
    }

    function bar() {
      console.log("Inside bar");
    }
    ```

    - After `bar` completes its execution, its frame is removed from the stack, and execution continues in `foo`.

- Example

    ```javascript
    main(); // Calls main, which then calls welcome and greet

    function main() {
    welcome();    // Frame for welcome is added and then starts executing.
    // AFter welcome() frame gets removed on its completion. Instruction Pointer will be back 
    // 
    greet("Alice");// Frame for greet is added on top
    }

    function welcome() {
    console.log("Welcome to the call stack example");
    }

    function greet(name) {
    console.log(`Hello, ${name}!`);
    }
    ```

- In this example, the call stack looks like:

    1. Frame for `main`
    1. Frame for `welcome`
    1. Frame for `greet`

As each function completes, its frame is popped off the stack. So, the order of removal is `greet`, `welcome`, and finally, `main`.

## WebAPI's and Single Threaded

- Why to learn Async JS? Answer is **JS is single threaded.**
  > At any point in time, JS (single thread) is running at most one line of JS code.
    >> This means that JavaScript code is executed line by line in a sequential manner, and at any given point, only one operation is being processed.
- However, the web browser environment in which JavaScript often runs is not single-threaded. **Browsers come with additional components** and features that are not part of the JavaScript language itself. One of these components is the Web APIs (Application Programming Interfaces).
  > Web APIs provide functionality that allows JavaScript to interact with the browser environment. These APIs include, but are not limited to:
  >
    >> - **DOM (Document Object Model):** Allows manipulation of HTML and XML documents.
    >> - **XMLHttpRequest (XHR):** Enables making HTTP requests.
    >> - **setTimeout, setInterval:** Functions for scheduling code to run asynchronously.
    >>
    >>> Browsers come with Web APIs that are able to handle certain tasks in the background (like making requests or setTimeout.)
    >>>
    >>>> The JS call stack recognizes these Web API functions and passes them off to browser to take care of.
    >>>>
    >>>>> Once the browser finishes those tasks, they **return from callback queue** and are **pushed onto the stack as a callback**.

    ```js
    console.log("Start");

    setTimeout(function () {
    console.log("Timeout complete");
    }, 2000);

    console.log("End");
    ```

   In this example, "Start" and "End" will be logged first because `setTimeout` is asynchronous and we've asked browser to do it for us (due to its async. nature). After approximately 2 seconds, "Timeout complete" will be logged when the callback function is moved from the callback queue to the call stack.

### Summary

- JavaScript itself is single-threaded, but the browser environment is not.

- Web APIs provide functionality to interact with the browser environment.
- Asynchronous operations in JavaScript use the callback queue and the event loop to handle tasks outside the main thread of execution.

## Callback Hell

- Callback hell refers to a situation where multiple nested callbacks are used in asynchronous JavaScript code.
  - This scenario can occur when dealing with complex and dependent asynchronous tasks, leading to code that is difficult to read, maintain, and reason about.
    - The deeper the nesting, the more challenging it becomes to understand the flow of the program.

  - Here's an example that might lead to a callback hell.

    ```js
    getUserData(userId, (userData) => {
      getProfileDetails(userData.profileId, (profileDetails) => {
        getPosts(userData.userId, (posts) => {
          updateUI(userData, profileDetails, posts, () => {
            // All operations completed, do something else
          });
        });
      });
    });
    ```

  - This structure, with callbacks within callbacks, can quickly become difficult to manage as more operations are added. The code's indentation increases, making it harder to understand the logical flow and dependencies.

## Promises

### Working with Promises

- Promises represent the eventual completion of an asynchronous operation, allowing us to handle success or failure gracefully. They offer a more structured approach, especially when dealing with branching paths based on the success or failure of an operation.
  - Making request or getting Data from API represents an asynchronous operation. Sometimes it would work sometime won't due to incorrect credentials, authorization denial, internet down or doesn't exist
  - In the past, callbacks were commonly used for asynchronous operations, leading to callback hell when handling multiple dependent actions. For instance:

    ```js
    const fakeCallBackRequest = (url, success, failure) => {
      const delay = Math.floor(Math.random() * 4500) + 500;
      setTimeout( () => {
        delay > 4000? failure('Connection Timeout :(') : success(`Here is your fake data from ${url}`);
      }, delay)
    }
    // fake request using callback
    fakeCallBackRequest('books.com', 
                        (str) => console.log(`It worked!!! ${str}`),
                        (str) => console.log(`ERROR!!! ${str}`)); 
    
    // fake request using callback hell (nested up to 4 dependent actions)                    
    fakeCallBackRequest('books.com/page1', 
      (response) => {
        console.log(`Req. (1) worked!!! ${response}`);
          fakeCallBackRequest('books.com/page2',
            (response) => {
              console.log(`Req. (2) worked!!! ${response}`);
              fakeCallBackRequest('books.com/page3',
                (response) => {
                  console.log(`Req. (3) worked!!! ${response}`);
                  fakeCallBackRequest('books.com/page4', 
                    (response) => {
                      console.log(`Req. (4) worked!!! ${response}`);
                    }, (error) => {
                      console.log(`Req. (4) failed!!! ${error}`);
                    }
                  )
                }, (error) => {
                  console.log(`Req. (3) failed!!! ${error}`)
                }
              )
            }, (error) =>{
              console.log(`Req. (2) failed!!! ${error}`);
            }
          )
      }, (error) => {
        console.log(`Req. (1) failed!!! ${error}`)
      }
    )
    ```

- Promises simplify this structure and introduce three states: `pending`, `resolved` and `rejected`.
- We can attach callbacks on these states, instead of passing callbacks into function.
  - `then()` for `resolution`
  - `catch()` for `rejection`

    ```js
    const fakeRequestPromise = (url) => {
      return new Promise( (resolve, reject) => {
          const delay = Math.floor(Math.random() * 4500) + 500;
          console.log(delay);
          setTimeout(() => {
              delay > 4000 ? reject(`connection timeout :(`) : resolve(`here is your data from ${url}`);
          }, delay);
      })
    }

    // nested promises leading to syntax and readability problems also :(
    fakeRequestPromise('books.com/api/1')
        .then((response) => {
            console.log(`Req. (1) worked!!! ${response}`);
            fakeRequestPromise('books.com/api/2')
                .then((response) => {
                    console.log(`Req. (2) worked!!! ${response}`);
                    fakeRequestPromise('books.com/api/3')
                        .then((response) => {
                            console.log(`Req. (3) worked!!! ${response}`);
                        })
                        .catch((error) => {
                            console.log(`Req. (3) failed!!! ${error}`);
                        });
                })
                .catch((error) => {
                    console.log(`Req. (2) failed!!! ${error}`);
                });
        })
        .catch((error) => {
            console.log(`Req. (1) failed!!! ${error}`)
        });
    ```

- Magic of Promises (Promise Chaining - dependent asynchronous actions) - where promises get better than callbacks
  - **We chain promises not nest them.** within the `.then()` of the prev. promise, we can return a new `promise` and call its `.then()` and so on. We may have only one `.catch()` to hold all the errors of async operations. If any one of the request failed, all the subsequent operations are ignored and `.catch()` is executed.

    ```js
    fakeRequestPromise('books.com/api/1')
      .then((response) => {
        console.log(`Req. (1) worked!!! ${response}`);
        return fakeRequestPromise('books.com/api/2');
      })
      .then((response) => {
        console.log(`Req. (2) worked!!! ${response}`);
        return fakeRequestPromise('books.com/api/3');
      })
      .then((response) => {
        console.log(`Req. (3) worked!!! ${response}`)
      })
      .catch((error) => {
        console.log(`A request failed!!! ${error}`)
      });
    ```

- The magic of promises lies in chaining them, where we return a new promise within the `then()` callback. This leads to a more organized and sequential flow, enhancing code readability and maintainability.

### Creating Promises

- Promise is like a container for a value which might be available now, or in the future, or never.
  - Here's a basic structure of creating a Promise:
  
    ```js
    const myPromise = new Promise((resolve, reject) => {
    // Asynchronous operation or task
    // ...

    // If the operation is successful, call resolve with the result
    // resolve(result);

    // If there is an error, call reject with an error message
    // reject(error);
    });
    ```
  
  - The **`Promise` constructor takes a single argument**, a function commonly referred to as the **executor.** The executor function takes two parameters: `resolve` and `reject`.

    - `resolve` is a function to be called when the asynchronous operation is successful, and it accepts a value (the result of the operation).

    - `reject` is a function to be called when there is an error during the asynchronous operation, and it accepts an error message or an error object.

    ```js
    const delayedColorChange = (nextColor, delay) => {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          document.body.style.backgroundColor = nextColor;
          console.log(`Color changed to ${nextColor}`);
          resolve();
        }, delay);
      });
    }
    
    delayedColorChange('red', 1000)
      .then(() => delayedColorChange('green', 1000))
      .then (() => delayedColorChange('blue', 1000))
      .then(() =>  delayedColorChange('yellow', 1000))
      .then(() => delayedColorChange('indigo', 1000))
      .then(() => delayedColorChange('violet', 1000))
      .then(() => delayedColorChange('orange', 1000))
    ```

## Async Functions

- Async functions in JavaScript offer a syntax improvement over `Promises`, making asynchronous code more readable and manageable. The combination of `async` and `await` simplifies the syntax and structure of code that involves asynchronous operations.

### Async Function Declaration

- An **async function** is declared using the `async` keyword. This keyword is placed before the function keyword in the function declaration.
  - An async function always returns a Promise, implicitly wrapping the value it returns in a resolved `Promise`.

  ```js
  async function myAsyncFunction() {
    // Async code here
    return "Hello from async function!";
  }

  const greet = async () => { // Promise { <state>: "fulfilled", <value>: "Hello from async function" }
    return "Hello from async function";
  }
  ```

  Code Demonstration
  
  ```js
  function delayedColorChange(color, delay) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            document.body.style.backgroundColor = color;
            console.log(`color changed to ${color}`);
            resolve();
        }, delay);
    })
  }
  const rainbow = async () => {
      await delayedColorChange('yellow', 3000);
      await delayedColorChange("orange", 2000);
      await delayedColorChange('teal', 1000)
  }

  rainbow()
  ```

- In the `rainbow()`, the await keyword ensures that each call to delayedColorChange is completed before moving on to the next one. This sequential execution simplifies the code, making it more readable.

### Await Keyword

- The `await` keyword can only be used inside an async function. It is used to pause the execution of the async function until the `Promise` is resolved. This **makes asynchronous code look and behave more like synchronous code**.

#### Handling Errors in Async Functions

- The `try` block contains the code where you expect the asynchronous operation to succeed, and the `catch` block handles any errors that may occur.
  - Any errors that occur during the execution of asynchronous operations are caught and can be handled appropriately.

  ```js
  const fetchData = async () => {
  try {
    const result = await someAsyncOperation();
    console.log(result);
  } catch (error) {
    console.error('An error occurred:', error);
    }
  };
  ```
