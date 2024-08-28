# Class 3: Props and State

## 1. Props (Properties)

### What are Props?

- **Props** (short for "properties") are a mechanism in React that allows you to pass data from a parent component to its child components. Think of props as the "inputs" to a component, similar to how you might pass arguments to a function in JavaScript.
- **Analogy**: Imagine a prop as a gift that a parent gives to their child. The child can use the gift (prop) but cannot change it. Similarly, props are read-only—once the parent provides a prop to a child component, the child can use it but cannot modify it.

### Using Props

#### Passing Props:

- In a **functional component**, props are passed as an argument to the function:
  
  ```jsx
  function Greeting(props) {
    return <h1>Hello, {props.name}!</h1>;
  }
  ```

- In a **class component**, props are accessed using `this.props`:

  ```jsx
  class Greeting extends React.Component {
    render() {
      return <h1>Hello, {this.props.name}!</h1>;
    }
  }
  ```

### Example: Using Props

Let’s create a simple example to understand how props work.

#### 1. Create a new file called `Greeting.jsx`:

```jsx
// Greeting.jsx
import React from 'react';

function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

export default Greeting;
```

#### 2. Use the `Greeting` component in your `App.jsx`:

```jsx
// App.jsx
import React from 'react';
import Greeting from './Greeting';

function App() {
  return (
    <div>
      <Greeting name="John" />
      <Greeting name="Alice" />
    </div>
  );
}

export default App;
```

In this example, the `Greeting` component receives a `name` prop from the parent `App` component and uses it to display a personalized greeting.

## 2. State

### What is State?

- **State** represents the data that a component maintains and manages internally. Unlike props, state is mutable, meaning it can change over time based on user interactions or other factors.
- **Analogy**: Think of state as the memory of a component. Just like how you might keep track of something in your memory, a component uses state to remember things that can change, like a user’s input or whether a checkbox is checked.

### State and the Component Lifecycle

- **State** is closely tied to the lifecycle of a component in React. As the state changes, the component re-renders to reflect the updated state, ensuring that the UI is always in sync with the current state of the data.
- **Component Lifecycle**: When a component is first created, React calls the constructor, where you can initialize the state. As the component renders and updates, React handles the state changes and ensures the UI updates accordingly.

### Managing State in Functional Components

- In modern React, you can manage state in **functional components** using the `useState` hook:

  ```jsx
  import React, { useState } from 'react';

  function Counter() {
    const [count, setCount] = useState(0);

    return (
      <div>
        <p>You clicked {count} times</p>
        <button onClick={() => setCount(count + 1)}>
          Click me
        </button>
      </div>
    );
  }
  ```

  In this example, `count` is a piece of state, and `setCount` is the function used to update that state. When the button is clicked, the state changes, and the component re-renders with the updated count.

### Managing State in Class Components

- In **class components**, state is typically initialized in the constructor and updated using `this.setState`:

  ```jsx
  class Counter extends React.Component {
    constructor(props) {
      super(props);
      this.state = { count: 0 };
    }

    render() {
      return (
        <div>
          <p>You clicked {this.state.count} times</p>
          <button onClick={() => this.setState({ count: this.state.count + 1 })}>
            Click me
          </button>
        </div>
      );
    }
  }
  ```

  Here, `this.state.count` holds the current count, and `this.setState` updates the count, triggering a re-render.

## 3. Enhancing the To-Do App with Props and State

### Using State to Manage the To-Do List

In our To-Do app, we'll use state to manage the list of tasks. We’ll also introduce the concept of adding new tasks to the list dynamically.

#### 1. Create a new component called `TodoList.jsx`:

```jsx
// TodoList.jsx
import React, { useState } from 'react';

function TodoList() {
  const [todos, setTodos] = useState(['Learn React', 'Build a To-Do App']);
  const [newTodo, setNewTodo] = useState('');

  const addTodo = () => {
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
      <input
        type="text"
        value={newTodo}
        onChange={(e) => setNewTodo(e.target.value)}
      />
      <button onClick={addTodo}>Add</button>
    </div>
  );
}

export default TodoList;
```

### Connecting Props and State in Our To-Do App

We can enhance our `TodoList` component by passing some initial tasks as props from the parent component.

#### 2. Modify the `TodoList` component to accept initial tasks as props:

```jsx
// TodoList.jsx
import React, { useState, useEffect } from 'react';

function TodoList({ initialTodos }) {
  const [todos, setTodos] = useState(initialTodos);
  const [newTodo, setNewTodo] = useState('');

  const addTodo = () => {
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
      <input
        type="text"
        value={newTodo}
        onChange={(e) => setNewTodo(e.target.value)}
      />
      <button onClick={addTodo}>Add</button>
    </div>
  );
}

export default TodoList;
```

#### 3. Pass the initial tasks as props from `App.jsx`:

```jsx
// App.jsx
import React from 'react';
import TodoList from './TodoList';

function App() {
  return (
    <div>
      <TodoList initialTodos={['Learn React', 'Build a To-Do App']} />
    </div>
  );
}

export default App;
```

Now, the `TodoList` component starts with an initial set of tasks passed from the parent `App` component via props. The component manages its own state internally and allows users to add new tasks to the list.

### State and Component Lifecycle Example

If we want to fetch the initial list of to-dos from an API or local storage when the component mounts, we can use the `useEffect` hook:

```jsx
import React, { useState, useEffect } from 'react';

function TodoList() {
  const [todos, setTodos] = useState([]);
  const [newTodo, setNewTodo] = useState('');

  useEffect(() => {
    // Simulate fetching data
    const savedTodos = ['Learn React', 'Build a To-Do App'];
    setTodos(savedTodos);
  }, []);

  const addTodo = () => {
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
      <input
        type="text"
        value={newTodo}
        onChange={(e) => setNewTodo(e.target.value)}
      />
      <button onClick={addTodo}>Add</button>
    </div>
  );
}

export default TodoList;
```

In this example, the `useEffect` hook mimics the component lifecycle method `componentDidMount` in class components. It runs when the component mounts and is used here to initialize the `todos` state with some data.

This completes Class 3, where we've covered the concepts of props and state, explained how they work with analogies and examples, and implemented them in our ongoing To-Do app project.
This markdown code contains the expanded content with added explanations, analogies, and code examples, making the concepts of props and state more accessible to beginners. The To-Do app example has also been enhanced with the use of props and state management.
