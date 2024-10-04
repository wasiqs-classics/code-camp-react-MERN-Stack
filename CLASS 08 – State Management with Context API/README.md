## Class 8: State Management with Context API

### 1. Introduction to Context API

#### What is Context API?
- **Context API**: The Context API in React provides a way to share values between components without having to pass props through every level of the component tree. This is particularly useful for managing global state, such as user authentication, themes, or language preferences.

#### Why Use Context API?
- **Prop Drilling**: In React, data is typically passed from parent to child components via props. However, this can become cumbersome when you need to pass data through many levels of components, a problem known as "prop drilling."

#### Example Scenario of Prop Drilling
- **Scenario**: Imagine you have a React application with a `Grandparent` component that needs to pass data to a `Grandchild` component. The `Grandparent` component passes the data to the `Parent` component, which then passes it to the `Grandchild` component. This can become cumbersome and difficult to manage, especially as the component tree grows larger and deeper.

### 2. Setting Up Context API

#### Creating a Context
1. **Create a Context**:
   - Use the `createContext` function to create a new context.
     ```jsx
     import React, { createContext } from 'react';

     const MyContext = createContext();
     ```

#### Provider and Consumer
- **Provider**: The Provider component is used to wrap the part of your application that needs access to the context. It provides the context value to its children.
- **Consumer**: The Consumer component is used to access the context value within a component.

#### Example of Provider and Consumer
- **Scenario**: Let's say we have a `UserContext` that provides user information to various components in the application.

1. **Create UserContext**:
   ```jsx
   import React, { createContext, useState } from 'react';

   const UserContext = createContext();

   const UserProvider = ({ children }) => {
     const [user, setUser] = useState({ name: 'John Doe', email: 'john@example.com' });

     return (
       <UserContext.Provider value={user}>
         {children}
       </UserContext.Provider>
     );
   };

   export { UserContext, UserProvider };
   ```

2. **Use UserProvider in App**:
   ```jsx
   import React from 'react';
   import { UserProvider } from './UserContext';
   import UserProfile from './UserProfile';

   function App() {
     return (
       <UserProvider>
         <UserProfile />
       </UserProvider>
     );
   }

   export default App;
   ```

3. **Access UserContext in UserProfile**:
   ```jsx
   import React, { useContext } from 'react';
   import { UserContext } from './UserContext';

   function UserProfile() {
     const user = useContext(UserContext);

     return (
       <div>
         <h1>User Profile</h1>
         <p>Name: {user.name}</p>
         <p>Email: {user.email}</p>
       </div>
     );
   }

   export default UserProfile;
   ```

### 3. Practical Example: Theme Management

Let's create a simple example to manage a light and dark theme using Context API.

1. **Create Context and Provider**:
   - **ThemeContext.js**:
     ```jsx
     import React, { createContext, useState } from 'react';

     // Create a new context
     const ThemeContext = createContext();

     // Create a provider component
     const ThemeProvider = ({ children }) => {
       // State to manage the current theme
       const [theme, setTheme] = useState('light');

       // Function to toggle between light and dark themes
       const toggleTheme = () => {
         setTheme((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));
       };

       return (
         // Provide the current theme and toggle function to children
         <ThemeContext.Provider value={{ theme, toggleTheme }}>
           {children}
         </ThemeContext.Provider>
       );
     };

     export { ThemeContext, ThemeProvider };
     ```

2. **Use Provider in App**:
   - **App.js**:
     ```jsx
     import React from 'react';
     import { ThemeProvider } from './ThemeContext';
     import Navbar from './Navbar';
     import Content from './Content';

     function App() {
       return (
         <ThemeProvider>
           <Navbar />
           <Content />
         </ThemeProvider>
       );
     }

     export default App;
     ```

3. **Access Context in Components**:
   - **Navbar.js**:
     ```jsx
     import React, { useContext } from 'react';
     import { ThemeContext } from './ThemeContext';

     function Navbar() {
       const { theme, toggleTheme } = useContext(ThemeContext);

       return (
         <nav className={`navbar ${theme}`}>
           <h1>My App</h1>
           <button onClick={toggleTheme}>
             Switch to {theme === 'light' ? 'dark' : 'light'} mode
           </button>
         </nav>
       );
     }

     export default Navbar;
     ```

   - **Content.js**:
     ```jsx
     import React, { useContext } from 'react';
     import { ThemeContext } from './ThemeContext';

     function Content() {
       const { theme } = useContext(ThemeContext);

       return (
         <div className={`content ${theme}`}>
           <h2>Welcome to my app!</h2>
           <p>This is a simple example of using Context API for theme management.</p>
         </div>
       );
     }

     export default Content;
     ```

4. **CSS Styles**:
   - **styles.css**:
     ```css
     .navbar {
       padding: 1rem;
       display: flex;
       justify-content: space-between;
       align-items: center;
     }

     .navbar.light {
       background-color: #f0f0f0;
       color: #000;
     }

     .navbar.dark {
       background-color: #333;
       color: #fff;
     }

     .content {
       padding: 2rem;
     }

     .content.light {
       background-color: #fff;
       color: #000;
     }

     .content.dark {
       background-color: #000;
       color: #fff;
     }
     ```

5. **Import CSS in App**:
   - **App.js**:
     ```jsx
     import React from 'react';
     import { ThemeProvider } from './ThemeContext';
     import Navbar from './Navbar';
     import Content from './Content';
     import './styles.css'; // Import the CSS file

     function App() {
       return (
         <ThemeProvider>
           <Navbar />
           <Content />
         </ThemeProvider>
       );
     }

     export default App;
     ```

### 4. Summary

In this class, we covered the basics of the Context API in React. We started with an introduction to the Context API and its benefits, particularly in avoiding prop drilling and managing global state. We then set up a simple example to manage a light and dark theme using Context API, including creating a context, setting up a provider, and accessing the context in components. We also implemented different CSS styles based on the theme selection to make the example more practical and understandable.

By the end of this class, you should have a good understanding of how to use the Context API for state management in React applications. This knowledge will help you manage global state more efficiently and avoid the pitfalls of prop drilling.
