# React Props

- Props enable the creation of configurable components.
- They allow components to receive and utilize external data.
- In a component declaration, we pass `props` as a parameter to access the data.
  - We call the `Greeter` component and pass the `name` as its argument.
  - The syntax resembles HTML attribute syntax, such as `<a href="https://google.com">` or `<input type='password'>`.

    ```jsx
    import Greeter from './Greeter'

    <Greeter name='David' />
    <Greeter name='Milan' />
    ```

  - The `Greeter` component then uses the passed arguments.

    ```jsx
    export default function(props){
        console.log(props);
        return(
            <h1>Hi there, {props.name}</h1>;
        )
    }
    ```

## Destructuring Props

- Destructuring simplifies the use of props by directly extracting properties.
- Instead of `props.name`, we can use `{ name }` directly.

    ```jsx
    export default function({ name }){
        console.log(name);
        return(
            <h1>Hi there, {name}</h1>;
        )
    ```

## Passing Multiple Props

- When calling a component, you can provide multiple attributes, each representing a different prop, just like you would with HTML attributes

**`Component Definition (e.g., Greeter.jsx):`**

  ```jsx
    export default function({ name, from }){
        console.log(props);
        return(
            <h1>Hi there, {name} &mdash; {from}</h1>;
        )
    }   
  ```

**`Component Definition (e.g., App.jsx):`**

  ```jsx
  // App.jsx

      // import Greeter from './Greeter';

      // function App() {
      //   return (
      //     <>
      //       <Greeter name="David" from="Milan" />
      //       <Greeter name="Alice" from="New York" />
      //     </>
      //   );
      // }

      // export default App;

  import Greeter from './Greeter';

  export default () => <Greeter name='David' from='Malan' />;
  ```

## Passing Non-String Props

- For non-string props, use `{}` to pass JavaScript expressions.
  - To create a different-sided dice roll

  ```jsx
  export default function Die({ numSided }){
    return(
      <p>{numSided}-sided die roll: {Math.floor(Math.random() * numSided) + 1};
    )
  }
  ```

  ```jsx
  import Die from './Die'

  export default function App(){
    return (
      <Die numSided={20} />
      <Die numSided={10} />
      <Die numSided={6} />
    )
  }
  ```

## Setting Default Props Values

- Setting default values for props in React is achieved using the same syntax as providing default values for JavaScript object properties. This ensures that if a prop is not explicitly passed when using a component, it defaults to the specified value.

```jsx
export default function Die({ numSided = 6 }) {
  const roll = Math.floor(Math.random() * numSided) + 1;
  return (
    <h4>
      {numSided}-sided roll: {roll}
    </h4>
  );
}
```

If `numSided` is not passed, it defaults to 6.

```jsx
import Die from './Die'

export default function App(){
  return (
    <Die numSided={20} />
    <Die numSided={10} />
    <Die />
  )
}
```

In React, passing arrays and objects as props is common and can be done in a straightforward manner. Here's how you can pass and use arrays and objects as props:

## Passing an Array as a Prop

```jsx
// ParentComponent.jsx
import ChildComponent from './ChildComponent';

function ParentComponent() {
    // const colors = ['red', 'green', 'blue'];
    // return <ChildComponent colors={colors} />;

  return <ChildComponent colors={['red', 'green', 'blue']}>
}
```

```jsx
// ChildComponent.jsx
export default function ChildComponent({ colors }) {
  return (
    <div>
      <p>Colors:</p>
      <ul>
        {colors.map((color, index) => (
          <li key={index}>{color}</li>
        ))}
      </ul>
    </div>
  );
}
```

In this example, the `ParentComponent` passes an array (`colors`) to the `ChildComponent` as a prop. The `ChildComponent` then maps over the array to display each color.

## Passing an Object as a Prop

```jsx
// ParentComponent.jsx
import ChildComponent from './ChildComponent';

function ParentComponent() {
  const person = {
    name: 'John Doe',
    age: 25,
    city: 'New York',
  };

  return <ChildComponent person={person} />;
}
```

```jsx
// ChildComponent.jsx
export default function ChildComponent({ person }) {
  return (
    <div>
      <p>Name: {person.name}</p>
      <p>Age: {person.age}</p>
      <p>City: {person.city}</p>
    </div>
  );
}
```

Here, the `ParentComponent` passes an object (`person`) to the `ChildComponent` as a prop. The `ChildComponent` then accesses individual properties of the object to display information.
