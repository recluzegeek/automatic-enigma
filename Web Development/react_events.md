# React Events

[Learn-More-About-Events-On-react.dev](https://react.dev/learn/responding-to-events)

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

## Event Delegation

 Events in the DOM propagate in two phases: capturing and bubbling.

1. **Event Propagation in the DOM:**

   - The event is generated and starts its journey at the root and travels down the tree during the capturing phase, reaching the target element.
   - After reaching the target, the event bubbles back up during the bubbling phase.
     - It's crucial to understand that during the capturing and bubbling phases, the event object traverses through each child and parent element individually. It's almost like the event occurs in every single element.
     - By default, event handlers are configured to react to events on the initial target element and stay active as the event bubbles up through the webpage's structure.

        ```js
        // prevent event propagation
        e.stopPropagation()
        ```

2. **Event Delegation in the DOM:**
   - Event delegation is a technique where a single event listener is used to manage events for multiple elements.
   - Instead of attaching event listeners to individual elements, you attach them to a common ancestor.
   - This is efficient and works well with dynamically generated content.

3. **React's Handling of Events:**
   - React uses synthetic events, which are wrappers around native DOM event objects.
   - Synthetic events fix browser inconsistencies and ensure a consistent event handling approach across browsers.
   - All important synthetic events in React bubble, even those that don't bubble natively (e.g., defocus, blur, change).

4. **Event Handlers in React:**
   - React registers event handlers at the root DOM container, not on individual elements.
   - This is achieved during the render phase, with one event handler per event type.
   - Event delegation is performed by React behind the scenes, making React apps more performant.

5. **Differences from Vanilla JavaScript:**
   - Event handler prop names in React use camelCase (e.g., `onClick`), unlike vanilla JavaScript (e.g., `click`).
   - To prevent default behavior, React uses `preventDefault` on the synthetic event, not by returning `false`.
   - In rare cases where capturing is needed, `capture` can be added to the event handler name.

### [Click to learn the Event Summary](./react_working_behind_scenes.md#03-react-behind-the-scenes)