# Class 2: JSX and Components

## 1. Introduction to JSX

### What is JSX?

- **JSX (JavaScript XML)** is a syntax extension for JavaScript that allows you to write HTML directly within JavaScript. It makes it easier to create and visualize the structure of your UI components.
- **Why Use JSX?**: JSX simplifies the process of writing React components by allowing you to write HTML-like syntax within JavaScript. It also helps in making the code more readable and maintainable.

### Basic JSX Syntax

- JSX looks similar to HTML but is written within JavaScript. Here’s a simple example:

    ```jsx
    const element = <h1>Hello, world!</h1>;
    ```

- This JSX code is transformed into a `React.createElement` call by Babel, a JavaScript compiler, making it understandable by the browser.

### Embedding Expressions in JSX

- You can embed any JavaScript expression in JSX by wrapping it in curly braces `{}`.

    ```jsx
    const name = 'Wasiq';
    const element = <h1>Hello, {name}!</h1>;
    ```

### JSX Example

- Here’s a simple example of using JSX in a React component:

    ```jsx
    import React from 'react';
    import ReactDOM from 'react-dom';

    const name = 'John';
    const element = <h1>Hello, {name}!</h1>;

    ReactDOM.render(element, document.getElementById('root'));
    ```

## 2. Introduction to Components
!["Components in REACT"](https://info340.github.io/img/react/uimockscript.png)
Image Courtesy: [Learn React GitHub](https://info340.github.io/react.html)
### What are Components?

- **Components** are the building blocks of a React application. They are reusable pieces of UI that can manage their own state and logic.
- **Types of Components**: There are two main types of components in React:
    - **Functional Components**: These are simple JavaScript functions that return JSX.
    - **Class Components**: These are ES6 classes that extend `React.Component` and have a `render` method that returns JSX.

### Creating Functional Components

- Functional components are the simplest way to create a component in React. Here’s an example:

    ```jsx
    function Welcome(props) {
      return <h1>Hello, {props.name}</h1>;
    }
    ```

- You can use this component in your JSX like this:

    ```jsx
    const element = <Welcome name="John" />;
    ```

### Creating Class Components

- Class components are more feature-rich and can manage their own state. Here’s an example:

    ```jsx
    class Welcome extends React.Component {
      render() {
        return <h1>Hello, {this.props.name}</h1>;
      }
    }
    ```

- You can use this component in your JSX like this:

    ```jsx
    const element = <Welcome name="John" />;
    ```

## 3. Setting Up the To-Do App Project

### Project Setup with Vite

- Let’s set up our project using Vite. Open your terminal and run:

    ```bash
    npm create vite@latest todo-app
    cd todo-app
    npm install
    npm run dev
    ```

- This will create a new React project using Vite and start the development server.

### Project Structure

- The basic structure of your Vite project will look like this:

    ```plaintext
    todo-app/
    ├── index.html
    ├── package.json
    ├── src/
    │   ├── App.jsx
    │   ├── main.jsx
    │   └── components/
    └── vite.config.js
    ```

### Creating the To-Do App Component

- Let’s start by creating a simple component for our To-Do app. Create a new file `TodoApp.jsx` in the `src` directory:

    ```jsx
    import React from 'react';

    function TodoApp() {
      return (
        <div>
          <h1>To-Do List</h1>
          <ul>
            <li>Learn React</li>
            <li>Build a To-Do App</li>
          </ul>
        </div>
      );
    }

    export default TodoApp;
    ```

### Using the To-Do App Component

- Now, let’s use this component in our `App.jsx` file:

    ```jsx
    import React from 'react';
    import TodoApp from './TodoApp';

    function App() {
      return (
        <div>
          <TodoApp />
        </div>
      );
    }

    export default App;
    ```

### Rendering the App

- Finally, ensure your `main.jsx` file renders the `App` component:

    ```jsx
    import React from 'react';
    import ReactDOM from 'react-dom';
    import App from './App';

    ReactDOM.render(<App />, document.getElementById('root'));
    ```

This should give you a solid start for Class 2, covering the basics of JSX and components, along with setting up the initial structure for your To-Do app.
