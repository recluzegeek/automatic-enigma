# Memoization

- Memoization is an optimization technique that **executes a pure function once**, and saves the result in memory. If we try to execute the same function again with **same arguments as before**, the previously saved result will be returned, **without executing the function again**.
  - Utilizing memoization provides advantages such as:
    - **Prevent wasted renders**
    - **Improve app speed and responsiveness**

## Optimization using Memoization

- Memoize **components** with `memo`
- Memoize **objects/values** with `useMemo`
- Memoize **functions** with `useCallback`

## `memo` Function

>[Excellent Official React Documentation](https://react.dev/reference/react/memo)
>
>> `memo` lets you skip re-rendering a component when its props are unchanged.
>>
>>> ```jsx
>>> const MemoizedComponent = memo(SomeComponent, arePropsEqual?)
>>> ```

- Memoized Component &mdash; Used to create components that will **not re-render when its parent re-renders**, as long as the **props stays the same between render**
  - Memoization only has to do with props that are passed to the component from its parent.
    - A memoized content will still **re-render when its own state** changes or the **context that its subscribed to changes**. 
    - Make a component memoized using `memo` only when the **component is heavy** (re-rendering is slow), **re-renders often** and **does so with the same props**.
    - Wrap the component inside `memo()` to make it memoized component

    ```jsx
    const Greeting = memo(function Greeting({ name }) {
        return <h1>Hello, {name}!</h1>;
    ;

    export default Greeting;
    ```

### Issue with `memo`

[Excellent Official React Documentation](https://react.dev/reference/react/memo#minimizing-props-changes)

- In React, **everything is re-created on every render** (including objects and functions)
  - One classic example is that two empty objects are actually different `({} != {})`
- Therefore, if objects or functions are passed as props, the child component will always see them as **new props on each re-render**
- **If props are different between re-renders, `memo` will not work**
- We need to memoize objects and functions, to make them stable(preserve) between re-renders `(memoized {} == memoized {})`

## `useMemo` and `useCallback` Hook

- used to memoize values **(useMemo)** and functions **(useCallback) between renders**
  - Values passed into `useMemo` and `useCallback` will be stored in memory ("cached") and returned in **subsequent re-renders, as long as dependencies**("input") **stay the same**
- `useMemo` and `useCallback` have a dependency array (like `useEffect`): whenever **one dependency changes**, the value will be **re-created**

### UseCases

- Only use `useMemo` or `useCallback` for one of the three **use cases!**
  - Memoizing props (using `useMemo`) to prevent wasted renders (together with `memo`)
  - Memoizing values to avoid expensive re-calculation on every re-render
  - Memoizing values that are used in the dependency array of other hooks
    - To avoid infinite `useEffect` loops