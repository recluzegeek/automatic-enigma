# JSX Details

## Importing and Exporting Components

In React, we commonly use import and export syntax to organize and share components across different files. Let's explore this through a simple example with two components, Header and Footer, each in its own file.

Assuming these files:

- **Header.js**

  ```jsx
  import React from 'react';

  const Header = () => {
    return <h1>This is the Header component</h1>;
  };

  export default Header;
  ```

- **Footer.js**

  ```jsx
  import React from 'react';

  const Footer = () => {
    return <footer>&copy; 2023 My Website</footer>;
  };

  export default Footer;
  ```

Now, to use these components in another file, let's say in an `App.js` file:

- **App.js**

  ```jsx
  import React from 'react';
  import Header from './Header';
  import Footer from './Footer';

  const App = () => {
    return (
      <div>
        <Header />
        <p>Main Content Goes Here</p>
        <Footer />
      </div>
    );
  };

  export default App;
  ```

Here's a breakdown:

- Header and Footer components are defined in their respective files (`Header.js` and `Footer.js`).
- They are shared with other files using `export default`.
- In `App.js`, these components are imported using the `import` statement and used seamlessly y, add as customAdd } from './utils';

// Now you can use custom names for the imported functions
const result = customMultiply(3, 4);
const sum = customAdd(5, 6);in JSX.

**_Adjust the paths in the `import` statements based on the project's file structure._**

- For a shorter syntax, especially when exporting a function directly:

  ```js
  import React from 'react';

  // Exporting the Greeter component using shorter syntax
  export default () => <h1>Hello World!</h1>;
  ```

  - The function component is defined and directly exported as the `default export` in a concise manner. Import and use it in another file using the `import` statement.

    | Aspect                 | Named Export                           | Default Export                         |
    |------------------------|----------------------------------------|----------------------------------------|
    | **Syntax**             | `export { VariableName };`             | `export default VariableName;`         |
    | **Import Syntax**      | `import { VariableName } from './File';`| `import VariableName from './File';`   |
    | **Usage**              | Multiple named exports in a file        | Only one default export per file       |
    | **Import Customization**| Import can be customized with aliasing | Default export can be aliased or renamed|

---

In JavaScript, customization during import is common when dealing with named exports or default exports.

### Customizing Named Exports (Aliasing)

Assuming a module with named exports, such as:

**File: `utils.js`**

```js
export const multiply = (a, b) => a * b;
export const add = (a, b) => a + b;
```

Customize the import in another file:

**File: `app.js`**

```js
import { multiply as customMultiply, add as customAdd } from './utils';

// Now you can use custom names for the imported functions
const result = customMultiply(3, 4);
const sum = customAdd(5, 6);
```

### Customizing Default Exports (Renaming)

Assuming a module with a default export:

**File: `greeter.js`**

```js
const Greeter = () => <h2>Hello</h2>;
export default Greeter;
```

Customize the import by renaming the default export:

**File: `app.js`**

```js
import CustomGreeter from './greeter';

// Now you can use the custom name for the imported component
const greeting = <CustomGreeter />;
```

## JSX Rules

### Explicitly close self-closing elements

- In JSX, self-closing elements like `<br />` or `<input type='password' />` should be explicitly closed with a trailing slash.

    ```jsx
    // Correct
    <br />
    <input type='password' />

    // Incorrect
    <br>
    <input type='password'>
    ```

### A component can only return a single element

- A React component can only return a single element. To wrap multiple elements without introducing an additional parent div, you can use a **fragment** (`<></>`).

    ```jsx
    // Correct
    const MultiElementComponent = () => (
    <>
        <h1>Title</h1>
        <p>Paragraph</p>
    </>
    );

    // Incorrect (without using a fragment)
    const IncorrectComponent = () => (
        <h1>Title</h1>
        <p>Paragraph</p>
    );
    ```

Using the fragment (`<>` and `</>`) allows you to group multiple elements without introducing an extra parent div, which is especially useful when strict nesting is required in React components and there's a desire to avoid introducing an extra wrapper element.

## Evaluating JS Expression in JSX

- Anything that's inside curly braces `{}` will be evaluated as JS expression.

  ```jsx
  import React from 'react';

  function sum(a, b) {
    return a + b;
  }

  export default function MyComponent() {
    return <h1>Sum of 2 and 3 is {sum(2, 3)}</h1>;
  }
  ```

- Now inside the **`App.jsx`**, import the `MyComponent` and see the result.

## Component Decomposition

- Component decomposition in React involves breaking down complex components into smaller, reusable, and manageable pieces.
  - This practice enhances code readability, maintainability, and encourages the reuse of components across the application.

Consider a complex `UserProfile` component that displays user information, including their profile picture, name, and a list of recent activities. Let's decompose it into smaller components:

  ```jsx
  import React from 'react';

  // ProfilePicture component
  const ProfilePicture = ({ imageUrl }) => <img src={imageUrl} alt="Profile" />;

  // UserName component
  const UserName = ({ name }) => <h2>{name}</h2>;

  // RecentActivities component
  const RecentActivities = ({ activities }) => (
    <ul>
      {activities.map((activity, index) => (
        <li key={index}>{activity}</li>
      ))}
    </ul>
  );

  // UserProfile component using decomposition
  const UserProfile = ({ user }) => {
    const { imageUrl, name, activities } = user;

    return (
      <div>
        <ProfilePicture imageUrl={imageUrl} />
        <UserName name={name} />
        <RecentActivities activities={activities} />
      </div>
    );
  };

  export default UserProfile;
  ```

In this example, we decomposed the `UserProfile` component into smaller components (`ProfilePicture`, `UserName`, and `RecentActivities`). Each smaller component has a specific responsibility, making the code more modular and easier to maintain. The `UserProfile` component then assembles these smaller components to render the complete user profile.

## Styling Components

Styling components in React involves several approaches, and one common method is using CSS. Here are some guidelines:

### Importing CSS Files

```jsx
// MyComponent.jsx
import './MyComponent.css';

const MyComponent = () => {
  return <div className="myComponent">Hello, World!</div>;
};
```

### Naming Convention

- It's a good practice to name the CSS file similarly to the component file. For example, `MyComponent.jsx` corresponds to `MyComponent.css`.
- This naming convention helps maintain a clear and organized structure in your project.

### Global Styles Note

- Importing a CSS file applies its styles globally across all components, regardless of whether the CSS file is explicitly imported in each component.
- This is important to note because styles can inadvertently affect components that you might not expect.

  - Example CSS File (`MyComponent.css`):

  ```css
  /* MyComponent.css */
  .myComponent {
    color: red;
    font-size: 18px;
  }
  ```
