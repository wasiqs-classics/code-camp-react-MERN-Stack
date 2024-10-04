## Class 7: React Router

### 1. Introduction to Routing

#### What is Routing?
- **Routing**: In web development, routing refers to the mechanism of navigating between different pages or views in an application. It determines how an application responds to a client request for a specific endpoint, which is typically a URL path.

#### Example of Routing in Traditional HTML/CSS/JS Projects
- In traditional web development, routing is often handled by the server. When a user clicks a link, the browser sends a request to the server, which then responds with the appropriate HTML page.
- **Example**:
  ```html
  <!-- index.html -->
  <nav>
    <a href="home.html">Home</a>
    <a href="about.html">About</a>
    <a href="contact.html">Contact</a>
  </nav>
  <div id="content">
    <!-- Content will be loaded here -->
  </div>
  ```

  ```html
  <!-- home.html -->
  <h1>Home Page</h1>
  <p>Welcome to the home page!</p>
  ```

  ```html
  <!-- about.html -->
  <h1>About Page</h1>
  <p>Learn more about us on this page.</p>
  ```

  ```html
  <!-- contact.html -->
  <h1>Contact Page</h1>
  <p>Get in touch with us!</p>
  ```

- In this example, clicking on the links in the navigation bar will load different HTML pages from the server.

### 2. React Router

#### How Routing is Different in React Applications
- Unlike traditional HTML routing, React applications use client-side routing. This means that the routing is handled within the browser using JavaScript, without requiring a full page reload. React Router is a popular library for implementing routing in React applications.

#### Setting up React Router
1. **Installation**:
   - Install React Router using npm or yarn:
     ```bash
     npm install react-router-dom
     ```

2. **Basic Setup**:
   - Wrap your application with the `BrowserRouter` component to enable routing:
     ```jsx
     import React from 'react';
     import ReactDOM from 'react-dom';
     import { BrowserRouter } from 'react-router-dom';
     import App from './App';

     ReactDOM.render(
       <BrowserRouter>
         <App />
       </BrowserRouter>,
       document.getElementById('root')
     );
     ```

#### Explanation of Components
- **BrowserRouter**: This component uses the HTML5 history API to keep your UI in sync with the URL. It should wrap your entire application to enable routing.
- **Routes**: This component is used to define all the routes in your application.
- **Route**: This component is used to specify a single route. It takes a `path` prop to define the URL path and an `element` prop to define the component to render.

### 3. Single Page Applications (SPA)

#### What is a Single Page Application (SPA)?
- **SPA**: A single-page application loads a single HTML page and dynamically updates the content as the user interacts with the app. This provides a smoother and faster user experience compared to traditional multi-page applications.

#### Creating a Basic SPA with Routing
1. **Define Routes**:
   - Use the `Routes` and `Route` components to define your application's routes:
     ```jsx
     import React from 'react';
     import { Routes, Route } from 'react-router-dom';
     import Home from './Home';
     import About from './About';
     import Contact from './Contact';

     function App() {
       return (
         <Routes>
           <Route path="/" element={<Home />} />
           <Route path="/about" element={<About />} />
           <Route path="/contact" element={<Contact />} />
         </Routes>
       );
     }

     export default App;
     ```

2. **Create Components**:
   - Create simple components for each route:
     ```jsx
     // Home.jsx
     import React from 'react';

     function Home() {
       return <h1>Home Page</h1>;
     }

     export default Home;
     ```

     ```jsx
     // About.jsx
     import React from 'react';

     function About() {
       return <h1>About Page</h1>;
     }

     export default About;
     ```

     ```jsx
     // Contact.jsx
     import React from 'react';

     function Contact() {
       return <h1>Contact Page</h1>;
     }

     export default Contact;
     ```

3. **Navigation**:
   - Use the `Link` component to navigate between routes without reloading the page:
     ```jsx
     import React from 'react';
     import { Link } from 'react-router-dom';

     function Navbar() {
       return (
         <nav>
           <ul>
             <li><Link to="/">Home</Link></li>
             <li><Link to="/about">About</Link></li>
             <li><Link to="/contact">Contact</Link></li>
           </ul>
         </nav>
       );
     }

     export default Navbar;
     ```

### 4. Example: Portfolio of John Doe

Let's build a SPA for John Doe, a web developer, with the following components: Home, About, Projects, Tech Stack, and Contact.

1. **Define Routes**:
   ```jsx
   import React from 'react';
   import { Routes, Route } from 'react-router-dom';
   import Home from './Home';
   import About from './About';
   import Projects from './Projects';
   import TechStack from './TechStack';
   import Contact from './Contact';

   function App() {
     return (
       <Routes>
         <Route path="/" element={<Home />} />
         <Route path="/about" element={<About />} />
         <Route path="/projects" element={<Projects />} />
         <Route path="/techstack" element={<TechStack />} />
         <Route path="/contact" element={<Contact />} />
       </Routes>
     );
   }

   export default App;
   ```

