# React Forms

- React creates Single Page Applications (SPAs) and doesn't refresh the webpage for new data. Therefore, when submitting forms, we aim to avoid refreshing the webpage.
- To achieve this, we prevent the default HTML form behavior using `e.preventDefault()` within the `onSubmit` event handler.

## Controlled Components Technique

- Typically, the state of HTML input elements is managed by the DOM by default, which is not the preferred approach. For better control and maintainability, we want states to be handled in a central location managed by React, not by the DOM. This leads us to use React-controlled elements.

This ensures that the React component state is the single source of truth for the input field's value.

### Steps to Implement Controlled Components

1. **Create State for Input Value:**

- Initialize a state variable to hold the value of the input field.

   ```javascript
   const [description, setDescription] = useState('');
   ```

1. **Handle Input Changes:**

- Create an event handler function (e.g., `handleInput`) to update the state when the input value changes.

   ```javascript
   function handleInput(e) {
     setDescription(e.target.value);
   }
   ```

1. **Bind State to Input Element:**

- Bind the value of the input field to the state variable (`description`) and set an `onChange` event handler to capture input changes.

    ```jsx
      <input type='text' value={description} onChange={handleInput} />
    ```

1. **Prevent Default Form Behavior:**

- In the `onSubmit` handler of the form, prevent the default form submission behavior to avoid page refresh.

    ```javascript
    function handleSubmit(e) {
        e.preventDefault();
        // Additional logic for form submission if needed
    }
    ```

    ```jsx
    <form onSubmit={handleSubmit}>
        {/* Form fields go here */}
        <button type="submit">Submit</button>
    </form>
    ```

- **Example Code**

    ```jsx
    import React, { useState } from 'react';
    function MyForm() {
    const [description, setDescription] = useState('');
    function handleInput(e) {
        setDescription(e.target.value);
    }
    function handleSubmit(e) {
        e.preventDefault();
        // Additional logic for form submission if needed
    }
    return (
        <form onSubmit={handleSubmit}>
        <label>
            Description:
            <input type='text' value={description} onChange={handleInput} />
        </label>
        <button type="submit">Submit</button>
        </form>
    );
    }
    export default MyForm;
    ```
