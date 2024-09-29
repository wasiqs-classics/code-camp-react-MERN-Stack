# Building a Simple React Todo List App: A Step-by-Step Tutorial

Welcome to this **React Todo List App** tutorial! In this guide, we'll build a basic single-page application (SPA) that allows users to add, mark as complete, and delete tasks. We'll use **Vite** for project setup and focus solely on the React concepts you've learned so far:

1. **JSX**
2. **Components**
3. **Props**
4. **Event Handling and Conditional Rendering**
5. **List & Keys**
6. **Forms and Controlled Components**

By the end of this tutorial, you'll have a functional Todo List app with a gradient background, a heading, instructions, and interactive task management features.

---

## Table of Contents

1. [Project Setup with Vite](#project-setup-with-vite)
2. [Adding Global Styling](#adding-global-styling)
3. [Creating the App Structure](#creating-the-app-structure)
4. [Building Components](#building-components)
   - [1. Header Component](#1-header-component)
   - [2. TodoForm Component](#2-todoform-component)
   - [3. TodoList Component](#3-todolist-component)
   - [4. TodoItem Component](#4-todoitem-component)
5. [Managing State in App Component](#managing-state-in-app-component)
6. [Implementing Event Handlers](#implementing-event-handlers)
7. [Adding Conditional Rendering](#adding-conditional-rendering)
8. [Styling Components](#styling-components)
9. [Final Touches](#final-touches)
10. [Conclusion](#conclusion)

---

## Project Setup with Vite

### Step 1: Install Vite and Create a New React Project

Vite is a fast build tool that provides a better development experience for modern web projects. Let's set up a new React project using Vite.

1. **Open your terminal** and navigate to the directory where you want to create your project.

2. **Run the following command** to create a new Vite React project:

   ```bash
   npm create vite@latest react-todo-app -- --template react
   ```

   - `react-todo-app` is the name of your project folder.
   - `--template react` specifies that you want to use the React template.

3. **Navigate into your project directory**:

   ```bash
   cd react-todo-app
   ```

4. **Install dependencies**:

   ```bash
   npm install
   ```

5. **Start the development server**:

   ```bash
   npm run dev
   ```

   This command will start the development server and provide you with a local URL (e.g., `http://localhost:5173`) where you can view your app.

### Step 2: Verify the Setup

Open your browser and navigate to the URL provided by Vite. You should see the default Vite React welcome page. We'll be replacing this with our Todo List app.

---

## Adding Global Styling

We'll add a global CSS file to style our application, including a gradient background.

### Step 1: Create a Global CSS File

1. **Navigate to the `src` folder** in your project directory.

2. **Create a new file** named `index.css`.

3. **Add the following CSS** to `index.css`:

   ```css
   /* src/index.css */

   /* Apply a gradient background to the entire app */
   body {
     margin: 0;
     padding: 0;
     box-sizing: border-box;
     font-family: Arial, sans-serif;
     background: linear-gradient(135deg, #f06, #4a90e2);
     min-height: 100vh;
     display: flex;
     justify-content: center;
     align-items: center;
   }

   /* Container for the app */
   .container {
     background-color: rgba(255, 255, 255, 0.9);
     padding: 2rem;
     border-radius: 8px;
     box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
     width: 90%;
     max-width: 500px;
   }

   /* Heading styles */
   h1 {
     text-align: center;
     color: #333;
   }

   /* Instruction paragraph */
   p {
     text-align: center;
     color: #666;
   }

   /* Success message */
   .success {
     color: green;
     text-align: center;
     margin-bottom: 1em;
     font-weight: bold;
   }

   /* Error message */
   .error {
     color: red;
     font-size: 0.8em;
     margin-top: 0.5em;
     display: block;
   }
   ```

### Step 2: Import Global CSS in `main.jsx`

1. **Open `main.jsx`** in the `src` folder.

2. **Import the global CSS** by adding the following line at the top:

   ```jsx
   import './index.css';
   ```

   Your `main.jsx` should look like this:

   ```jsx
   import React from 'react';
   import ReactDOM from 'react-dom/client';
   import App from './App';
   import './index.css';

   ReactDOM.createRoot(document.getElementById('root')).render(
     <React.StrictMode>
       <App />
     </React.StrictMode>
   );
   ```

---

## Creating the App Structure

We'll structure our app by creating a container that holds all components: a header, instructions, the form to add tasks, and the list of tasks.

### Step 1: Update `App.jsx`

1. **Open `App.jsx`** in the `src` folder.

2. **Replace its content** with the following:

   ```jsx
   // src/App.jsx
   import React from 'react';
   import Header from './components/Header';
   import TodoForm from './components/TodoForm';
   import TodoList from './components/TodoList';

   function App() {
     return (
       <div className="container">
         <Header />
         <TodoForm />
         <TodoList />
       </div>
     );
   }

   export default App;
   ```

   **Explanation**:

   - **Header Component**: Displays the main heading and instructions.
   - **TodoForm Component**: Contains the input field and button to add tasks.
   - **TodoList Component**: Displays the list of tasks.

### Step 2: Create the Components Folder

1. **Within the `src` folder**, create a new folder named `components`.

2. **Inside `components`**, we'll create individual component files: `Header.jsx`, `TodoForm.jsx`, and `TodoList.jsx`.

---

## Building Components

We'll create each component step-by-step, ensuring they work together seamlessly.

### 1. Header Component

The `Header` component will display the main heading and instructions.

#### Step 1: Create `Header.jsx`

1. **In the `components` folder**, create a new file named `Header.jsx`.

2. **Add the following code** to `Header.jsx`:

   ```jsx
   // src/components/Header.jsx
   import React from 'react';

   function Header() {
     return (
       <div>
         <h1>My Todo List</h1>
         <p>Manage your tasks efficiently by adding, completing, or deleting them below.</p>
       </div>
     );
   }

   export default Header;
   ```

   **Explanation**:

   - **JSX**: Returns a `div` containing an `h1` for the title and a `p` for instructions.

### 2. TodoForm Component

The `TodoForm` component allows users to input new tasks and add them to the list.

#### Step 1: Create `TodoForm.jsx`

1. **In the `components` folder**, create a new file named `TodoForm.jsx`.

2. **Add the following code** to `TodoForm.jsx`:

   ```jsx
   // src/components/TodoForm.jsx
   import React, { useState } from 'react';

   function TodoForm({ addTask }) {
     const [task, setTask] = useState('');

     const handleChange = (e) => {
       setTask(e.target.value);
     };

     const handleSubmit = (e) => {
       e.preventDefault();
       if (task.trim()) {
         addTask(task);
         setTask('');
       }
     };

     return (
       <form onSubmit={handleSubmit}>
         <input
           type="text"
           placeholder="Enter a new task"
           value={task}
           onChange={handleChange}
         />
         <button type="submit">Add Task</button>
       </form>
     );
   }

   export default TodoForm;
   ```

   **Explanation**:

   - **useState Hook**: Manages the state of the input field (`task`).
   - **handleChange Function**: Updates the `task` state as the user types.
   - **handleSubmit Function**: Prevents the default form submission, checks if the input is not empty, calls the `addTask` function passed via props, and clears the input field.
   - **Props**: Receives `addTask` from the parent component (`App`) to add a new task to the list.

### 3. TodoList Component

The `TodoList` component displays all the tasks added by the user.

#### Step 1: Create `TodoList.jsx`

1. **In the `components` folder**, create a new file named `TodoList.jsx`.

2. **Add the following code** to `TodoList.jsx`:

   ```jsx
   // src/components/TodoList.jsx
   import React from 'react';
   import TodoItem from './TodoItem';

   function TodoList({ tasks, deleteTask, toggleComplete }) {
     return (
       <ul>
         {tasks.map((task) => (
           <TodoItem
             key={task.id}
             task={task}
             deleteTask={deleteTask}
             toggleComplete={toggleComplete}
           />
         ))}
       </ul>
     );
   }

   export default TodoList;
   ```

   **Explanation**:

   - **Props**:
     - `tasks`: An array of task objects.
     - `deleteTask`: Function to delete a task.
     - `toggleComplete`: Function to mark a task as complete.
   - **List Rendering**: Uses the `map` method to render each task as a `TodoItem` component.
   - **Keys**: Each `TodoItem` is given a unique `key` prop (`task.id`) to help React identify each element uniquely.

### 4. TodoItem Component

The `TodoItem` component represents an individual task with options to mark as complete or delete it.

#### Step 1: Create `TodoItem.jsx`

1. **In the `components` folder**, create a new file named `TodoItem.jsx`.

2. **Add the following code** to `TodoItem.jsx`:

   ```jsx
   // src/components/TodoItem.jsx
   import React, { useState } from 'react';

   function TodoItem({ task, deleteTask, toggleComplete }) {
     const [isHovered, setIsHovered] = useState(false);

     const handleMouseEnter = () => {
       setIsHovered(true);
     };

     const handleMouseLeave = () => {
       setIsHovered(false);
     };

     return (
       <li
         onMouseEnter={handleMouseEnter}
         onMouseLeave={handleMouseLeave}
         style={{
           display: 'flex',
           justifyContent: 'space-between',
           alignItems: 'center',
           padding: '0.5em',
           borderBottom: '1px solid #ccc',
           textDecoration: task.completed ? 'line-through' : 'none',
         }}
       >
         <span>{task.text}</span>
         {isHovered && (
           <div>
             <button
               onClick={() => toggleComplete(task.id)}
               style={{
                 marginRight: '0.5em',
                 backgroundColor: task.completed ? '#ffc107' : '#28a745',
                 color: '#fff',
                 border: 'none',
                 padding: '0.3em 0.6em',
                 borderRadius: '4px',
                 cursor: 'pointer',
               }}
             >
               {task.completed ? 'Undo' : 'Complete'}
             </button>
             <button
               onClick={() => deleteTask(task.id)}
               style={{
                 backgroundColor: '#dc3545',
                 color: '#fff',
                 border: 'none',
                 padding: '0.3em 0.6em',
                 borderRadius: '4px',
                 cursor: 'pointer',
               }}
             >
               Delete
             </button>
           </div>
         )}
       </li>
     );
   }

   export default TodoItem;
   ```

   **Explanation**:

   - **Props**:
     - `task`: The task object containing `id`, `text`, and `completed` status.
     - `deleteTask`: Function to delete the task.
     - `toggleComplete`: Function to toggle the task's completion status.
   - **State (`isHovered`)**: Manages whether the mouse is hovering over the task item.
   - **Event Handlers**:
     - `handleMouseEnter`: Sets `isHovered` to `true` when the mouse enters the list item.
     - `handleMouseLeave`: Sets `isHovered` to `false` when the mouse leaves the list item.
   - **Conditional Rendering**: Displays the "Complete/Undo" and "Delete" buttons only when `isHovered` is `true`.
   - **Styling**: Inline styles are used for simplicity, including strikethrough for completed tasks and button colors.

---

## Managing State in App Component

The `App` component will manage the state of all tasks and provide necessary functions to add, delete, and toggle tasks.

### Step 1: Update `App.jsx`

1. **Open `App.jsx`** in the `src` folder.

2. **Modify the code** to include state management and pass necessary props to child components:

   ```jsx
   // src/App.jsx
   import React, { useState } from 'react';
   import Header from './components/Header';
   import TodoForm from './components/TodoForm';
   import TodoList from './components/TodoList';

   function App() {
     const [tasks, setTasks] = useState([]);

     const addTask = (taskText) => {
       const newTask = {
         id: Date.now(),
         text: taskText,
         completed: false,
       };
       setTasks([...tasks, newTask]);
     };

     const deleteTask = (id) => {
       setTasks(tasks.filter((task) => task.id !== id));
     };

     const toggleComplete = (id) => {
       setTasks(
         tasks.map((task) =>
           task.id === id ? { ...task, completed: !task.completed } : task
         )
       );
     };

     return (
       <div className="container">
         <Header />
         <TodoForm addTask={addTask} />
         <TodoList tasks={tasks} deleteTask={deleteTask} toggleComplete={toggleComplete} />
       </div>
     );
   }

   export default App;
   ```

   **Explanation**:

   - **useState Hook**: Initializes the `tasks` state as an empty array.
   - **addTask Function**:
     - Creates a new task object with a unique `id`, the task `text`, and a `completed` status set to `false`.
     - Updates the `tasks` state by adding the new task to the existing array.
   - **deleteTask Function**:
     - Removes a task from the `tasks` array based on its `id`.
   - **toggleComplete Function**:
     - Toggles the `completed` status of a task based on its `id`.
   - **Passing Props**:
     - `TodoForm` receives `addTask` to add new tasks.
     - `TodoList` receives `tasks`, `deleteTask`, and `toggleComplete` to manage and display tasks.

---

## Implementing Event Handlers

We've already implemented the event handlers within the components, but let's recap their roles and interactions.

### 1. Adding a Task

- **Component**: `TodoForm`
- **Process**:
  - User types a task into the input field.
  - On form submission (`handleSubmit`), the `addTask` function from `App` is called with the task text.
  - `App` creates a new task object and updates the `tasks` state.
  - The new task appears in the `TodoList`.

### 2. Deleting a Task

- **Component**: `TodoItem`
- **Process**:
  - User clicks the "Delete" button on a task.
  - The `deleteTask` function from `App` is called with the task's `id`.
  - `App` filters out the task from the `tasks` state.
  - The task is removed from the `TodoList`.

### 3. Toggling Task Completion

- **Component**: `TodoItem`
- **Process**:
  - User clicks the "Complete" or "Undo" button on a task.
  - The `toggleComplete` function from `App` is called with the task's `id`.
  - `App` updates the `completed` status of the task in the `tasks` state.
  - The task's appearance changes to reflect its completion status.

---

## Adding Conditional Rendering

Conditional rendering allows us to display certain elements based on specific conditions, enhancing user interactivity.

### 1. Displaying Buttons on Hover

In the `TodoItem` component, the "Complete/Undo" and "Delete" buttons are only visible when the user hovers over a task.

**Implementation**:

- **State (`isHovered`)**: Determines if the mouse is over the task item.
- **Event Handlers**:
  - `onMouseEnter`: Sets `isHovered` to `true`.
  - `onMouseLeave`: Sets `isHovered` to `false`.
- **Conditional Rendering**: The buttons are rendered only when `isHovered` is `true`.

### 2. Strikethrough for Completed Tasks

When a task is marked as complete, its text is displayed with a strikethrough.

**Implementation**:

- **Conditional Styling**:
  - The `textDecoration` style is set to `'line-through'` if `task.completed` is `true`, otherwise `'none'`.

---

## Styling Components

We'll add CSS styles to enhance the appearance of our components, making the app more visually appealing.

### 1. Update `index.css`

Add the following styles to `index.css` to style the form, buttons, and task list.

```css
/* src/index.css */

/* Existing styles... */

/* Form styles */
form {
  display: flex;
  justify-content: center;
  margin-bottom: 1.5em;
}

form input {
  width: 70%;
  padding: 0.5em;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-right: 0.5em;
}

form button {
  padding: 0.5em 1em;
  background-color: #4a90e2;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

form button:hover {
  background-color: #357ab8;
}

/* Task list styles */
ul {
  list-style-type: none;
  padding: 0;
}

li {
  position: relative;
}

li button {
  margin-left: 0.5em;
}

/* Complete button color */
button:nth-child(1) {
  background-color: #28a745;
}

button:nth-child(1):hover {
  background-color: #218838;
}

/* Delete button color */
button:nth-child(2) {
  background-color: #dc3545;
}

button:nth-child(2):hover {
  background-color: #c82333;
}

/* Responsive design */
@media (max-width: 600px) {
  form input {
    width: 60%;
  }
}
```

**Explanation**:

- **Form Styling**:
  - **Layout**: Flexbox is used to align the input and button side by side.
  - **Input Field**: Styled with padding, border, and rounded corners.
  - **Add Task Button**: Styled with a blue background, white text, and hover effects.
  
- **Task List Styling**:
  - **List Items**: Removed default list styles and added padding and border.
  - **Buttons**: Colored green for "Complete" and red for "Delete" with hover effects.
  
- **Responsive Design**:
  - Adjusts the input field width on smaller screens for better usability.

### 2. Inline Styles in `TodoItem.jsx`

In `TodoItem.jsx`, we've used inline styles for simplicity. Here's a recap:

```jsx
// src/components/TodoItem.jsx
// ... previous code ...

return (
  <li
    onMouseEnter={handleMouseEnter}
    onMouseLeave={handleMouseLeave}
    style={{
      display: 'flex',
      justifyContent: 'space-between',
      alignItems: 'center',
      padding: '0.5em',
      borderBottom: '1px solid #ccc',
      textDecoration: task.completed ? 'line-through' : 'none',
    }}
  >
    <span>{task.text}</span>
    {isHovered && (
      <div>
        <button
          onClick={() => toggleComplete(task.id)}
          style={{
            marginRight: '0.5em',
            backgroundColor: task.completed ? '#ffc107' : '#28a745',
            color: '#fff',
            border: 'none',
            padding: '0.3em 0.6em',
            borderRadius: '4px',
            cursor: 'pointer',
          }}
        >
          {task.completed ? 'Undo' : 'Complete'}
        </button>
        <button
          onClick={() => deleteTask(task.id)}
          style={{
            backgroundColor: '#dc3545',
            color: '#fff',
            border: 'none',
            padding: '0.3em 0.6em',
            borderRadius: '4px',
            cursor: 'pointer',
          }}
        >
          Delete
        </button>
      </div>
    )}
  </li>
);
```

**Explanation**:

- **List Item (`li`)**: Styled to display content flexibly with space between the task text and action buttons.
- **Buttons**: 
  - **Complete Button**: Changes color based on the task's completion status (`green` for complete, `yellow` for undo).
  - **Delete Button**: Always red for deletion.
- **Strikethrough**: Applied to the task text if the task is marked as complete.

---

## Final Touches

Let's ensure all components are connected correctly and the app functions as expected.

### Step 1: Update `TodoForm.jsx` to Use Props

Ensure that `TodoForm` receives the `addTask` function via props.

```jsx
// src/components/TodoForm.jsx
import React, { useState } from 'react';

function TodoForm({ addTask }) {
  const [task, setTask] = useState('');

  const handleChange = (e) => {
    setTask(e.target.value);
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (task.trim()) {
      addTask(task);
      setTask('');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        placeholder="Enter a new task"
        value={task}
        onChange={handleChange}
      />
      <button type="submit">Add Task</button>
    </form>
  );
}

export default TodoForm;
```

### Step 2: Update `TodoList.jsx` to Use Props

Ensure that `TodoList` receives `tasks`, `deleteTask`, and `toggleComplete` via props.

```jsx
// src/components/TodoList.jsx
import React from 'react';
import TodoItem from './TodoItem';

function TodoList({ tasks, deleteTask, toggleComplete }) {
  return (
    <ul>
      {tasks.map((task) => (
        <TodoItem
          key={task.id}
          task={task}
          deleteTask={deleteTask}
          toggleComplete={toggleComplete}
        />
      ))}
    </ul>
  );
}

export default TodoList;
```

### Step 3: Test the Application

1. **Ensure the development server is running** (`npm run dev`).

2. **Open the app in your browser** (`http://localhost:5173` or the URL provided by Vite).

3. **Test Adding Tasks**:
   - Enter a task in the input field.
   - Click the "Add Task" button.
   - The task should appear in the list below.

4. **Test Marking as Complete**:
   - Hover over a task to reveal the "Complete" button.
   - Click "Complete" to mark the task as done (strikethrough applied).
   - The button should change to "Undo".

5. **Test Deleting Tasks**:
   - Hover over a task to reveal the "Delete" button.
   - Click "Delete" to remove the task from the list.

6. **Responsive Design**:
   - Resize the browser window to ensure the app remains user-friendly on different screen sizes.

---

## Conclusion

Congratulations! You've successfully built a **Basic React Todo List App** using fundamental React concepts. Here's what you've accomplished:

1. **Project Setup**: Initialized a React project using Vite for a fast development environment.
2. **Global Styling**: Applied a gradient background and styled the app container for a clean look.
3. **Component Structure**: Created modular components (`Header`, `TodoForm`, `TodoList`, `TodoItem`) for better code organization and reusability.
4. **State Management**: Managed tasks using React's `useState` hook in the `App` component.
5. **Event Handling**: Implemented functions to add, delete, and toggle tasks, enhancing interactivity.
6. **Conditional Rendering**: Displayed action buttons on hover and applied strikethrough styling for completed tasks.
7. **List Rendering with Keys**: Rendered a dynamic list of tasks efficiently using unique keys.
8. **Responsive Design**: Ensured the app looks good on various screen sizes.

### Next Steps

Now that you've built a basic Todo List app, here are some suggestions to further enhance your skills and the app:

1. **Persisting Data**:
   - Implement `localStorage` to save tasks so they persist even after refreshing the page.
   - **Example**: Use `useEffect` to load tasks from `localStorage` on mount and save them whenever `tasks` state changes.

2. **Advanced Styling**:
   - Explore CSS frameworks like **Bootstrap** or **Tailwind CSS** for more sophisticated designs.
   - Add animations for adding or removing tasks.

3. **Form Enhancements**:
   - Add validation to prevent adding empty tasks.
   - Allow editing existing tasks.

4. **State Management Libraries**:
   - Learn about **Redux** or **Context API** for managing state in larger applications.

5. **Testing**:
   - Introduce testing libraries like **Jest** and **React Testing Library** to write tests for your components.

6. **Deployment**:
   - Deploy your app to platforms like **Vercel**, **Netlify**, or **GitHub Pages** to share it with others.

### Additional Resources

- [React Official Documentation](https://reactjs.org/docs/getting-started.html)
- [Vite Official Documentation](https://vitejs.dev/)
- [MDN Web Docs on React](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Frameworks/React)
- [CSS Tricks](https://css-tricks.com/) for styling tips and tricks

Keep practicing by building more features and exploring advanced React topics. Happy coding!