2. **Create Components**:
   - **Home.jsx**:
     ```jsx
     import React from 'react';

     function Home() {
       return <h1>Welcome to John Doe's Portfolio</h1>;
     }

     export default Home;
     ```

   - **About.jsx**:
     ```jsx
     import React from 'react';

     function About() {
       return <h1>About John Doe</h1>;
     }

     export default About;
     ```

   - **Projects.jsx**:
     ```jsx
     import React from 'react';

     function Projects() {
       const projectList = [
         { id: 1, name: 'Project One', description: 'Description of Project One' },
         { id: 2, name: 'Project Two', description: 'Description of Project Two' },
       ];

       return (
         <div>
           <h1>Projects</h1>
           {projectList.map(project => (
             <div key={project.id}>
               <h2>{project.name}</h2>
               <p>{project.description}</p>
             </div>
           ))}
         </div>
       );
     }

     export default Projects;
     ```

   - **TechStack.jsx**:
     ```jsx
     import React from 'react';

     function TechStack() {
       const techStack = [
         { id: 1, name: 'React' },
         { id: 2, name: 'Node.js' },
       ];

       return (
         <div>
           <h1>Tech Stack</h1>
           {techStack.map(tech => (
             <div key={tech.id}>
               <h2>{tech.name}</h2>
             </div>
           ))}
         </div>
       );
     }

     export default TechStack;
     ```

   - **Contact.jsx**:
     ```jsx
     import React, { useState } from 'react';

     function Contact() {
       const [formData, setFormData] = useState({ name: '', email: '', message: '' });

       const handleChange = (e) => {
         const { name, value } = e.target;
         setFormData({ ...formData, [name]: value });
       };

       const handleSubmit = (e) => {
         e.preventDefault();
         alert(`Message sent by ${formData.name}`);
       };

       return (
         <form onSubmit={handleSubmit}>
           <label>
             Name:
             <input type="text" name="name" value={formData.name} onChange={handleChange} />
           </label>
           <br />
           <label>
             Email:
             <input type="email" name="email" value={formData.email} onChange={handleChange} />
           </label>
           <br />
           <label>
             Message:
             <textarea name="message" value={formData.message} onChange={handleChange}></textarea>
           </label>
           <br />
           <button type="submit">Send</button>
         </form>
       );
     }

     export default Contact;
     ```

Certainly! Let's continue from where we left off.

3. **Navigation**:
   - **Navbar.jsx**:
     ```jsx
     import React from 'react';
     import { Link } from 'react-router-dom';

     function Navbar() {
       return (
         <nav>
           <ul>
             <li><Link to="/">Home</Link></li>
             <li><Link to="/about">About</Link></li>
             <li><Link to="/projects">Projects</Link></li>
             <li><Link to="/techstack">Tech Stack</Link></li>
             <li><Link to="/contact">Contact</Link></li>
           </ul>
         </nav>
       );
     }

     export default Navbar;
     ```

4. **Integrate Navbar in App**:
   - Update the `App.jsx` to include the `Navbar` component:
     ```jsx
     import React from 'react';
     import { Routes, Route } from 'react-router-dom';
     import Navbar from './Navbar';
     import Home from './Home';
     import About from './About';
     import Projects from './Projects';
     import TechStack from './TechStack';
     import Contact from './Contact';

     function App() {
       return (
         <div>
           <Navbar />
           <Routes>
             <Route path="/" element={<Home />} />
             <Route path="/about" element={<About />} />
             <Route path="/projects" element={<Projects />} />
             <Route path="/techstack" element={<TechStack />} />
             <Route path="/contact" element={<Contact />} />
           </Routes>
         </div>
       );
     }

     export default App;
     ```

### 5. Nested Routes

#### What are Nested Routes?
- **Nested Routes**: Nested routes allow you to render components within other components based on the URL path. This is useful for creating layouts with sub-sections or nested views.

#### Handling Nested Routes
1. **Define Nested Routes**:
   - Use the `Route` component's `children` prop to define nested routes:
     ```jsx
     import React from 'react';
     import { Routes, Route } from 'react-router-dom';
     import Dashboard from './Dashboard';
     import Profile from './Profile';
     import Settings from './Settings';

     function App() {
       return (
         <Routes>
           <Route path="/" element={<Dashboard />}>
             <Route path="profile" element={<Profile />} />
             <Route path="settings" element={<Settings />} />
           </Route>
         </Routes>
       );
     }

     export default App;
     ```

2. **Create Components**:
   - **Dashboard.jsx**:
     ```jsx
     import React from 'react';
     import { Outlet, Link } from 'react-router-dom';

     function Dashboard() {
       return (
         <div>
           <h1>Dashboard</h1>
           <nav>
             <ul>
               <li><Link to="profile">Profile</Link></li>
               <li><Link to="settings">Settings</Link></li>
             </ul>
           </nav>
           <Outlet />
         </div>
       );
     }

     export default Dashboard;
     ```

   - **Profile.jsx**:
     ```jsx
     import React from 'react';

     function Profile() {
       return <h1>Profile Page</h1>;
     }

     export default Profile;
     ```

   - **Settings.jsx**:
     ```jsx
     import React from 'react';

     function Settings() {
       return <h1>Settings Page</h1>;
     }

     export default Settings;
     ```

3. **Using `Outlet`**:
   - The `Outlet` component is used to render the nested route components within the parent component.

#### Use Cases for Nested Routes
- **User Dashboard**: A dashboard with sections like Profile, Settings, and Notifications.
- **E-commerce Site**: A product page with nested routes for details, reviews, and related products.
- **Blog**: A blog with nested routes for categories, tags, and individual posts.

### Summary

In this class, we covered the basics of routing in React using React Router. We started with an introduction to routing, comparing traditional HTML routing with React's client-side routing. We then set up React Router in a React application and created a basic single-page application (SPA) with routing. We also explored nested routes and their use cases.

By the end of this class, you should have a good understanding of how to implement routing in a React application, create nested routes, and build a simple portfolio SPA for John Doe (or yourself!). This knowledge will help you create more dynamic and interactive React applications.


