# Intermediate State Concepts

When managing state in a React component, it's crucial to handle updates correctly, especially when the new state depends on the previous state. This documentation explores the concept of using updater functions to ensure accurate state updates in such scenarios and some other crucial concepts.

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

The initial way seems to work, but it has a little problem. When you change the state directly inside an event, like when a button is clicked (`onClick`), React might not show the updated value right away. To avoid this issue, it's better to use the _**updater function inside the state setter function**_ when the new state depends on the previous one.

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
            // Prints 0 as the component has not been re-rendered,
            // and the recent value is not populated in count, 
            // which still has 0 in it.
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

### Correct Approach Using Updater Function

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

## State Initializer Functions

- [Avoid Recreating the Initial State](https://react.dev/reference/react/useState#avoiding-recreating-the-initial-state) from the official [Documentation](https://react.dev)

    > If you pass a function as `initialState`, it will be treated as an initializer function. It should be pure, should take no arguments, and should return a value of any type. React will call your initializer function when initializing the component, and store its return value as the initial state. See an example below.
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
        Add +4
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
>>> - When the `Add +4` button is clicked, you have multiple `setCounter` calls. React batches these state updates together for efficiency. Only one render occurs with the final state calculated after all updates.
>>> - If the current state is already 10 and you click the `Set to 10` button, the state doesn't change. React recognizes that the new state (10) is the same as the current state. In such cases, React may choose to skip rendering to optimize performance.

## Understanding State and Objects in React

- [Updating Objects in State](https://react.dev/learn/updating-objects-in-state) from [Official React Documentation](https://react.dev)
    >
    > State can hold any kind of JavaScript value, including objects. But you shouldn’t change objects that you hold in the React state directly. Instead, when you want to update an object, you need to create a new one (or make a copy of an existing one), and then set the state to use that copy.

### Treat State as Read-Only

- In other words, any JavaScript object you place into state should be treated as read-only.

### Incorrect Code

```jsx
import { useState } from "react";

export default function ScoreKeeper() {
  const [score, setScore] = useState({ p1Score: 0, p2Score: 0 });

  function incrementPlayerOne() {
    score.p1Score += 1; // Incorrect: Modifying the object directly
    console.log(score);
    setScore(score); // Incorrect: Setting state with the same object
  }

  return (
    <>
      <p>Player 1 Score: {score.p1Score}</p>
      <p>Player 2 Score: {score.p2Score} </p>
      <button onClick={incrementPlayerOne}>+1 Player 1</button>
      <button>+1 Player 2</button>
    </>
  );
}
```

1. **Direct Object Modification:**
   - `score.p1Score += 1;` directly modifies the `score` object, which doesn't create a new reference, potentially causing React not to detect the change.

2. **Setting State with the Same Object:**
   - `setScore(score);` sets the state with the same object, potentially causing React not to recognize the change.
     - **React doesn't do unnecessary renders; it only performs a render when there's a change, and in the case of objects, changes occur when the original memory reference changes**.

### Corrected Code

- To trigger a re-render, create a new object and pass it to the state-setting function:

    ```jsx
    import { useState } from "react";

    export default function ScoreKeeper() {
    const [score, setScore] = useState({ p1Score: 0, p2Score: 0 });

    function incrementPlayerOne() {
        // Corrected: Creating a new object with the updated score
        setScore((prevScore) => ({ ...prevScore, p1Score: prevScore.p1Score + 1 }));
    }

    function incrementPlayerTwo() {
        setScore((prevScore) => ({ ...prevScore, p2Score: prevScore.p2Score + 1}));
    }

    return (
        <>
        <p>Player 1 Score: {score.p1Score}</p>
        <p>Player 2 Score: {score.p2Score} </p>
        <button onClick={incrementPlayerOne}>+1 Player 1</button>
        <button onClick={incrementPlayerTwo}>+1 Player 2</button>
        </>
    );
    }
    ```

#### Explanation of Corrections

1. **Using the Spread Operator:**
   - `setScore((prevScore) => ({ ...prevScore, p1Score: prevScore.p1Score + 1 }));` creates a new object with the updated score, ensuring immutability and a new reference.

#### Why It Doesn't Work as Expected

React compares object references to detect changes. Directly modifying an object or using the same object reference in state updates can lead to React not recognizing the changes.

### Best Practices

- Treat objects as immutable, creating new objects with changes.
- Use the spread operator (`{...prevScore}`) for creating copies of objects while making modifications.

## Understanding Arrays and State in React

In React, arrays can be efficiently managed in state to handle dynamic data and facilitate interactive components. Let's break down the usage of arrays in state using the `map` function and the `useState` hook, as demonstrated in the `EmojiList` component.

```jsx
// EmojiList --- Component Definition
import { useState } from "react";

// Function to randomly select an emoji from the array
function emojiHelper(arr) {
  return arr[Math.floor(Math.random() * arr.length)];
}

// Function to instantiate the initial state with a single emoji in an array
function instantiate() {
  console.log('instantiating...')
  return ['😊'];    /// this is important for emojis to be an array 
}

// EmojiList Component
export default function EmojiList() {
  // Emoji array and state setter using the useState hook
  const emojiArr = ["😊 ", "🌟 ", "🚀 ", "🌈 ", "🎉 ", "🌸 ", "📚 ", "🎵 ", "🍕 ", "🌍 "];
  const [emojis, setEmojis] = useState(instantiate);

  // Function to add a new random emoji to the array
  function addEmoji() {
    // Using the spread operator to create a new array with the existing emojis and a new one
    const newEmojis = [...emojis, emojiHelper(emojiArr)];
    // Setting the state with the updated array
    setEmojis(newEmojis);
  }

  // JSX rendering
  return (
    <>
      {/* Render each emoji in the array using map */}
      {emojis.map((e, index) => (
        <span style={{ fontSize: '2rem' }} key={index}>{e}</span>
      ))}
      {/* Button to add a new emoji */}
      <div>
        <button onClick={addEmoji}>Add Emoji</button>
      </div>
    </>
  );
}
```

1. **Map Function with Parentheses `()` vs. Curly Braces `{}`:**

   [Map Function Refresher](js_array_callback_methods.md#map)

   [Arrow Function Refresher](js_array_callback_methods.md#arrow-functions)
   - When using the `map` function, the arrow function inside the parentheses `()` implies an implicit return. This is suitable for single-line expressions.
   - If curly braces `{}` are used, a `return` statement is required for multiline expressions.

2. **Calling and Passing Function in `useState()`:**
   - The `useState` hook is employed to initialize the state variable `emojis`.
   - A function, such as `instantiate`, can be passed to `useState` for lazy initialization. This function is called only during the initial render, boosting performance by ignoring the subsequent rendering.

3. **Instantiating with an Array in `useState()`:**
   - To initialize the state as an array, the `useState` hook is set to a function that returns an array. In this case, `instantiate` returns an array containing a single emoji.

4. **Adding a New Emoji:**
   - The `addEmoji` function uses the spread operator to create a new array with the existing emojis and a randomly selected new emoji. This updated array is then set as the new state.

## Generating IDs with UUID

UUIDs, or Universally Unique Identifiers, are a widely used solution for creating unique IDs. Here's a guide on how to generate IDs using UUIDs in JavaScript.

### Using `uuid` Library in JavaScript

The `uuid` library provides a convenient way to generate UUIDs in JavaScript. You can install it using npm or yarn:

```bash
npm install uuid
# or
yarn add uuid
```

### React Integration

1. **Importing UUID Function:**

   ```jsx
   import { v4 as uuid } from "uuid";
   ```

   In this line, the `v4` function from the `uuid` library is imported and given the alias `uuid`. The `v4` function is a version 4 UUID generator, and using an alias makes it easier to reference.

2. **Generating Unique IDs:**

   ```jsx
   const newEmojis = [...emojis, { id: uuid(), emoji: emojiHelper(emojiArr) }];
   ```

   In the `addEmoji` function, a new emoji object is created with a unique ID generated by calling `uuid()`. The `{ id: uuid(), emoji: emojiHelper(emojiArr) }` part creates an object with two properties: `id` (holding the unique identifier) and `emoji` (holding a randomly selected emoji).

3. **Assigning Unique Keys in Rendering:**

   ```jsx
   {emojis.map((e, index) => {
     return (
       <span style={{ fontSize: "20px" }} key={e.id}>
         {e.emoji}
       </span>
     );
   })}
   ```

   When rendering the emojis using the `map` function, each `<span>` element is given a unique `key` attribute with the value of the `id` property of the emoji. This helps React efficiently update and manage the list, ensuring each emoji has a distinct identifier.

### Ensuring Uniqueness

- To ensure that the initial state of the `emojis` array contains an object with a unique identifier, the `useState` hook is initialized with a function named `instantiate`:

    ```jsx
    const [emojis, setEmojis] = useState(instantiate);

    function instantiate() {
        console.log("instantiating");
        return [{ id: uuid(), emoji: "😊" }];
    }
    ```

- The `useState` hook is initialized with the `instantiate` function. This function will be called only during the initial rendering of the component, providing the initial state value for `emojis`.
- **Logging Instantiation:** &mdash; The `console.log("instantiating")` line is for demonstration purposes, showing that this function is called only once during the initial rendering.
   **Returning Initial State:**
- The function returns an array containing an object with two properties: `id` (a unique identifier generated using `uuid()`) and `emoji` (set to "😊").

## Deleting from Arrays — React Way

[Filter Callback Function Refresher](js_array_callback_methods.md#filter)

When managing a list of items in React, the process of deleting an item involves updating the state to reflect the removal of the selected item. The React way of achieving this is demonstrated using the `filter` function.

**Deleting Emoji:**

- The `deleteEmoji` function is responsible for removing an emoji from the state.
  - It uses the `filter` function to create a new array (`prevEmojis.filter(e => e.id !== id)`) that excludes the emoji with the specified `id`.
    - The state is then updated with the new array.

    ```jsx
    const deleteEmoji = (id) => {
        setEmojis(prevEmojis => prevEmojis.filter(e => e.id !== id));
    }
    ```

**Rendering Emojis:**

- The emojis are rendered within a `span` element.
  - Each emoji is given a unique `key` based on its `id` for efficient React rendering.
    - The `onClick` event is set to trigger the `deleteEmoji` function, removing the respective emoji.

    ```jsx
      {emojis.map((e, index) => {
        return (
          <span
            onClick={() => deleteEmoji(e.id)}
            style={{ fontSize: "20px", cursor: "pointer" }}
            key={e.id}
          >
            {e.emoji}
          </span>
        );
      })}
    ```

### Deletion Process

- When an emoji is clicked, the `deleteEmoji` function is called with the `id` of the clicked emoji.
  - The `onClick={() => deleteEmoji(e.id)}` expression is a concise way in React to handle a click event and pass a specific argument `(e.id)` to the `deleteEmoji` function. This pattern prevents immediate invocation and ensures the function is called only when the component is clicked.
- The `filter` function is employed to create a new array that excludes the emoji with the specified `id`.
- The state is updated with this new array, resulting in a re-render with the selected emoji removed.

## Common Array Updating Patterns

> [Updating Arrays in State](https://react.dev/learn/updating-arrays-in-state) from [Official React Documentation](https://react.dev)
>> Arrays are mutable in JavaScript, but you should treat them as immutable when you store them in state. Just like with objects, when you want to update an array stored in state, you need to create a new one (or make a copy of an existing one), and then set state to use the new array.

Updating arrays in React involves common patterns to ensure state immutability and trigger re-renders. Below are some common array updating patterns in React:

### 1. **Adding an Item:**

- Use spread operator (`...`) for copying arrays. Adding a new item to an array using the spread operator:

    ```js
    const [items, setItems] = useState([]);

    const addItem = (newItem) => {
        setItems([...items, newItem]);
    };
    ```

### 2. **Removing an Item:**

- Removing an item based on its index using `filter`:

    ```js
    const removeItem = (idx) => {
        return setItems(items.filter((_, index) => index !== idx));
    };
    ```

### 3. **Updating an Item:**

- Updating an item based on its index using `map`:

    ```js
    const updateItem = (indexToUpdate, updatedItem) => {
        setItems(
            items.map((item, index) =>
                index === indexToUpdate ? updatedItem : item
            )
        );
    };
    ```

    ```jsx
    // Updating the whole array

    const makeAllStar = () => {
        setEmojis(prevEmojis => prevEmojis.map(p => {
          return {...p, emoji: '🌟'}
         }));
    };
    ```

### 4. **Toggling Boolean Value:**

- Toggling a boolean value in an item:

    ```jsx
    const toggleBoolean = (indexToToggle) => {
        setItems(
            items.map((item, index) =>
                index === indexToToggle ? { ...item, active: !item.active } : item
            )
        );
    };
    ```

### 5. **Replacing Entire Array:**

- Replacing the entire array with a new one:

    ```jsx
    const replaceArray = (newArray) => {
        setItems(newArray);
    };
    ```

### Key-Points

- Always use the spread operator (`...`) to create a new array or object to maintain immutability.
- Use array methods like `map` and `filter` for updating and filtering items.
- When updating specific properties of an item, create a new object with the changes.
- Ensure the unique key property for each item when rendering in a component.

These patterns help manage state changes effectively, ensuring React detects updates and triggers re-renders as needed.

## Score Keeper Exercise

```jsx
import { useState } from "react";

export default function ScoreKeeper({ numOfPlayers = 3, winningScore = 3 }) {
  const scoreArr = new Array(numOfPlayers).fill(0);
  const [scores, setScores] = useState(scoreArr);
  const incrementScore = (idx) => {
    // setScores((prevScore) => {
    //   const newScore = [...prevScore];
    //   newScore[idx] += 1;
    //   return newScore;
    // });

    setScores((prevScore) => {
      return prevScore.map((score, i) => {
        return idx === i ? (score + 1) : score;
      });
    });

    // setScores((prevScores) => {
    //   return prevScores.map((score, i) => (i === idx ? score + 1 : score));
    // });
  };
  const childLI = scores.map((score, idx) => {
    return (
      <div>
        <li key={idx} style={{ display: "inline-block" }}>
          Player {idx + 1}'s Score : {score}{" "}
        </li>
        <button
          onClick={() => incrementScore(idx)}
          style={{ marginLeft: "16px" }}
        >
          +1
        </button>
        {scores[idx] >= winningScore && (
          <h4 style={{ display: "inline-block" }}>Winner</h4>
        )}
      </div>
    );
  });

  return (
    <>
      <h1>Score Keeper</h1>
      <ul>{childLI}</ul>
      <button onClick={() => setScores(scoreArr)}>Reset</button>
    </>
  );
}
```
