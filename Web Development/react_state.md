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

4. **API Data Fetching:**

   ```jsx
   const [data, setData] = useState(null);

   // Fetch data from API
   const fetchData = async () => {
     const result = await fetchDataFromAPI();
     setData(result);
   };
   ```

5. **Theme Switching:**

   ```jsx
   const [theme, setTheme] = useState('light');

   // Toggle theme
   const toggleTheme = () => {
     setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
   };
   ```

6. **Accordion Panels:**

   ```jsx
   const [expandedPanel, setExpandedPanel] = useState(null);

   // Toggle expanded panel
   const togglePanel = (panelId) => {
     setExpandedPanel((prevPanel) => (prevPanel === panelId ? null : panelId));
   };
   ```

7. **Filtering and Sorting:**

   ```jsx
   const [filter, setFilter] = useState('');

   // Update filter
   const handleFilterChange = (newFilter) => {
     setFilter(newFilter);
   };
   ```

8. **Multi-Step Forms:**

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

Let's go through a simple example to illustrate the points mentioned earlier.

Consider a React component using the `useState` hook:

```jsx
import React, { useState } from 'react';

const ExampleComponent = () => {
  // Step 1: State initialization - useState() populates num @ this line
  const [num, setNum] = useState(5);

  // Step 2: State change triggered by an event (e.g., button click)
  const handleButtonClick = () => {
    // Step 3: Calling setNum triggers a re-render
    setNum(num + 1);
    // Note: At this point, num is not immediately updated
    // Step 4: Component is re-rendered, and num is updated @ Step 1
  };

  // Step 5: React renders the component
  return (
    <div>
      <h1>Number: {num}</h1>
      <button onClick={handleButtonClick}>Increment</button>
    </div>
  );
};

export default ExampleComponent;
```

Explanation:

1. **State Initialization:**
   - `useState(5)` initializes the state variable `num` with an initial value of 5.

2. **State Change Trigger:**
   - When the button is clicked (`handleButtonClick` function), it triggers a state change.

3. **Calling setNum:**
   - `setNum(num + 1)` is called, but `num` is not immediately updated. This triggers a scheduled re-render.

4. **Component Re-render:**
   - The component is re-rendered from top to bottom. During this process, the updated value of `num` is fetched.

5. **Rendering Update:**
   - The updated value is now reflected in the UI. React efficiently updates only the parts of the component that depend on the changed state, in this case, the display of `num`.

This separation between scheduling a state change, triggering a re-render, and fetching the updated state value during the render helps React optimize performance by avoiding unnecessary and redundant renders.

### Misunderstanding in React State

- **Triggering Value Change:**
  - It's crucial to note that calling `setNum()` doesn't immediately change the value of `num`. Instead, it schedules a re-render with the updated value.

- **Rendering Update:**
  - The actual update of the `num` value happens when the component is re-rendered. This is when React fetches the latest state value, and your component gets the accurate and updated `num`.

- **Automatic Rendering:**
  - React handles the rendering automatically when state changes. This is a key feature because it abstracts away the manual manipulation of the DOM and allows you to focus on describing how your UI should look based on the current state.

In summary, your state changes are scheduled, and the actual rendering and updating of the state values happen as part of React's automatic rendering process. This separation of concerns makes it easier to reason about your UI's behavior.