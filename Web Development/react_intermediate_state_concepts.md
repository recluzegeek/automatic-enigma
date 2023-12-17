# Intermediate State Concepts

When managing state in a React component, it's crucial to handle updates correctly, especially when the new state depends on the previous state. This documentation explores the concept of using updater functions to ensure accurate state updates in such scenarios.

## Setting State with Updater Function

- Consider a basic counter component that increments a count when a button is clicked:

    ```jsx
    // Counter.jsx

    import { useState } from 'react';

    export default function Counter() {
        const [count, setCount] = useState(0);

        const incrementCount = () => {
            useState(count + 1); // Incorrect way, refer to this line
        };

        return (
            <div>
                <p>Count is: {count}</p>
                <button onClick={incrementCount}>+1</button>
            </div>
        );
    }
    ```

### Issue with Direct State Modification

The initial way seems to work, but it has a little problem. When you change the state directly inside an event, like when a button is clicked (`onClick`), React might not show the updated value right away. To avoid this issue, it's better to use the _**updater function inside the state setter**_ when the new state depends on the previous one.

**Explanation:**

- Let's make a small adjustment to the code above. Here, we've set up two buttons to increase the count by 1 and 3, respectively.
- If we use the same method for the setter function as before, both buttons will show the same output, even though we've assigned different `onClick` functions for each.
  - The issue is that the `count` value doesn't get immediately updated inside the setter function. It only updates when the component is re-rendered after the `useState()` is called.
    - So, the values stay the same for each `useState()` call until the component is re-rendered.

    ```jsx
    import React, { useState } from 'react';

    export default function Counter() {
    const [count, setCount] = useState(0);

    const incrementCountByOne = () => {
        // Incorrect usage: Directly modifying the count without updating the state
        useState(count + 1);
    };

    const incrementCountByThree = () => {
        // Incorrect usage: Directly modifying the count without updating the state
        useState(count + 1);
        console.log({ count }); // Prints 0 and not the recently updated value
        // Incorrect usage: Directly modifying the count without updating the state
        useState(count + 1);
        console.log({ count }); // Prints 0 and not the recently updated value
        // Incorrect usage: Directly modifying the count without updating the state
        useState(count + 1);
        // Prints 0 as the component has not been re-rendered, and the recent value is not populated in count, which still has 0 in it.
        console.log({ count });
    };

    return (
        <>
        <p>Count is: {count}</p>
        <button onClick={incrementCountByOne}>+1</button>
        <button onClick={incrementCountByThree}>+3</button>
        </>
    );
    }
    ```

### Corrected Approach Using Updater Function

- To address this issue, **use an updater function inside of the state setter function**, ensuring that state updates are based on the previous state:

    ```jsx
    // Counter.jsx

    import { useState } from 'react';

    export default function Counter() {
        const [count, setCount] = useState(0);

        const incrementCountByOne = () => setCount((prevCount) => prevCount + 1);

        const incrementCountByThree = () => {
            setCount((prevCount) => prevCount + 1);
            console.log(count); // It may not log the updated count immediately
            setCount((prevCount) => prevCount + 1);
            setCount((prevCount) => prevCount + 1);
        };

        return (
            <div>
                <p>Count is: {count}</p>
                <button onClick={incrementCountByOne}>+1</button>
                <button onClick={incrementCountByThree}>+3</button>
            </div>
        );
    }
    ```

### Key Points

- Always use the functional update form when modifying state based on the previous state.
- React batches state updates for performance, ensuring they are applied together.
  - In React, when you change the state, React is smart. **Instead of immediately updating the component each time, it waits a bit. If you make several state changes in a short time, React groups them together and updates the component only once.**
- If additional actions are required after state updates, consider using the useEffect hook.

Certainly! Let's discuss state initializer functions and highlight the difference between `exampleInitializer()` and `exampleInitializer` in React JSX.

## State Initializer Functions

