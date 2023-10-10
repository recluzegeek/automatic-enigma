
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
