# React Hooks

## Rules

- Special builtin functions that allows us to **hook** into the React Internals:
  - **Creating and accessing states** from Fibre tree &mdash; `useState`
  - **Registering effects** in fibre tree &mdash; `useEffect`
  - Manual **DOM Selections** / DOM Manipulations &mdash; `useRef`
  - Always starts with `use` like `useEffect`, `useState`
  - Enable easy **reusing non-visual logic** (everything that's not in our UI &mdash; side effects): we can **compose multiple hooks into our own custom hook**
  - Gave Functional Components  the ability to own states and run side effects during the [lifecycle points](react_component_design.md#component-instance-lifecycle)