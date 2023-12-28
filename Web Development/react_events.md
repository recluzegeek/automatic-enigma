# React Events

## React Click Events & Non-Click Events

- All JavaScript events in JSX follow camelCase naming conventions. For instance, `onclick` becomes `onClick`, and `onmouseover` becomes `onMouseOver`.
- _**Note:**_ Avoid using `onMouseOver={hover()}` to prevent immediate function execution. Instead, use `onMouseOver={hover}` to respect JSX event handling.

    ```jsx
    // EventHandler.jsx --- Component Definition

    const hover = () => console.log("Button was hovered...\n");

    export default function EventHandler() {
    return (
        <>
            <h1 onMouseOver={hover}>Hello! Hover me!</h1>
            <button onClick={() => console.log("Button was clicked")}>Click Me!!!</button>
        </>
    );
    }
    ```

## Working with Event Objects &mdash; Key Events

- Utilize event handlers to prevent form submission using `e.preventDefault()` when `e` is present in the function parameters.

    ```jsx
    // Form.jsx --- Component Definition

    function handleFormSubmission(e){
        e.preventDefault();
        console.log('Form submitted!!!');
    }

    export default function Form(){
        return (
            <form onSubmit={handleFormSubmission}>
                <button type='submit'>Submit Form</button>
            </form>
        )
    }
    ```
