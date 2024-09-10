## Class 4: Event Handling and Conditional Rendering

### 1. Event Handling

#### What is Event Handling?
- **Event Handling** in React refers to the process of capturing and responding to user actions such as clicks, form submissions, and keyboard inputs.
- React uses a synthetic event system that ensures events behave consistently across different browsers.

#### Basic Principles of Event Handling
- **Synthetic Events**: React wraps native browser events in a synthetic event object, providing a consistent API.
- **Naming Conventions**: Event handlers in React are named using camelCase (e.g., `onClick`, `onChange`).
- **Event Handlers**: Functions that are called when an event occurs. They are typically defined within the component.

#### Example: Handling a Button Click
1. Create a simple button that shows an alert when clicked:

```jsx
import React from 'react';

function ClickButton() {
  const handleClick = () => {
    alert('Button clicked!');
  };

  return <button onClick={handleClick}>Click Me</button>;
}

export default ClickButton;
```

### 2. Conditional Rendering

#### What is Conditional Rendering?
- **Conditional Rendering** in React allows you to render different components or elements based on certain conditions.
- It works similarly to conditional statements in JavaScript.

#### Methods of Conditional Rendering
- **if/else Statements**: Use standard if/else statements to conditionally render components.
- **Ternary Operator**: A concise way to render components based on a condition.
- **Logical && Operator**: Render a component only if a condition is true.

#### Example: Conditional Rendering with Ternary Operator
1. Create a component that displays a message based on a condition:

```jsx
import React, { useState } from 'react';

function Greeting() {
  const [isLoggedIn, setIsLoggedIn] = useState(false);

  return (
    <div>
      {isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please sign in.</h1>}
      <button onClick={() => setIsLoggedIn(!isLoggedIn)}>
        {isLoggedIn ? 'Logout' : 'Login'}
      </button>
    </div>
  );
}

export default Greeting;
```

### 3. Applying Concepts to the To-Do App

#### Adding Event Handling to the To-Do App
1. Update the `TodoList` component to handle form submissions and input changes:

```jsx
import React, { useState } from 'react';

function TodoList() {
  const [todos, setTodos] = useState(['Learn React', 'Build a To-Do App']);
  const [newTodo, setNewTodo] = useState('');

  const handleInputChange = (e) => {
    setNewTodo(e.target.value);
  };

  const handleAddTodo = (e) => {
    e.preventDefault();
    if (newTodo.trim()) {
      setTodos([...todos, newTodo]);
      setNewTodo('');
    }
  };

  return (
    <div>
      <h2>To-Do List</h2>
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>{todo}</li>
        ))}
      </ul>
      <form onSubmit={handleAddTodo}>
        <input
          type="text"
          value={newTodo}
          onChange={handleInputChange}
        />
        <button type="submit">Add</button>
      </form>
    </div>
  );
}

export default TodoList;
```

#### Adding Conditional Rendering to the To-Do App
1. Update the `TodoList` component to conditionally render a message when there are no to-do items:

```jsx
import React, { useState } from 'react';

function TodoList() {
  const [todos, setTodos] = useState([]);
  const [newTodo, setNewTodo] = useState('');

  const handleInputChange = (e) => {
    setNewTodo(e.target.value);
  };

  const handleAddTodo = (e) => {
    e.preventDefault();
    if (newTodo.trim()) {
      setTodos([...todos, newTodo]);
      setNewTodo('');
    }
  };

  return (
    <div>
      <h2>To-Do List</h2>
      {todos.length === 0 ? (
        <p>No to-do items. Add a new task!</p>
      ) : (
        <ul>
          {todos.map((todo, index) => (
            <li key={index}>{todo}</li>
          ))}
        </ul>
      )}
      <form onSubmit={handleAddTodo}>
        <input
          type="text"
          value={newTodo}
          onChange={handleInputChange}
        />
        <button type="submit">Add</button>
      </form>
    </div>
  );
}

export default TodoList;
```

### 4. Using the Updated TodoList in App

- Import and use the updated `TodoList` component in your `App.jsx`:

```jsx
import React from 'react';
import TodoList from './TodoList';

function App() {
  return (
    <div>
      <TodoList />
    </div>
  );
}

export default App;
```

---
