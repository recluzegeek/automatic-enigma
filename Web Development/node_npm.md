---
title: node_npm
updated: 2023-10-16 21:30:32Z
---

## Understanding `module.exports`

- In Node.js, the `module.exports` feature allows you to share JavaScript code between different files, making code modular and reusable.
  
### 1. Exporting a Module

- **Why?** To share specific functions or variables from a file.
- **How?** Define your functions or variables and use `module.exports` to make them accessible.
  
  ```js
  // math.js
  const add = (x, y) => x + y;

  const PI = 3.14159;

  const square = x => x * x;

    // Exporting Module
        // 1st way - a kind of clunky
            // module.exports.add = add;
            // module.exports.PI = PI;
            // module.exports.square = square;

        // 2nd way - make an object of the properties and functions that're to be exported
            //  const math = {
            //      add: add,
            //      PI: PI,
            //      square: square
            //  };
            //  module.exports = math; 

        // 3rd way - add the module.exports statement in front of the function definition
        // or properties / variable to export directly
            module.exports.add = (x, y) => x + y;

            module.exports.PI = 3.14159;

            module.exports.square = x => x * x;
  ```

### 2. Importing and Using the Module

- **Why?** Use the exported functionalities in another file.
- **How?** Use `require` to bring in the exported module.
  
  ```js
  // app.js
  // `./` dot slash is important as it tells node
  // to use our module and don't look inside node_modules folder
  const math = require('./math');

  console.log(math.PI);     // prints 3.14159

  console.log(math.add(12, 3));     // prints 15

  // destructuring the object for a cleaner code

  const {PI, square} = require('./math');

  console.log(PI);      // prints 3.14159

  console.log(square(9));       // prints 81
  ```

***Note:*** Always remember that `module.exports` should be an object.

### 3. Require an Entire Directory

- To require an entire directory, use an `index.js` file in that directory to consolidate exports from other files.
  - Let's say we've a directory named `cats` and it has 3 files inside it namely, `shadie.js`, `janet.js` and `blue.js`. Each one of them are just exporting an object about their information.
  
  ```js
  // index.js
  const blue = require('./blue');
  const shadie = require('./shadie');
  const janet = require('./janet');

  const allCats = [blue, shadie, janet];

  module.exports = allCats;
  ```

  - When we'll require the `cats` directory, node will look for the `index.js` file. `index.js` is kind of entry point to the directory, and whatever the `index file` exports is the what directory will export.

## npm - Node Package Manager

- **What is npm?** It's a library of thousands of free packages created by developers.
- **Why use npm?** Easily install and manage packages for your Node.js projects.

### installing packages using npm

- Use `npm install {packageName}` to add a package to your project.

#### node_modules folder & package_lock.json

- Never share `node_modules` with others; it's project-specific.
- `package-lock.json` helps ensure consistent installations across different machines.

### Managing Global Packages

- **Why use global packages?** For command-line tools you want to use directly.
- **How?** Use `npm install -g {packageName}` or `npm i -g {packageName}`.
- linking global package to the project

### package.json

- **What is it?** A special file in Node.js projects containing metadata, especially dependencies.
- **How to create?** Use npm init to generate a package.json file.
- **Adding dependencies:** Dependencies are automatically added to package.json when installing packages.

### Installing Project Dependencies

- Use `npm install` to install all project dependencies based on `package.json`.

## Node.js Package Management Cheat Sheet

## Node.js Package Management Cheat Sheet

    <h2>Node.js Package Management Cheat Sheet</h2>

    <table>
        <tr>
            <th>Action</th>
            <th>Command</th>
            <th>Description</th>
        </tr>
        <tr>
            <td colspan="3" align="center"><strong>Installing Local Packages</strong></td>
        </tr>
        <tr>
            <td>npm install packageName</td>
            <td></td>
            <td>Install a package locally in your project.</td>
        </tr>
        <tr>
            <td>npm install packageName --save</td>
            <td></td>
            <td>Install and save the package as a dependency in your <code>package.json</code> file.</td>
        </tr>
        <tr>
            <td>npm install packageName --save-dev</td>
            <td></td>
            <td>Install and save the package as a development dependency.</td>
        </tr>
        <tr>
            <td colspan="3" align="center"><strong>Removing Local Packages</strong></td>
        </tr>
        <tr>
            <td>npm uninstall packageName</td>
            <td></td>
            <td>Remove a package from your project.</td>
        </tr>
        <tr>
            <td>npm uninstall packageName --save</td>
            <td></td>
            <td>Remove a package and update the <code>package.json</code> file.</td>
        </tr>
        <tr>
            <td>npm uninstall packageName --save-dev</td>
            <td></td>
            <td>Remove a development dependency and update <code>package.json</code>.</td>
        </tr>
        <tr>
            <td>npm prune</td>
            <td></td>
            <td>Remove unused packages from <code>node_modules</code> that are not listed as dependencies.</td>
        </tr>
        <tr>
            <td colspan="3" align="center"><strong>Installing Global Packages</strong></td>
        </tr>
        <tr>
            <td>npm install -g packageName</td>
            <td></td>
            <td>Install a package globally for command-line tools.</td>
        </tr>
        <tr>
            <td colspan="3" align="center"><strong>Linking Global Package to Project</strong></td>
        </tr>
        <tr>
            <td>npm link packageName</td>
            <td></td>
            <td>Link a globally installed package to your project.</td>
        </tr>
        <tr>
            <td colspan="3" align="center"><strong>Removing Global Packages</strong></td>
        </tr>
        <tr>
            <td>npm uninstall -g packageName</td>
            <td></td>
            <td>Remove a globally installed package.</td>
        </tr>
        <tr>
            <td colspan="3" align="center"><strong>Unlinking Global Package from Project</strong></td>
        </tr>
        <tr>
            <td>npm unlink packageName</td>
            <td></td>
            <td>Remove the symbolic link between your project and the global package.</td>
        </tr>
    </table>

    <p>Remember to replace <code>packageName</code> with the actual name of the package you are working with. Exercise caution, especially with global package removal, to avoid unintended consequences.</p>
