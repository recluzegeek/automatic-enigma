
# Asynchronous Javascript

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
    >>>>> Once the browser finishes those tasks, they return and are pushed onto the stack as a callback.

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
