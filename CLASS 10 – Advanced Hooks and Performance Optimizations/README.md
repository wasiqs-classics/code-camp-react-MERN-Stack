## Class 10: Advanced Hooks and Performance Optimization
### Welcome to Class 10!

Congratulations on reaching Class 10! So far, you've built a solid foundation in React, understanding core concepts like **JSX**, **Components**, **Props**, **State Management with Hooks**, and more. In this class, we'll elevate your React skills by exploring **Advanced Hooks** and **Performance Optimization** techniques. These topics will empower you to build more efficient, maintainable, and high-performing React applications.

### Introduction

Up to this point, we already have a slight idea of what React Hooks are. In this tutorial, we will explore advanced hooks and performance optimization techniques in detail.

**Key Objectives:**

- Learn how to manage complex state using `useReducer`.
- Utilize `useContext` for efficient state sharing across components.
- Combine `useReducer` and `useContext` for global state management.
- Understand and implement performance optimization techniques using memoization.
- Apply these concepts through realistic and practical examples.

### 1. useReducer and useContext: Advanced State Management
Managing state in React can become increasingly complex as your application grows. While `useState` is perfect for simple state management, `useReducer` offers a more robust solution for handling complex state logic. When combined with `useContext`, it provides a powerful pattern for managing and sharing global state across your application.

### Understanding `useReducer`

#### What is `useReducer`?

`useReducer` is a Hook that lets you manage state in functional components using a **reducer function**. It's an alternative to `useState` and is particularly useful when the state logic is complex or involves multiple sub-values.

#### Why Use `useReducer`?

- **Complex State Logic**: Ideal for managing state transitions that depend on previous states or involve multiple state variables.
- **Predictable State Updates**: Reducers centralize state changes, making them easier to trace and debug.
- **Separation of Concerns**: Keeps state management logic separate from UI logic.

#### How Does `useReducer` Work?

`useReducer` takes two arguments:

1. **Reducer Function**: Defines how the state changes in response to actions.
2. **Initial State**: The starting value of the state.

It returns an array with two elements:

1. **Current State**: The current value of the state.
2. **Dispatch Function**: A function to send actions to the reducer.

**Syntax:**

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

#### Example Scenario

Imagine building a to-do application where each task can be added, removed, or toggled between completed and incomplete. Managing this with `useState` might involve multiple state variables and complex update logic. `useReducer` can streamline this process by centralizing the state management logic.

### Understanding `useContext`

#### What is `useContext`?

`useContext` is a Hook that allows you to consume context values directly within functional components. It eliminates the need for prop drilling—passing props through multiple layers of components just to reach a deeply nested component.

#### Why Use `useContext`?

- **Simplify Prop Drilling**: Share data across components without manually passing props through every intermediate component.
- **Global State Management**: Ideal for managing global states like user authentication, themes, or language settings.
- **Enhance Readability**: Makes your component hierarchy cleaner by reducing the number of props.

#### How Does `useContext` Work?

1. **Create a Context**: Use `createContext` to initialize a new context.
2. **Provide the Context**: Wrap your component tree with the context provider and supply the necessary value.
3. **Consume the Context**: Use `useContext` within any component to access the context value.

**Syntax:**

