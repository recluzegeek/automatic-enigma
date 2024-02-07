# React Router

React Router is a library that enables navigation and routing within a React application. It allows you to create a Single Page Application (SPA) by defining different routes for your components, enabling navigation between them based on the URL.

## Setting Up Routing

### Installation

Firstly, you need to install `react-router-dom` using npm:

```bash
npm install react-router-dom
```

In your `App.js` file, import necessary components and set up the routing.

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import HomePage from './HomePage';
import Product from './Product';
import AboutUs from './AboutUs';
import NotFound from './404';
```

- `BrowserRouter`: This component provides the routing context for your application. It uses the HTML5 history API to keep your UI in sync with the current browser URL.

- `Routes`: This component is used to define your routes. Inside it, you place `Route` components that define the mapping between a URL path and a React component.

- `Route`: Each `Route` component corresponds to a specific URL path and is associated with a React component (provided using the `element` prop).

- **Route Definitions**

  ```jsx
  export default function App() {
    return (
      <BrowserRouter>
        <Routes>
          <Route path='/' element={<HomePage />} />
          <Route path='Product/:id' element={<Product />} />
          <Route path='AboutUs' element={<AboutUs />} />
          <Route path='*' element={<NotFound />} />
        </Routes>
      </BrowserRouter>
    );
  }
  ```

- `<Route path='/' element={<HomePage />} />`: This defines a route for the root URL (`/`) and associates it with the `HomePage` component.

- `<Route path='Product/:id' element={<Product />} />`: This defines a route for URLs like `/Product/123`, where `123` is a dynamic parameter (`id`). The `Product` component will receive this parameter.

- `<Route path='AboutUs' element={<AboutUs />} />`: This defines a route for the `/AboutUs` URL, associated with the `AboutUs` component.

- `<Route path='*' element={<NotFound />} />`: This is a catch-all route. If none of the specified routes match, the `NotFound` component will be rendered. The `*` here acts as a wildcard.

### Component Rendering

In each `Route`, the `element` prop is used to specify the React component to render when the route is matched.

```jsx
export default function HomePage() {
  return(
    HomePage....
  )
}
```

## Key Points

React Router simplifies navigation in your React applications. The code provided sets up a basic routing configuration using the `BrowserRouter`, `Routes`, and `Route` components. It defines routes for different URLs, associating them with corresponding React components. Additionally, it includes a catch-all route for handling unknown URLs, rendering a `NotFound` component.
