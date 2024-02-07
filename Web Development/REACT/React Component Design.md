# React Component Types

- What props do we need ?
- What state do we need ?
- And where ?
  - State can't be passed upwards. Data is passed uni-directional from parents to child and not vice versa.

**Presentational / Stateless / Dumb Components** &mdash; Focus on rendering UI elements and receiving props. Doesn't have a state; is primarily about appearance/UI

**Logical / Stateful Components** &mdash; Responsible for managing state and handling business logic. Has state or related logic, isn't about a specific UI

## Component (Instance) Lifecycle

This cycle involves three key phases: Mount/Initial Render, Re-Render (optional), and Unmount.

### Mount / Initial Render

- Component Instance is rendered for the very first time
- Fresh states and Props are created

### Re-Render &mdash; Optional

- Happens when (we're talking in the context of component instance and not the entire application)
  - State Changes
  - Props Changes
  - Parent Re-render
  - Context Changes

### Unmount

- Component instance is removed and destroyed
- State and Props are destroyed

## State Management

> **Lift the state as high as needed &mdash; but no higher.**
>
- Think about where the state will live?
  - In case of **LuckyN Dice** &mdash; (we roll dices and checks whether the sum of the rolls is equal to some goal or not), where should the state lie? In the LuckyN component or Dice Component which in turn is again parent to individual die components.

    - **App.jsx** &mdash; don't need it, so shouldn't put it.
    - **LuckyN.jsx** &mdash; this is the game itself (ideal)
    - **Dice.jsx** &mdash; should just be showing a hand
    - **Die.jsx** &mdash; render each die component onto screen, no need for state and also we need the sum of every Die not just of a single instance, so we should lift the state up.

### Passing State and Functions as Props

In React, sometimes the data you're working with stays within a component (it doesn't go to other components), but it's also common to share the state with child components.

- A common use case for passing functions as props is to allow child components to update the state of the parent. [Click here to learn more](./react_state_management.md#inverse-data-flow)

**Summary:**

- **Parent components define functions.**
- **These functions are passed as props to child components.**
- **Child components can then invoke these functions.**

## Component Composition

- Combining different components using `children` props ([learn about them by clicking here](./react_props.md#passing-children-prop)) or explicitly defined props.
- We use component composition for:
  - Create highly reusable and flexible components
  - **Fix prop drilling** &mdash; great for layouts... ([learn about prop drilling here](./react_props.md#props-drilling))