```jsx
const value = useContext(MyContext);

### Combining `useReducer` and `useContext`

When you combine `useReducer` with `useContext`, you achieve a powerful global state management system. Here's how they work together:

1. **`useReducer`** manages the state logic.
2. **`useContext`** provides access to the state and dispatch function across the component tree.

#### Example Scenario
- **Scenario**: Imagine you have a to-do application where you need to manage the state of the to-do list and user authentication. Using `useReducer` and `useContext`, you can manage the state efficiently and make it accessible to all components.

**Components Involved:**

- **GlobalStateContext**: The context that holds global state and dispatch function.
- **GlobalStateProvider**: The provider component that wraps the app and supplies the context value.
- **TodoList**: Component to display and add to-dos.
- **LoginToggle**: Component to handle user login and logout.

### Example: Combining useReducer and useContext

1. **Create Context and Reducer**:
   - **GlobalStateContext.js**:
     ```jsx
     import React, { createContext, useReducer } from 'react';

     // Create a context
     const GlobalStateContext = createContext();

     // Define initial state
     const initialState = {
       todos: [],
       isLoggedIn: false,
     };

     // Define reducer function
     const reducer = (state, action) => {
       switch (action.type) {
         case 'ADD_TODO':
           return { ...state, todos: [...state.todos, action.payload] };
         case 'TOGGLE_LOGIN':
           return { ...state, isLoggedIn: !state.isLoggedIn };
         default:
           return state;
       }
     };

     // Create a provider component
     const GlobalStateProvider = ({ children }) => {
       const [state, dispatch] = useReducer(reducer, initialState);

       return (
         <GlobalStateContext.Provider value={{ state, dispatch }}>
           {children}
         </GlobalStateContext.Provider>
       );
     };

     export { GlobalStateContext, GlobalStateProvider };
     ```

2. **Use Provider in App**:
   - **App.js**:
     ```jsx
     import React from 'react';
     import { GlobalStateProvider } from './GlobalStateContext';
     import TodoList from './TodoList';
     import LoginToggle from './LoginToggle';

     function App() {
       return (
         <GlobalStateProvider>
           <TodoList />
           <LoginToggle />
         </GlobalStateProvider>
       );
     }

     export default App;
     ```

3. **Access Context in Components**:
   - **TodoList.js**:
     ```jsx
     import React, { useContext, useState } from 'react';
     import { GlobalStateContext } from './GlobalStateContext';

     function TodoList() {
       const { state, dispatch } = useContext(GlobalStateContext);
       const [newTodo, setNewTodo] = useState('');

       const addTodo = () => {
         dispatch({ type: 'ADD_TODO', payload: newTodo });
         setNewTodo('');
       };

       return (
         <div>
           <h1>Todo List</h1>
           <input
             type="text"
             value={newTodo}
             onChange={(e) => setNewTodo(e.target.value)}
           />
           <button onClick={addTodo}>Add Todo</button>
           <ul>
             {state.todos.map((todo, index) => (
               <li key={index}>{todo}</li>
             ))}
           </ul>
         </div>
       );
     }

     export default TodoList;
     ```

   - **LoginToggle.js**:
     ```jsx
     import React, { useContext } from 'react';
     import { GlobalStateContext } from './GlobalStateContext';

     function LoginToggle() {
       const { state, dispatch } = useContext(GlobalStateContext);

       return (
         <div>
           <h1>{state.isLoggedIn ? 'Logged In' : 'Logged Out'}</h1>
           <button onClick={() => dispatch({ type: 'TOGGLE_LOGIN' })}>
             {state.isLoggedIn ? 'Logout' : 'Login'}
           </button>
         </div>
       );
     }

     export default LoginToggle;
     ```

### 2. Memoization: Performance Optimization

#### What is Memoization?
- **Memoization**: Memoization is an optimization technique used to speed up applications by storing the results of expensive function calls and returning the cached result when the same inputs occur again.

#### React.memo
- **React.memo**: This is a higher-order component that memoizes the result of a component. It prevents unnecessary re-renders by comparing the previous and next props.

**Example**:
```jsx
import React from 'react';

const ExpensiveComponent = React.memo(({ data }) => {
  console.log('ExpensiveComponent rendered');
  return <div>{data}</div>;
});

export default ExpensiveComponent;
```

## 3. Performance Optimization Techniques

As your React application grows, performance can become a concern. Optimizing your application ensures a smooth user experience and efficient resource usage. One effective way to optimize performance in React is through **memoization**. Memoization techniques prevent unnecessary re-renders and expensive calculations, enhancing your app's responsiveness.

### `useMemo` Hook

#### What is `useMemo`?

`useMemo` is a Hook that memoizes the result of an expensive calculation, recomputing it only when its dependencies change. This optimization prevents unnecessary recalculations on every render.

#### Why Use `useMemo`?

- **Optimize Expensive Calculations**: Prevent performance bottlenecks caused by heavy computations.
- **Maintain Referential Equality**: Ensure that objects or arrays are only recreated when necessary, which is crucial for preventing unnecessary re-renders in child components.

#### How Does `useMemo` Work?

`useMemo` takes two arguments:

1. **Create Function**: A function that returns the value you want to memoize.
2. **Dependency Array**: An array of dependencies that determine when to recompute the memoized value.

**Syntax:**

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

#### Example Scenario

Suppose you have a component that performs a complex calculation based on user input. Without `useMemo`, this calculation would run on every render, potentially slowing down the application. By using `useMemo`, you ensure the calculation only runs when the input values change.

**Example**:
```jsx
import React, { useState, useMemo } from 'react';

