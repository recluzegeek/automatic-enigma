# Introducing State

- State is like the memory or notes your program keeps for each of its parts, allowing them to remember and change as needed.
  - Imagine your program is like a living being, and each part of it, whether it's a button, a form, or a section of a webpage, has its own little notepad. This notepad is the "**state**." Now, this notepad can hold different information, like a counter's current number, whether a button is clicked, or even the user's name in a form.

- **Here's the thing:** _This notepad is not set in stone._ **It's dynamic. It can change.**
  - For example, if you click a button, the number on the notepad might go up. If you fill out a form, the notepad might now have your name on it.

- So, in react, we use "state" to keep track of these changing bits of information. It's like having a memory for each part of the program, allowing it to remember and adapt as things happen.

## When to Use State?

State is employed when you need to keep track of information that can be altered or updated during the execution of a program.

The set functions returned by `useState` are used to update the state. They can be simple or involve more complex logic, depending on the use case. Here are examples of the `useState()` and **set functions** with different logics:

1. **Simple Increment/Decrement:**

   ```jsx
   const [count, setCount] = useState(0);

   // Increment
   const handleIncrement = () => {
     setCount(count + 1);
   };

   // Decrement
   const handleDecrement = () => {
     setCount(count - 1);
   };
   ```

2. **Toggle Visibility:**

   ```jsx
   const [isVisible, setIsVisible] = useState(false);

   // Toggle visibility
   const toggleVisibility = () => {
     setIsVisible(!isVisible);
   };
   ```

3. **Updating Form Data:**

   ```jsx
   const [formData, setFormData] = useState({ username: '', password: '' });

   // Update form data
   const handleInputChange = (event) => {
     const { name, value } = event.target;
     setFormData({ ...formData, [name]: value });
   };
   ```

4. **Filtering and Sorting:**

   ```jsx
   const [filter, setFilter] = useState('');

   // Update filter
   const handleFilterChange = (newFilter) => {
     setFilter(newFilter);
   };
   ```

1. **Multi-Step Forms:**

   ```jsx
   const [currentStep, setCurrentStep] = useState(1);

   // Move to the next step
   const goToNextStep = () => {
     setCurrentStep((prevStep) => prevStep + 1);
   };
   ```

## Using State in React

In React, we create state using the `useState` hook. Hooks in React are built-in functions, and their names typically start with "use" (e.g., `useState`, `useReducer`, `useMemo`).

```jsx
import { useState } from "react";

const ExampleComponent = () => {
  const [count, setCount] = useState(0); // returns state variable and setter function

  return (
    <>
      <h1>Count is: {count}</h1>
      <button onClick={() => setCount(count + 1)}>Increment Button</button>
      <button onClick={() => setCount(count - 1)}>Decrement Button</button>
    </>
  );
};
```

- The `useState` hook returns an array containing two elements: the piece of state itself and a function to change the state (`setCount` in this example).

- **Note:** Remember to call `useState` inside a React component.

## Multiple Pieces of State

You can use multiple `useState` hooks to manage different pieces of state independently within a component. Each piece of state is isolated from the others.

```jsx
const ExampleComponent = () => {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("John");

  // ... rest of the component
};
```

## useState and Rendering (Component Lifecycle)

[Click here to Learn about Component Lifecycle](./React%20Component%20Design.md#component-instance-lifecycle)

## States vs Props

| Feature                 | State                                        | Props                                      |
|-------------------------|----------------------------------------------|--------------------------------------------|
| **Ownership**           | Internal data owned by the component.        | External data owned by the parent component.|
| **Location**            | Stored in the component's memory.            | Similar to function parameters, passed from parent. |
| **Mutability**          | Can be updated by the component itself.      | Read-only (immutable) — cannot be modified by the component. |
| **Update Trigger**      | Updating state causes the component to re-render. | Receiving new props triggers re-render, usually when the parent component is updated. |
| **Example Declaration** | `const [count, setCount] = useState(0);`     | `<MyComponent propValue={someValue} />`     |

## Summary

- State is analogous to **Components Memory**
- Updating component state triggers React to re-render the component.
- State allows developers to:
  - Update the component's view by re-rendering it.
  - Persists the local variables between renders.
