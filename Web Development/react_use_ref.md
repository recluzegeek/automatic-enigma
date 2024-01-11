# useRef

`useRef` is a React hook that provides a way to create mutable object references that persist across renders.

- `Ref` is a **Box(object)**  with a mutable `.current` property that is persisted across renders ('normal' variables are always reset on re-renders)

    ```jsx
    const myRef = useRef(2024);
    console.log(myRef)
    console.log(myRef.current) // 2024
    ```

- We can write to and read from the ref using `.current`
- Unlike state, changes to a `ref` don't trigger a re-render of the component.
- **Use Cases:**
  - Creating a variables that stays the same between the renders (e.g. previous state, if of `setTimeout` etc...)
  - Selecting and Storing DOM Elements
- It's particularly useful for accessing and interacting with the DOM directly.
- `Refs` are for data that is not rendered: usually only appears in event handlers or effects not in JSX (otherwise use state)
  - Do not read or write `.current` in render logic (like state)
    - We perform these transitions usually in `useEffect`

## Refs to Select DOM Elements

This example uses`useRef` to focus on a search bar on page load and when a specific key, like '/' is pressed:

```jsx
import React, { useEffect, useRef, useState } from "react";

export default function Comp2() {
  const searchInputRef = useRef(null);
  const [name, setName] = useState("");

  useEffect(() => {
    const handleKeyPress = (event) => {
      if (document.activeElement === searchInputRef.current) return;

      if (event.key === "/" || event.key === "Enter") {
        // Focus the search bar when '/' or 'Enter' is pressed
        searchInputRef.current.focus();
        setName("");
      }
    };

    document.addEventListener("keydown", handleKeyPress);

    return () => {
      document.removeEventListener("keydown", handleKeyPress);
    };
  }, []);

  return (
    <div>
      <input
        type="text"
        placeholder="Search..."
        id="search"
        onChange={(e) => setName(e.target.value)}
        ref={searchInputRef}
      />
      {name && <p>Searching for {name}</p>}
    </div>
  );
}
```

In this example:

- We create a `searchInputRef` using `useRef` and attach it to the input element using the `ref` attribute.
- In the `useEffect` hook, we use `searchInputRef.current.focus()` to focus on the search bar when the component mounts (`[]` as the dependency array ensures it runs only once).
- We define a `handleKeyPress` function that checks if the pressed key is '/', and if so, it focuses on the search bar using `searchInputRef.current.focus()`.
