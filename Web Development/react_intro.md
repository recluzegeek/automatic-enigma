# React

- React is a frontend library that help us in building user interface from components.
- React is more of a ecosystem than a just individual library.
  - React doesn't work on server side.
  - React doesn't connect with database.
  - Mostly used for single page applications (SPA).
- We can assemble smaller components to build large applications.

## Components

- Components combine `HTML` and `logic` into a single reusable function.
- Functions that knows **how to render themselves into HTML**.
  
    ```js
    function Header(){
        return <h1>I'm a header component!</h1>;
    }
    ```

- React is all about reusing component and not just build those components.
  - A component can be as small as a Like button or an html form.
  - A single video component in react may comprised of a video thumbnail, video title, video description and a nice little like button. All this is packed up in a single video component, that we can then use in our application &mdash; reusing the video component in a **Video List Component**.
  - We build complex frontend from the small and modular components by using `HTML`, `CSS` and `JS` logic and can reuse them also.

## Setting up CodeSandBox &mdash; Easiest Way

- Its the easiest way to get up and running react.
- `react.dev` &mdash; official react documentation also uses `codesandbox.io`. Use `react.new` to make new react project in `codesandbox`

### React Sandbox File Structure

- `public/` &mdash; contains `index.html` which is the main entry point to our react application. All of our content will be rendered in the **div with id root**.
  - Our react application will be inserted into this empty div with id root and we can change this by modifying the configuration.
- `src/` &mdash; This is where we'll be writing our react code, individual components will go in this directory.

## Basic React App Structure

- Basic Conventions that most react application follows:
  - **`App component`** &mdash; Highest level component of the react application. All of the individual components ranging from hundreds to thousands (typically in a large application) have to be rendered inside a component which is typically **`App Component`**
  - **`index.js`** &mdash; This is the file where our **`App Component`** gets wrapped into the html document. We usually don't change it either. Look at this particular line in the file,

    ```js
    const rootElement = document.getElementById('root')
    ```

    Now, if you go to the `public/index.html` file, there's a `div#root` and that's where the wrapped html is inserted into. 
    - Its like component is a function and a function has to be called somewhere, and that's where it's rendered, it's same as if its been called there.

    ```js
    const root = createElement(rootElement);
    root.render(
        <StrictMode>
            <App />
        </StrictMode>    
    );
    ```

## Basics of JSX

- JSX &mdash; is a syntax extension for Javascript.
  - Allows us to **write markup** that looks like **HTML directly inside of our HTML/JS**!
- It's not a legal JS on it's own, so it must be **transpiled** into real JS.
  - Go to **`babeljs.io`** to look how the JSX is transpiled into real JS. babel does the heavy lifting for us and converts _JSX_ into _REAL JS_ that's rendered by the **React Virtual DOM**.

## First React Component

- Writing a component is same as of writing a function. It has a name, returns something
  - CamelCase notation should be followed in the react components.
  - _**Defining a component is not the same as rendering it, the same way as defining function is not the same as executing it.**_
  - Syntax for rendering a component looks like an html element. For example, calling `Greeter` component in the react application, we'll do something like `<Greeter />` &mdash; a self closing html tag syntax, but now its JSX syntax for calling a component. We don't use `Greeter()` &mdash; a regular way of calling functions in JS instead we do `<Greeter />`.

    ```js
    import React from 'react';

    // first react component
    function Greeter(){ 
        return <h3>Hello World!</h3>
    }

    export function App(props) {
        return (
            <div className='App'>
            <!--// calling the Greeter react component. -->
                <Greet />
                <Greet />
                <Greet />
                <Greet />
                <h2>Start editing to see some magic happen!</h2>
                <h1>Hello React.</h1>
            </div>
        );
    }

    // Log to console
    console.log('Hello console')
    ```
