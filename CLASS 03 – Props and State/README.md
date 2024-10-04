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
  We can also use other ways to define functions here. Like:

  Here's how you can convert that line of code to use a normal function instead of an arrow function:

```jsx
<button onClick={function() { setCount(count + 1); }}>
```

Or, if you want to define the function separately:

```jsx
function incrementCount() {
  setCount(count + 1);
}

<button onClick={incrementCount}>
```

Both of these approaches achieve the same result, with the second example providing a named function that can be reused or tested separately if needed.

Great question! When you define a state in React using the `useState` hook, like this:

```javascript
const [count, setCount] = useState(0);
```

The `useState` hook returns an array with exactly two elements:

1. The first element (`count` in this case) is the current state value.
2. The second element (`setCount` in this case) is a function that you can use to update the state.

### How React Knows `setCount` is a Function

This behavior is built into the `useState` hook provided by React. Here’s how it works:

- **React's `useState` Function**: When you call `useState(initialValue)`, React internally creates a piece of state for your component. It keeps track of this state and ensures it persists across re-renders.
  
- **Returning an Array**: `useState` returns an array with two items:
  1. The first item is the current state value.
  2. The second item is a setter function that you can call to update the state.

Because the second item in the array is a function, React knows that when you call `setCount(newValue)`, it should update the `count` state to `newValue` and trigger a re-render of the component to reflect the updated state.

### Example Breakdown

In the example:

```javascript
const [count, setCount] = useState(0);
```

- `useState(0)` creates a state variable with an initial value of `0`.
- React returns an array where:
  - `count` is `0` (the initial value of the state).
  - `setCount` is a function provided by React that you can use to update the value of `count`.

When you later call `setCount(newValue)`, React:
1. Updates the `count` state to `newValue`.
2. Re-renders the component with the updated state.

This is how React abstracts state management and provides you with the tools (`setCount`, in this case) to update the state in a predictable way.

### Managing State in Class Components (Optional)

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


### Summary

This `TodoList` component is a simple example of how to use `useState` to manage the state in a functional React component. It allows a user to add new tasks to a to-do list, with each task being displayed as a list item. The `newTodo` state keeps track of what the user types in the input field, and the `todos` state keeps track of the list of tasks. When the user clicks "Add," the new task is added to the list and the input field is cleared.
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

This completes Class 3, where we've covered the concepts of props and state, explained how they work with analogies and examples.
This markdown code contains the expanded content with added explanations, analogies, and code examples, making the concepts of props and state more accessible to beginners. 