function ExpensiveCalculationComponent({ a, b }) {
  const result = useMemo(() => {
    console.log('Calculating...');
    return a + b;
  }, [a, b]);

  return <div>Result: {result}</div>;
}

export default ExpensiveCalculationComponent;
```

### `useCallback` Hook

#### What is `useCallback`?

`useCallback` is a Hook that memoizes a function, returning the same function instance between renders unless its dependencies change. It's particularly useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary re-renders.

#### Why Use `useCallback`?

- **Prevent Unnecessary Re-renders**: Ensures that child components receive stable function references.
- **Optimize Performance**: Avoids recreating functions on every render, saving computational resources.

#### How Does `useCallback` Work?

`useCallback` takes two arguments:

1. **Callback Function**: The function you want to memoize.
2. **Dependency Array**: An array of dependencies that determine when to recreate the function.

**Syntax:**

```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

#### Example Scenario

Consider a parent component that passes a callback function to a memoized child component. Without `useCallback`, the function would be recreated on every render, causing the child component to re-render even if its props haven't changed. Using `useCallback` ensures the function reference remains the same unless its dependencies change, preventing unnecessary re-renders.

### Realistic Example Scenario

**Scenario:** Optimizing a List of Expensive Components

Imagine an application that displays a list of items, each rendering complex UI elements or performing expensive computations. As the list grows, performance can degrade due to frequent re-renders. By applying memoization techniques, you can ensure that only the necessary components re-render when their data changes.

**Components Involved:**

- **ExpensiveItem**: A child component that performs heavy computations or renders complex UI.
- **ItemList**: A parent component that renders a list of `ExpensiveItem` components.
- **FilterComponent**: A component that allows users to filter the list.

**Optimization Steps:**

1. **Wrap `ExpensiveItem` with `React.memo`**: Prevents re-rendering unless its props change.
2. **Use `useCallback` for Callback Functions**: Ensures stable function references passed to child components.
3. **Use `useMemo` for Filtered Data**: Memoizes the filtered list to avoid unnecessary calculations.

---

**Example**:
```jsx
import React, { useState, useCallback } from 'react';

function Button({ onClick }) {
  console.log('Button rendered');
  return <button onClick={onClick}>Click me</button>;
}

const MemoizedButton = React.memo(Button);

function ParentComponent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount((prevCount) => prevCount + 1);
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <MemoizedButton onClick={handleClick} />
    </div>
  );
}

export default ParentComponent;
```

### Summary

In **Class 10**, we've explored advanced Hooks and performance optimization techniques in React, equipping you with the tools to build more efficient and maintainable applications.

**Key Learnings:**

1. **Advanced State Management**:
   - **`useReducer`**: Simplifies complex state logic by centralizing state transitions.
   - **`useContext`**: Facilitates global state sharing across components without prop drilling.
   - **Combined Usage**: Leveraging both Hooks to manage and distribute global state effectively.

2. **Performance Optimization**:
   - **`React.memo`**: Prevents unnecessary re-renders of functional components.
   - **`useMemo`**: Memoizes expensive calculations, recomputing only when dependencies change.
   - **`useCallback`**: Memoizes functions to maintain stable references, optimizing child component renders.

3. **Best Practices**:
   - Adhering to the Rules of Hooks.
   - Proper naming and organization of custom Hooks.
   - Efficient dependency management in Hooks.
   - Strategic use of memoization to enhance performance.

By mastering these advanced Hooks and optimization techniques, you're well on your way to building high-performing React applications that are both scalable and maintainable. Continue practicing these concepts, integrate them into your projects, and explore further to deepen your React expertise.

---

By the end of this class, you should have a solid understanding of how to manage complex state and optimize performance in React applications using advanced hooks and memoization techniques.
