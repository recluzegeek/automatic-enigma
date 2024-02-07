
# CSS Selectors Table

| Selector                      | Description                                                   |
|-------------------------------|---------------------------------------------------------------|
| `element`                     | Selects all instances of the specified HTML element.           |
| `#id`                         | Selects a specific element with the given ID attribute.        |
| `.class`                      | Selects all elements with the specified class attribute.       |
| `element, element`            | Selects multiple elements.                                     |
| `element.class`               | Selects elements that have the specified class within an element. |
| `element > child`              | Selects a direct child of the specified element.               |
| `element + sibling`            | Selects an element that is the adjacent sibling of another element. |
| `:hover`                      | Selects an element when the user hovers over it.               |
| `:nth-child(n)`               | Selects the nth child of its parent.                           |
| `:not(selector)`              | Selects all elements that do not match the given selector.     |
| `selector1, selector2`         | Groups multiple selectors together.                            |
| `h1`                          | Selects all `<h1>` elements.                                   |
| `.info`                       | Selects all elements with the class "info".                   |
| `p + p`                       | Selects a `<p>` element that is the adjacent sibling of another `<p>` element. |
| `a[href^="https"]`            | Selects all `<a>` elements with an "href" attribute starting with "https". |
| `ul > li`                     | Selects all `<li>` elements that are direct children of a `<ul>` element. |
| `input[type="text"]`          | Selects all `<input>` elements with a "type" attribute set to "text". |
| `:nth-child(odd)`             | Selects all odd-numbered children of an element.              |
| `div:not(.special)`           | Selects all `<div>` elements that do not have the class "special". |
| `button:disabled`              | Selects all disabled `<button>` elements.                     |
| `body`                        | Selects the `<body>` element.                                 |
| `h2 span`                     | Selects all `<span>` elements that are descendants of an `<h2>` element. |
| `.container p`                | Selects all `<p>` elements that are descendants of an element with the class "container". |
| `article > p`                 | Selects all `<p>` elements that are direct children of an `<article>` element. |
| `section .highlight`          | Selects all elements with the class "highlight" that are descendants of a `<section>` element. |
| `body div span`               | Selects all `<span>` elements that are descendants of a `<div>` element, which is a descendant of the `<body>` element. |
| `form input[type="submit"]`   | Selects all `<input>` elements with a "type" attribute set to "submit" that are descendants of a `<form>` element. |

**Example:**

```css
/* CSS example 1 */
body {
  font-family: 'Arial', sans-serif;
}

.header {
  background-color: #333;
  color: #fff;
}

a:hover {
  text-decoration: underline;
}

.container > div {
  margin: 10px;
}

.list-item + .list-item {
  margin-top: 5px;
}

input:not([type="submit"]) {
  border: 1px solid #ccc;
}

/* CSS example 2 */
h1 {
  color: #0066cc;
}

.info {
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  padding: 10px;
}

p + p {
  margin-top: 1em;
}

a[href^="https"] {
  color: #008000;
}

ul > li {
  list-style-type: square;
}

input[type="text"] {
  width: 200px;
}

.special {
  font-weight: bold;
}

button:disabled {
  opacity: 0.6;
}

/* CSS example 3 */
body {
  font-family: 'Arial', sans-serif;
}

h2 span {
  color: #ff6600;
}

.container p {
  margin: 10px;
}

ul li {
  list-style-type: circle;
}

article > p {
  font-style: italic;
}

section .highlight {
  background-color: #ffffcc;
}

body div span {
  font-weight: bold;
}

form input[type="submit"] {
  background-color: #4caf50;
  color: #fff;
}
```