- [Avoid Recreating the Initial State](https://react.dev/reference/react/useState#avoiding-recreating-the-initial-state) from the official [Documentation](react.dev)

    > - If you pass a function as `initialState`, it will be treated as an initializer function. It should be pure, should take no arguments, and should return a value of any type. React will call your initializer function when initializing the component, and store its return value as the initial state. See an example below.
    >
    >> ### Avoiding recreating the initial state
    >>
    >> React saves the initial state once and ignores it on the next renders.
    >>
    >>```jsx 
    >>function TodoList() {
    >>  const [todos, setTodos] = useState(createInitialTodos());
    >>  // ...
    >>}
    >>```
    >>>
    >>> Although the result of `createInitialTodos()` is only used for the initial render, you’re still calling this function on every render. This can be wasteful if it’s creating large arrays or performing expensive calculations.
    >>> To solve this, you may pass it as an initializer function to useState instead:
    >>>
    >>>```jsx
    >>>function TodoList() {
    >>>  const [todos, setTodos] = useState(createInitialTodos);
    >>>  // ...
    >>>}
    >>>```

- Notice that you’re passing `createInitialTodos`, which is the function itself, and not `createInitialTodos()`, which is the result of calling it. If you pass a function to `useState`, React will only call it during initialization.

## When Does React Re-Render?

In React, **rendering occurs when the state or props of a component change**. It doesn't necessarily happen immediately; React optimizes rendering for performance.

```jsx
import React, { useState } from "react";

export default function Counter() {
  console.log("Rendered...");
  const [counter, setCounter] = useState(0);
  return (
    <div>
      <p>Count is: {counter}</p>
      <button onClick={() => setCounter((c) => ++c)}>+1</button>
      <button
        onClick={() => {
          setCounter((c) => c + 1);
          setCounter((d) => d + 1);
          setCounter((c) => c + 1);
          setCounter((c) => c + 1);
        }}
      >
        +4
      </button>
      <button onClick={() => setCounter((c) => 10)}>Set to 10</button>
    </div>
  );
}

```

- Considering the above example, **rendering will occur** when:

    1. The **component initially mounts**.
    2. The **state of the `counter` changes using `setCounter`**.

- Let's break down the example:
  - The initial render occurs when the component mounts.

    ```jsx
    const [counter, setCounter] = useState(0);
    ```

  - Clicking the "+1" button triggers a state update using the functional form of `setCounter`, which correctly increments the counter and triggers a re-render.

    ```html
    <button onClick={() => setCounter((c) => ++c)}>Add +1</button>
    ```

  - Clicking the "+4" button triggers a series of state updates. However, due to React's batching mechanism, these updates will be batched together, and the component will only render once. The final count will be the result of all these updates.

    ```html
    <button
    onClick={() => {
        setCounter((c) => c + 1);
        setCounter((d) => d + 1);
        setCounter((c) => c + 1);
        setCounter((c) => c + 1);
    }}
    >
    Add +4
    </button>
    ```

  - Clicking the "Set to 10" button triggers a state update, causing a re-render with the counter set to 10.

    ```html
    <button onClick={() => setCounter((c) => 10)}>Set to 10</button>
    ```

>**Note:**
>> React employs a mechanism called **"reconciliation"** to optimize rendering. When you use the `setState` function to update the state of a component, React compares the new state with the current state. If they are the same, React may decide to skip the rendering process to avoid unnecessary updates to the DOM.
>>> React's reconciliation process ensures that unnecessary renders are avoided whenever possible.
>>>
>>> - When the "+4" button is clicked, you have multiple `setCounter` calls. React batches these state updates together for efficiency. Only one render occurs with the final state calculated after all updates.
>>> - If the current state is already 10 and you click the `Set to 10` button, the state doesn't change. React recognizes that the new state (10) is the same as the current state. In such cases, React may choose to skip rendering to optimize performance.
