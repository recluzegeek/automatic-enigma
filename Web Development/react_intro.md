# React

- React serves as a frontend library facilitating the construction of user interfaces through components.
- It forms an ecosystem rather than existing as a standalone library, with characteristics such as:
  - Absence of server-side functionality.
  - No direct integration with databases.
  - Predominantly utilized for single-page applications (SPAs).
- Large applications are created by assembling smaller components.

## Components

- Components merge HTML and JS logic into a singular, reusable function.
- Functions that knows **how to render themselves into HTML**.
  
    ```js
    function Header(){
        return <h1>I'm a header component!</h1>;
    }
    ```

- React emphasizes the reuse of components, ranging from small elements like buttons to more complex structures like video list components.
  - A single video component in react may comprised of a video thumbnail, video title, video description and a nice little like button. All this is packed up in a single video component, that we can then use in our application &mdash; reusing the video component in a **Video List Component**.
  
## Setting up CodeSandBox &mdash; Streamlined Approach

- CodeSandbox is the easiest way to initiate a React project.
- Official React documentation at `react.dev` also leverages `codesandbox.io`. Utilize `react.new` to create a new React project.

### React Sandbox File Structure

- `public/` &mdash; Houses `index.html`, the primary entry point for the React application. Content is rendered within the div with the ID "root."
- `src/` &mdash; This directory is dedicated to writing React code, where individual components reside.

## Basic React App Structure

- Conventions in a typical React application:
  - **`App component`** &mdash; The highest-level component that encapsulates all other components. All of the individual components ranging from hundreds to thousands (typically in a large application) are rendered inside this component.
  - **`index.js`** &mdash; This is the file where our **`App Component`** gets wrapped into the html document. We usually don't change it either. Look at this particular line in the file,

    ```js
    const rootElement = document.getElementById('root')
    ```

    - If you check out the `public/index.html` file, you'll see a `div#root` where all the wrapped HTML is placed. Think of it like a component being a function, and just as a function needs to be called somewhere, this is where it's getting rendered—similar to making a function call.

    ```js
    const root = createElement(rootElement);
    root.render(
        <StrictMode>
            <App />
        </StrictMode>    
    );
    ```

## JSX Basics

- JSX serves as a syntax extension for JavaScript, allowing the direct inclusion of HTML-like markup inside JavaScript.
- It's not a legal JS on it's own, so it must be **transpiled** into real JS.
- It must be transpiled into valid JavaScript, a task often performed by Babel. Visit **`babeljs.io`** to observe JSX transpilation.

## First React Component

- Writing a component is akin(similar) to creating a function, adhering to CamelCase notation.
  - _**Defining a component is not the same as rendering it, the same way as defining function is not the same as executing it.**_
  - The process of rendering a component follows a syntax resembling an HTML element. For instance, when calling the `Greeter` component in a React application, we use something like `<Greeter />`—this adopts a self-closing HTML tag syntax, but it's actually JSX syntax for invoking a component. Instead of the conventional `Greeter()` method for calling functions in JavaScript, we opt for the JSX style with `<Greeter />`.

    ```js
    import React from 'react';

    // First React component
    function Greeter() { 
        return <h3>Hello World!</h3>;
    }

    export function App(props) {
        return (
            <div className='App'>
                <!-- Invoking the Greeter React component. -->
                <Greeter />
                <Greeter />
                <Greeter />
                <Greeter />
                <h2>Start editing to see some magic happen!</h2>
                <h1>Hello React.</h1>
            </div>
        );
    }

    // Console log
    console.log('Hello console');
    ```
