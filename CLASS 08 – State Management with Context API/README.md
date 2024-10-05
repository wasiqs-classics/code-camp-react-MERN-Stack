## Class 8: State Management with Context API

Welcome to **Class 8**, where we'll dive into **State Management with Context API** in React. Understanding how to manage state effectively is crucial for building scalable and maintainable applications. The Context API provides a powerful solution to share data across components without the hassle of prop drilling.

---

### Table of Contents

1. [Introduction to Context API](#1-introduction-to-context-api)
   - [What is Context API?](#what-is-context-api)
   - [Why Use Context API?](#why-use-context-api)
   - [Example Scenario of Prop Drilling](#example-scenario-of-prop-drilling)
2. [Setting Up Context API](#2-setting-up-context-api)
   - [Creating a Context](#creating-a-context)
   - [Provider and Consumer](#provider-and-consumer)
3. [Practical Example: Theme Management](#3-practical-example-theme-management)
   - [Creating ThemeContext and ThemeProvider](#creating-themecontext-and-themeprovider)
   - [Using ThemeProvider in the App](#using-themeprovider-in-the-app)
   - [Accessing Context in Components](#accessing-context-in-components)
   - [Styling Based on Theme](#styling-based-on-theme)
4. [Advanced Usage](#4-advanced-usage)
   - [Updating Context Values](#updating-context-values)
   - [Multiple Contexts](#multiple-contexts)
5. [Best Practices](#5-best-practices)
6. [Conclusion](#6-conclusion)
7. [Additional Resources](#7-additional-resources)

---

## 1. Introduction to Context API

### What is Context API?

The **Context API** in React is a mechanism to share data across the component tree without manually passing props at every level. It allows for a global state that can be accessed by any component within its provider, making state management more efficient and reducing the complexity of prop drilling.

### Why Use Context API?

- **Avoid Prop Drilling**: Pass data directly to deeply nested components without intermediate components needing to handle it.
- **Global State Management**: Manage global states like user authentication, themes, or language preferences effortlessly.
- **Enhanced Readability and Maintenance**: Simplify component structures by reducing the need for excessive prop passing.

### Example Scenario of Prop Drilling

**Scenario**: Imagine a React application with three levels of components: `Grandparent`, `Parent`, and `Grandchild`. The `Grandparent` component holds user data that the `Grandchild` component needs to access.

**Without Context API**:
- `Grandparent` passes user data to `Parent` via props.
- `Parent` passes the same user data to `Grandchild` via props.
- This can become cumbersome as the component tree grows.

**With Context API**:
- `Grandparent` provides user data via a context.
- `Grandchild` consumes the user data directly from the context.
- `Parent` no longer needs to pass the data, simplifying the component structure.

---

## 2. Setting Up Context API

### Creating a Context

To begin using Context API, you need to create a context using the `createContext` function.

```jsx
// src/contexts/MyContext.jsx
import React, { createContext } from 'react';

// Create a new context
const MyContext = createContext();

// Export the context to be used by other components
export default MyContext;
```

**Explanation**:
- **`createContext()`**: Initializes a new context. It returns an object with a `Provider` and `Consumer`.
- **Exporting the Context**: Allows other components to access and consume the context.

### Provider and Consumer

- **Provider**: A component that wraps parts of your application and supplies the context value to its children.
- **Consumer**: A component or hook that accesses the context value provided by the nearest Provider.

**Example Structure**:
```
App
 └── ThemeProvider (Provider)
      ├── Navbar
      └── Content
```

In this structure, both `Navbar` and `Content` can access the theme context without needing to pass props through intermediate components.


---

## 3. Practical Example: Theme Management

To solidify our understanding, let's create a simple **Theme Management** system using Context API. Users can toggle between light and dark themes, and the entire application will respond accordingly.
!["Context API Structure"](https://github.com/wasiqs-classics/code-camp-react-MERN-Stack/blob/master/CLASS%2008%20%E2%80%93%20State%20Management%20with%20Context%20API/React%20Context%20API-2024-10-05-002938.png)

### Explanation of the Diagram

Let's break down each part of the flowchart to understand how the Context API works in a React application.

1. **Create Context (`A`)**
   - **Description**: Initialize a new context using `createContext()`.
   - **Purpose**: Establish a context object that will hold the global state or data you want to share across components.

2. **Create Provider Component (`B`)**
   - **Description**: Develop a provider component that uses the `Context.Provider` to supply context values to its child components.
   - **Purpose**: Manage and provide the state or functions that need to be accessible globally.

3. **Wrap Application with Provider (`C`)**
   - **Description**: Enclose the main application (or a part of it) with the provider component.
   - **Purpose**: Ensure that all nested components within the provider can access the context data.

4. **Consumer Components Access Context (`D`)**
   - **Description**: Components like `Navbar` and `Content` that need to consume the context data.
   - **Purpose**: Utilize the shared data or functions provided by the context.

5. **useContext Hook (`E`)**
   - **Description**: Inside consumer components, use the `useContext` hook to access the context values.
   - **Purpose**: Simplify the process of subscribing to context changes and accessing context data without using the `Consumer` component.

6. **Components Receive Context Data (`F`)**
   - **Description**: After using `useContext`, components receive the current context value.
   - **Purpose**: Enable components to use and react to the shared data or state.

7. **Dynamic Styling (`I`)**
   - **Description**: Apply conditional styling based on the received context data (e.g., theme).
   - **Purpose**: Visually reflect changes in the global state, such as switching between light and dark themes.

8. **Light Theme (`J`) & Dark Theme (`K`)**
   - **Description**: Different styling options based on the theme context.
   - **Purpose**: Enhance user experience by allowing theme toggling across the application.

9. **Toggle Theme Function (`L`)**
   - **Description**: A function provided by the context to switch between themes.
   - **Purpose**: Allow user interaction to change the global state, which in turn updates the UI dynamically.

### Creating ThemeContext and ThemeProvider

1. **Create the Context and Provider**:

   ```jsx
   // src/contexts/ThemeContext.jsx
   import React, { createContext, useState } from 'react';

   // Create a new context for theme
   const ThemeContext = createContext();

   // Create a provider component
   const ThemeProvider = ({ children }) => {
     // State to manage the current theme; default is 'light'
     const [theme, setTheme] = useState('light');

     // Function to toggle between 'light' and 'dark' themes
     const toggleTheme = () => {
       setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
     };

     // Context value that will be supplied to any descendants of this provider
     const contextValue = {
       theme,
       toggleTheme,
     };

     return (
       // The Provider component wraps its children and provides the context value
       <ThemeContext.Provider value={contextValue}>
         {children}
       </ThemeContext.Provider>
     );
   };

   export { ThemeContext, ThemeProvider };
   ```

   **Explanation**:
   - **`ThemeContext`**: The context object that holds theme-related data.
   - **`ThemeProvider`**: A component that manages the theme state and provides the `theme` and `toggleTheme` functions to its children.
   - **`contextValue`**: An object containing the current theme and a function to toggle it. This is what components consuming the context will access.

### Using ThemeProvider in the App

2. **Wrap the Application with ThemeProvider**:

   ```jsx
   // src/App.jsx
   import React from 'react';
   import { ThemeProvider } from './contexts/ThemeContext';
   import Navbar from './components/Navbar';
   import Content from './components/Content';
   import './styles.css'; // Import global styles

   function App() {
     return (
       // ThemeProvider provides theme context to all nested components
       <ThemeProvider>
         <div className="app-container">
           <Navbar />
           <Content />
         </div>
       </ThemeProvider>
     );
   }

   export default App;
   ```

   **Explanation**:
   - **Wrapping with `ThemeProvider`**: Ensures that any component within `ThemeProvider` can access the theme context.
   - **`Navbar` and `Content`**: These components will consume the theme context to adjust their styles accordingly.

### Accessing Context in Components

3. **Consume Theme Context in Navbar and Content**:

   - **Navbar Component**:

     ```jsx
     // src/components/Navbar.jsx
     import React, { useContext } from 'react';
     import { ThemeContext } from '../contexts/ThemeContext';

     function Navbar() {
       // Access theme and toggleTheme from ThemeContext
       const { theme, toggleTheme } = useContext(ThemeContext);

       return (
         // Apply theme-based class for styling
         <nav className={`navbar ${theme}`}>
           <h1>My Application</h1>
           <button onClick={toggleTheme}>
             Switch to {theme === 'light' ? 'Dark' : 'Light'} Mode
           </button>
         </nav>
       );
     }

     export default Navbar;
     ```

     **Explanation**:
     - **`useContext(ThemeContext)`**: Hook to access the current theme and toggle function.
     - **Dynamic Button Text**: Changes based on the current theme.
     - **Dynamic Class Name**: Applies different styles based on the theme.

   - **Content Component**:

     ```jsx
     // src/components/Content.jsx
     import React, { useContext } from 'react';
     import { ThemeContext } from '../contexts/ThemeContext';

     function Content() {
       // Access current theme from ThemeContext
       const { theme } = useContext(ThemeContext);

       return (
         // Apply theme-based class for styling
         <div className={`content ${theme}`}>
           <h2>Welcome to the Theme Management App!</h2>
           <p>
             Use the button in the navbar to toggle between light and dark themes. Your preference will be applied throughout the application.
           </p>
         </div>
       );
     }

     export default Content;
     ```

     **Explanation**:
     - **`useContext(ThemeContext)`**: Hook to access the current theme.
     - **Dynamic Class Name**: Applies different styles based on the theme.

### Styling Based on Theme

4. **Define CSS Styles for Themes**:

   ```css
   /* src/styles.css */

   /* Base Styles */
   body {
     margin: 0;
     font-family: Arial, sans-serif;
   }

   .app-container {
     min-height: 100vh;
     display: flex;
     flex-direction: column;
     align-items: center;
     justify-content: center;
     transition: background-color 0.3s ease;
   }

   /* Navbar Styles */
   .navbar {
     width: 100%;
     padding: 1rem;
     display: flex;
     justify-content: space-between;
     align-items: center;
     box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
     transition: background-color 0.3s ease, color 0.3s ease;
   }

   .navbar.light {
     background-color: #f0f0f0;
     color: #333;
   }

   .navbar.dark {
     background-color: #333;
     color: #f0f0f0;
   }

   .navbar button {
     padding: 0.5em 1em;
     border: none;
     border-radius: 4px;
     cursor: pointer;
     transition: background-color 0.3s ease;
   }

   .navbar.light button {
     background-color: #333;
     color: #f0f0f0;
   }

   .navbar.dark button {
     background-color: #f0f0f0;
     color: #333;
   }

   .navbar button:hover {
     opacity: 0.8;
   }

   /* Content Styles */
   .content {
     padding: 2rem;
     border-radius: 8px;
     transition: background-color 0.3s ease, color 0.3s ease;
   }

   .content.light {
     background-color: #ffffff;
     color: #333;
   }

   .content.dark {
     background-color: #444;
     color: #f0f0f0;
   }
   ```

   **Explanation**:
   - **Transition Effects**: Smooth transitions when toggling themes.
   - **Dynamic Classes**: `.light` and `.dark` classes apply different background and text colors.
   - **Button Styling**: Buttons adapt their styles based on the current theme for consistency.

### Full Project Structure

```
react-context-api-demo/
├── src/
│   ├── components/
│   │   ├── Content.jsx
│   │   └── Navbar.jsx
│   ├── contexts/
│   │   └── ThemeContext.jsx
│   ├── styles.css
│   ├── App.jsx
│   └── index.jsx
├── public/
│   └── index.html
├── package.json
└── vite.config.js
```

**Note**: Ensure that all components and contexts are correctly imported and exported as shown in the examples above.

---

## 4. Advanced Usage (Optional)

### Updating Context Values

While our basic example covers reading and toggling the theme, you can extend the Context API to handle more complex state updates.
!["Context API Structure"](https://github.com/wasiqs-classics/code-camp-react-MERN-Stack/blob/master/CLASS%2008%20%E2%80%93%20State%20Management%20with%20Context%20API/React%20Context%20API-2024-10-05-004156.png)

### Explanation of the Diagram

Let's break down each component and step of the **Auth Context** workflow to understand how authentication state is managed in a React application.

1. **Create AuthContext (`A`)**
   - **Description**: Initialize a new context using `createContext()`.
   - **Purpose**: Establish a context object (`AuthContext`) that will hold authentication-related data, such as user information and authentication status.
   - **Role**: Acts as a centralized store for authentication state that can be accessed by any component within the application.

2. **Create AuthProvider Component (`B`)**
   - **Description**: Develop a provider component (`AuthProvider`) that utilizes the `AuthContext.Provider` to supply context values to its child components.
   - **Purpose**: Manage and provide the authentication state (`isAuthenticated`, `user`) and authentication functions (`login`, `logout`) to the rest of the application.
   - **Role**: Encapsulates the logic for updating authentication state and ensures that any component wrapped by `AuthProvider` can access and modify authentication data.

3. **Wrap Application with AuthProvider (`C`)**
   - **Description**: Enclose the main application (or specific parts of it) with the `AuthProvider` component.
   - **Purpose**: Ensure that all nested components within `AuthProvider` have access to the authentication context.
   - **Role**: Acts as the boundary within which authentication state is accessible, preventing the need for prop drilling.

4. **Nested Components Access AuthContext (`D`)**
   - **Description**: Components like `Navbar` and `Content` that require access to authentication data.
   - **Purpose**: Consume the authentication context to display user information, provide login/logout functionality, and conditionally render content based on authentication status.
   - **Role**: Represents any component within the application that needs to interact with the authentication state.

5. **useContext Hook (`E`)**
   - **Description**: Inside consumer components, utilize the `useContext` hook to access `AuthContext`.
   - **Purpose**: Simplify the process of subscribing to context changes and accessing context data without needing to use the `Consumer` component.
   - **Role**: Facilitates the retrieval of authentication data and functions within functional components.

6. **Components Receive Auth Data (`F`)**
   - **Description**: After invoking `useContext`, components receive the current authentication state and functions.
   - **Purpose**: Enable components to use authentication data (`isAuthenticated`, `user`) and perform actions (`login`, `logout`) based on that data.
   - **Role**: Provides the necessary data and functions for components to render UI conditionally and handle user interactions related to authentication.

7. **Use Auth Data in Components (`I`)**
   - **Description**: Components utilize the received authentication data to render UI elements, such as displaying user information or showing/hiding certain features based on authentication status.
   - **Purpose**: Enhance user experience by dynamically adjusting the UI to reflect the user's authentication state.
   - **Role**: Represents the practical application of authentication data within various components to control visibility, access, and functionality.

8. **Login Function (`J`) & Logout Function (`K`)**
   - **Description**: Functions provided by `AuthContext` to handle user login and logout actions.
   - **Purpose**: Allow components to update the authentication state by logging users in or out, which in turn updates the UI accordingly.
   - **Role**: Facilitates user interaction with the authentication system, enabling state transitions based on user actions.

9. **Update Auth State (`L`)**
   - **Description**: The process of changing the authentication state (`isAuthenticated`, `user`) when users log in or out.
   - **Purpose**: Reflect changes in authentication status across the application, ensuring that all components consuming the context are updated with the latest state.
   - **Role**: Acts as the trigger for re-rendering components based on the new authentication state, maintaining consistency throughout the app.

**Example**: Managing User Authentication

1. **Create AuthContext**:

   ```jsx
   // src/contexts/AuthContext.jsx
   import React, { createContext, useState } from 'react';

   const AuthContext = createContext();

   const AuthProvider = ({ children }) => {
     const [isAuthenticated, setIsAuthenticated] = useState(false);
     const [user, setUser] = useState(null);

     const login = (userData) => {
       setIsAuthenticated(true);
       setUser(userData);
     };

     const logout = () => {
       setIsAuthenticated(false);
       setUser(null);
     };

     return (
       <AuthContext.Provider value={{ isAuthenticated, user, login, logout }}>
         {children}
       </AuthContext.Provider>
     );
   };

   export { AuthContext, AuthProvider };
   ```

2. **Use AuthProvider in App**:

   ```jsx
   // src/App.jsx
   import React from 'react';
   import { AuthProvider } from './contexts/AuthContext';
   import Navbar from './components/Navbar';
   import Content from './components/Content';
   import './styles.css';

   function App() {
     return (
       <AuthProvider>
         <div className="app-container">
           <Navbar />
           <Content />
         </div>
       </AuthProvider>
     );
   }

   export default App;
   ```

3. **Access AuthContext in Components**:

   - **Navbar.jsx**:

     ```jsx
     // src/components/Navbar.jsx
     import React, { useContext } from 'react';
     import { AuthContext } from '../contexts/AuthContext';

     function Navbar() {
       const { isAuthenticated, user, login, logout } = useContext(AuthContext);

       const handleLogin = () => {
         // Simulate a login action
         login({ name: 'Jane Doe', email: 'jane@example.com' });
       };

       const handleLogout = () => {
         logout();
       };

       return (
         <nav className={`navbar ${isAuthenticated ? 'authenticated' : 'unauthenticated'}`}>
           <h1>My Application</h1>
           {isAuthenticated ? (
             <div>
               <span>Welcome, {user.name}</span>
               <button onClick={handleLogout}>Logout</button>
             </div>
           ) : (
             <button onClick={handleLogin}>Login</button>
           )}
         </nav>
       );
     }

     export default Navbar;
     ```

   - **Content.jsx**:

     ```jsx
     // src/components/Content.jsx
     import React, { useContext } from 'react';
     import { AuthContext } from '../contexts/AuthContext';

     function Content() {
       const { isAuthenticated, user } = useContext(AuthContext);

       return (
         <div className={`content ${isAuthenticated ? 'authenticated' : 'unauthenticated'}`}>
           {isAuthenticated ? (
             <div>
               <h2>Dashboard</h2>
               <p>Email: {user.email}</p>
             </div>
           ) : (
             <div>
               <h2>Welcome to the App!</h2>
               <p>Please log in to access your dashboard.</p>
             </div>
           )}
         </div>
       );
     }

     export default Content;
     ```

4. **Update CSS for Authenticated/Unauthenticated States**:

   ```css
   /* src/styles.css */

   /* Existing styles... */

   /* Authenticated Navbar */
   .navbar.authenticated {
     background-color: #28a745;
     color: #fff;
   }

   /* Unauthenticated Navbar */
   .navbar.unauthenticated {
     background-color: #dc3545;
     color: #fff;
   }

   /* Authenticated Content */
   .content.authenticated {
     background-color: #e9ffe9;
     color: #333;
   }

   /* Unauthenticated Content */
   .content.unauthenticated {
     background-color: #ffe9e9;
     color: #333;
   }

   /* Button Styles */
   .navbar button {
     margin-left: 1em;
     padding: 0.5em 1em;
     border: none;
     border-radius: 4px;
     cursor: pointer;
     transition: background-color 0.3s ease;
   }

   .navbar.authenticated button {
     background-color: #ffc107;
     color: #333;
   }

   .navbar.unauthenticated button {
     background-color: #ffffff;
     color: #dc3545;
   }

   .navbar button:hover {
     opacity: 0.8;
   }
   ```

**Explanation**:
- **AuthContext**: Manages authentication state and provides `login` and `logout` functions.
- **Navbar Component**:
  - Displays login button when unauthenticated.
  - Displays user info and logout button when authenticated.
- **Content Component**:
  - Shows different content based on authentication status.
- **CSS**:
  - Styles adjust based on whether the user is authenticated.

### Multiple Contexts

In larger applications, you might need to manage multiple global states (e.g., theme, authentication, notifications). React allows you to use multiple contexts seamlessly.

**Example**:

1. **Create Multiple Contexts**:
   - `ThemeContext.jsx`
   - `AuthContext.jsx`

2. **Wrap App with Multiple Providers**:

   ```jsx
   // src/App.jsx
   import React from 'react';
   import { ThemeProvider } from './contexts/ThemeContext';
   import { AuthProvider } from './contexts/AuthContext';
   import Navbar from './components/Navbar';
   import Content from './components/Content';
   import './styles.css';

   function App() {
     return (
       <ThemeProvider>
         <AuthProvider>
           <div className="app-container">
             <Navbar />
             <Content />
           </div>
         </AuthProvider>
       </ThemeProvider>
     );
   }

   export default App;
   ```

**Note**: The order of providers can matter based on dependency. For instance, if `AuthProvider` depends on `ThemeProvider`, ensure `ThemeProvider` wraps `AuthProvider`.

---

## 5. Best Practices

- **Keep Contexts Focused**: Each context should manage a specific piece of state or functionality to maintain clarity and reusability.
  
  **Good**:
  - `ThemeContext` for theming.
  - `AuthContext` for authentication.
  
  **Bad**:
  - A single `AppContext` managing unrelated states like theme, auth, and notifications.

- **Avoid Overusing Context**: Not every state needs to be in a context. Use context for global or widely shared state.

- **Use Default Values Wisely**: Provide meaningful default values to prevent errors when consuming context.

- **Optimize Performance**:
  - **Memoization**: Use `React.memo` or `useMemo` to prevent unnecessary re-renders.
  - **Selector Functions**: Extract only the needed parts of the context to minimize component updates.

- **Separate Concerns**: Keep context logic separate from component logic to enhance readability and maintainability.

- **Documentation**: Clearly document the purpose and usage of each context to assist team members and future you.

---

## 6. Conclusion

In this class, we've explored the **Context API** in React, a powerful tool for managing global state without the complexities of prop drilling. By creating contexts, providers, and consumers, we can efficiently share data across our component tree, leading to cleaner and more maintainable code.

**Key Takeaways**:

1. **Context Creation**: Initialize contexts using `createContext` and define providers to supply context values.
2. **Consumption**: Access context values within components using the `useContext` hook.
3. **Practical Application**: Implemented a Theme Management system demonstrating how to toggle themes across the app.
4. **Advanced Usage**: Managed user authentication state and handled multiple contexts within the same application.
5. **Best Practices**: Emphasized the importance of focused contexts, performance optimization, and maintainability.

Mastering the Context API empowers you to build more scalable and organized React applications, especially as your projects grow in complexity.

---

## 7. Additional Resources

- **React Official Documentation**:
  - [Context](https://reactjs.org/docs/context.html)
  - [Hooks: useContext](https://reactjs.org/docs/hooks-reference.html#usecontext)

- **Tutorials and Guides**:
  - [Using Context API in React](https://www.digitalocean.com/community/tutorials/react-usecontext-hook)
  - [React Context API: A Complete Guide](https://www.freecodecamp.org/news/react-context-api-guide/)

- **Advanced Topics**:
  - [State Management with Redux vs. Context API](https://redux.js.org/introduction/getting-started)
  - [Optimizing Performance with React Context](https://kentcdodds.com/blog/how-to-optimize-context-value)

- **Community and Support**:
  - [Reactiflux Discord Community](https://www.reactiflux.com/)
  - [Stack Overflow React Tag](https://stackoverflow.com/questions/tagged/reactjs)

Feel free to explore these resources to deepen your understanding of the Context API and state management in React. Happy coding!
