# React Hooks

## Rules

- Special builtin functions that allows us to **hook** into the React Internals:
  - **Creating and accessing states** from Fibre tree &mdash; `useState`
  - **Registering effects** in fibre tree &mdash; `useEffect`
  - Manual **DOM Selections** / DOM Manipulations &mdash; `useRef`
  - Always starts with `use` like `useEffect`, `useState`
  - Enable easy **reusing non-visual logic** (everything that's not in our UI &mdash; side effects): we can **compose multiple hooks into our own custom hook**
  - Gave Functional Components  the ability to own states and run side effects during the [lifecycle points](react_component_design.md#component-instance-lifecycle)

### State Hook &mdash; [useState](react_use_state.md)

### Effect Hook &mdash; [useEffect](react_use_effect.md)

### Ref Hook &mdash; [useRef](react_use_ref.md)

### Reducer Hook &mdash; [useReducer](react_use_reducer_hook.md)

### Memo &mdash; [memo](react_memoization.md#memo-function)

### useMemo &mdash; [useMemo](react_memoization.md#usememo-hook)

### useCallback &mdash; [useCallback](react_memoization.md#usecallback-hook)

### Context API

- Allows us to read a state from everywhere
- A solution to [Prop Drilling](react_props.md#props-drilling)
  - Passing state into multiple deeply nested child components

### What is Context API?

- System to pass data throughout the app without manually passing props down the tree
- Allows us to **broadcast global state** to the entire app
  - **Provider:** &mdash; gives all child components access to `value`
    - **Value:** &mdash; data that we want to make available (usually state/s and setter function/s)
  - **Consumers:** &mdash; all components that read the provided context `value`
    - Consumers are the components that are subscribed to the context
- We can create as many consumers as we want for any context
- When the `value` is updated, all the **consumers are re-render**

### Creating and Providing the Context

The `createContext` and `useContext` functions provide an elegant solution to address this issue.

#### **Creating Context**

In the root `App` component or any appropriate level, we initiate context using the `createContext` function imported from React. This function sets up a context with an optional initial value.

```jsx
import React, { createContext } from 'react';

const PostContext = createContext();
```

#### Providing Context

Context is provided using the `Provider` component, which wraps the child components. In the example below, the `PostContext.Provider` encompasses the `ChildComponent` and passes relevant values through the `value` prop.

```jsx
import React, { useState } from 'react';
import { PostContext } from './App';

export default function App() {
  const [count, setCount] = useState(0);

  return (
    <PostContext.Provider value={{ count, setCount }}>
      <ChildComponent />
    </PostContext.Provider>
    // remaining component logic
  );
}
```

#### Exporting Context

Ensure that the context is exported so that consumers can access it. This is typically done using named exports.

```jsx
export { PostContext };
```

#### Consuming Context

In the consumer component, import the context from the relevant file and utilize the `useContext` hook to extract the required values.

```jsx
import React, { useContext } from 'react';
import { PostContext } from './App';

export default function ChildComponent() {
  const { count, setCount } = useContext(PostContext);

  return (
    <h1>Component Count: {count}</h1>
  );
}
```

**Create Context:** &mdash; Use `createContext` to initialize a context, defining the structure and initial values if needed.

**Provide Values** &mdash; Wrap the child components in the `Provider` to supply them with the required values. This is achieved by setting the `value` (which is an object) prop within the `Provider`.

**Consume Values:** &mdash; In the child component, use the `useContext` hook to access the values provided by the context.
