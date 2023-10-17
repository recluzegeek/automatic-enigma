---
title: node
updated: 2023-10-16 20:03:59Z
---

## Understanding Node.js

Node.js is a JavaScript runtime that operates outside the web browser. It allows us to execute JavaScript code on servers, enabling functionalities not traditionally associated with browser-based scripting.

## The Basics

- **JavaScript Runtime:**
  - Node.js serves as a runtime for JavaScript, providing an environment for executing code outside the browser.

  - **REPL (Read, Evaluate, Print Loop):**
    - Node.js features a REPL environment for interactive code execution.

    - Unlike the browser, Node.js doesn't have access to the Document Object Model (DOM) or browser-specific APIs. However, it provides access to modules that facilitate tasks like working with the file system and interacting with the operating system.

    - The global object in Node.js serves as an alternative to the `window` object in browser-based JavaScript.

## Command Line Operations

- **Processes and `process.argv`:**
  - Node.js allows us to handle command line arguments through the `process.argv` array.

  - To extract user-provided arguments, we can use the `slice()` method starting from index 2 (as the first two elements are the Node executable path and the file name).

  - Here's a simple example:

    ```javascript
    // Extracting command line arguments
    const args = process.argv.slice(2);

    // Performing operations on the arguments
    for (const arg of args) {
      // Your code here
      console.log(arg);
    }
    ```

## Filesystem Operations

- **Working with Filesystem:**
  - Node.js allows us to perform operations on the file system, providing powerful capabilities for reading, writing, and manipulating files.

  - Example use cases include reading from a file, writing to a file, creating directories, and more.

  - Explore the Node.js documentation for the `fs` module to learn more about filesystem operations.
