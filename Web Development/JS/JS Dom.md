---
title: js_dom
updated: 2023-10-13 21:41:54Z
---

## DOM

- Stands for Document Object Model.
The Document Object Model (DOM) is the JavaScript representation of a webpage, consisting of objects that we can interact with through JavaScript.

### Document Object

- The Document Object is the root of the DOM, serving as an entry point. It contains the representation of all content on a page along with useful methods and properties.

#### Selecting Elements

- **`getElementById()`**
- **`getElementsByTagName()`**
  - Returns an HTML Collection, which is iterable and not an array.
- **`getElementsByClassName()`**

##### Query Selector & Query Selector All

- **`querySelector()`** -- An all-in-one method to select a single element.
- **`querySelectorAll()`** -- Similar idea but returns a collection of matching elements.
  - We can input selectors using CSS selector syntax.

    ```js
    // Finds the first h1 element:
    document.querySelector('h1');

    // Finds the first element with the ID of red
    document.querySelector('#red');

    // Finds the first element with the class of big
    document.querySelector('.big')

    // Finds all anchor tags inside paragraphs (descendant selector)
    document.querySelectorAll('p a');
    ```

#### Manipulating DOM

- `document.querySelector('p').innerText`: Edit or read text from the document.
- `document.querySelector('p').textContent`: Edit or read text from the HTML file.
- `document.querySelector('p').innerHTML`: Add HTML elements inside the document.


| Property/Method                 | Description                                                           | Usage and Differences                                                |
|----------------------------------|-----------------------------------------------------------------------|----------------------------------------------------------------------|
| `h1.style.fontSize`              | Accesses the inline style of an `<h1>` element.                       | Does not return anything, as JavaScript cares about inline styling. |
| `window.getComputedStyle(element)`| Retrieves the computed style of an element.                            | Useful for getting the actual computed style, including inherited styles. |
| `classList.add(className)`        | Adds a class to the list of classes for an element.                   | Useful for dynamically adding styles or behavior to an element.      |
| `classList.remove(className)`     | Removes a class from the list of classes for an element.              | Allows for the dynamic removal of styles or behavior.                |
| `classList.toggle(className)`     | Toggles the presence of a class in the list of classes for an element.| Adds the class if it's not present, removes it if it is.              |
| `parentElement`                  | Returns the parent element of the selected element.                   | Useful for navigation and manipulation within the DOM hierarchy.    |
| `childElementCount`              | Returns the number of child elements of the selected element.         | Provides a count of the immediate child elements.                   |
| `children`                       | Returns a live HTMLCollection of child elements.                      | Allows iteration through the immediate child elements.              |
| `createElement(tagName)`         | Creates a new HTML element with the specified tag name.              | Useful for dynamically generating HTML elements in JavaScript.      |
| `append()`                       | Appends one or more nodes or DOMString to the end of the element.    | Adds content as the last child of the element.                      |
| `prepend()`                      | Inserts one or more nodes or DOMString before the first child of the element.| Adds content as the first child of the element.               |
| `insertAdjacentElement(position, element)` | Inserts an element into a specified position relative to the current element.| Allows for precise control over element insertion.           |
| `remove()`                       | Removes the element from the DOM.                                    | Permanently removes the element from the document.                 |
