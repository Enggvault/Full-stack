# Tech Stacks — Web Development

> **Day 2 | Full Stack Development**
> Structured notes covering tech stacks, frontend, backend, databases, DOM, and BOM.

---

## Table of Contents

1. [What is a Tech Stack?](#1-what-is-a-tech-stack)
2. [Main Parts of a Tech Stack](#2-main-parts-of-a-tech-stack)
3. [Frontend Technologies](#3-frontend-technologies)
4. [Backend Technologies](#4-backend-technologies)
5. [Databases](#5-databases)
6. [Popular Tech Stacks](#6-popular-tech-stacks)
7. [MERN Stack](#7-mern-stack)
8. [MARN Stack](#8-marn-stack)
9. [DOM — Document Object Model](#9-dom--document-object-model)
10. [BOM — Browser Object Model](#10-bom--browser-object-model)
11. [Comparison Tables](#11-comparison-tables)
12. [Architecture Diagrams](#12-architecture-diagrams)
13. [Quick Revision](#13-quick-revision)

---

## 1. What is a Tech Stack?

A **tech stack** is the complete set of technologies — programming languages, frameworks, libraries, databases, and tools — chosen to build a particular application.

Every application has a stack, whether explicitly defined or not. The stack determines:
- What language(s) developers write
- How fast the application can be built
- How well the application performs at scale
- How easy it is to find developers who can work on it

### Analogy — Building a House

```
+---------------------------+      +---------------------------+
|   Building a House        |      |   Building a Web App      |
+---------------------------+      +---------------------------+
| Foundation                | ---> | Database                  |
| Structural frame          | ---> | Backend / Server          |
| Electrical + Plumbing     | ---> | API Layer                 |
| Interior design           | ---> | Frontend (UI)             |
| Street address / location | ---> | Hosting / Deployment      |
+---------------------------+      +---------------------------+
```

Just as a house requires a coordinated set of materials and trades, a web app requires a coordinated set of technologies working in layers.

---

## 2. Main Parts of a Tech Stack

Every tech stack is divided into distinct layers, each owning a specific responsibility:

```
+--------------------------------------------------+
|                  Frontend                        |
|         HTML, CSS, JavaScript, React             |
+--------------------------------------------------+
                        |
+--------------------------------------------------+
|           API / Communication Layer              |
|         REST API, GraphQL, WebSockets            |
+--------------------------------------------------+
                        |
+--------------------------------------------------+
|                  Backend                         |
|         Node.js, Express, Django, Spring Boot    |
+--------------------------------------------------+
                        |
+--------------------------------------------------+
|                  Database                        |
|         MongoDB, PostgreSQL, MySQL, Redis        |
+--------------------------------------------------+
                        |
+--------------------------------------------------+
|               Developer Tools                    |
|         Git, VS Code, Docker, Postman            |
+--------------------------------------------------+
                        |
+--------------------------------------------------+
|            Deploy / Hosting                      |
|         Vercel, AWS, Netlify, Railway            |
+--------------------------------------------------+
```

### Layer Summary

| Layer            | Responsibility                                    | Example Technologies              |
|------------------|---------------------------------------------------|-----------------------------------|
| Frontend         | Visual interface the user sees and interacts with | React, HTML, CSS, Tailwind        |
| API              | Communication channel between frontend/backend    | REST, GraphQL, WebSockets         |
| Backend          | Server logic, request processing, business rules  | Node.js, Express, Django          |
| Database         | Permanent storage of all application data         | MongoDB, PostgreSQL, MySQL, Redis |
| Dev Tools        | Tools that assist and streamline development      | Git, VS Code, Docker, Postman     |
| Deploy / Hosting | Deploying the app so users can access it online   | Vercel, AWS, Netlify, Railway     |

---

## 3. Frontend Technologies

The frontend is the part of the application that runs directly in the user's browser. It defines what the user sees and how they interact with the app.

### HTML — HyperText Markup Language

- **Role:** Defines the structure and content of a webpage.
- **Elements used:** Headings, paragraphs, images, links, forms, buttons, tables.
- **Example:**
```html
<h1>Welcome to My App</h1>
<button id="loginBtn">Login</button>
```

### CSS — Cascading Style Sheets

- **Role:** Controls the visual presentation of HTML elements.
- **Features:** Colors, fonts, spacing, layout (Flexbox, Grid), animations.
- **Example:**
```css
body {
  font-family: 'Inter', sans-serif;
  background-color: #f5f5f5;
}
```

### JavaScript

- **Role:** Adds interactivity and dynamic behavior to the browser.
- **Features:** Click/keyboard event handling, DOM manipulation, API requests.
- **Example:**
```javascript
document.getElementById("loginBtn").addEventListener("click", function() {
  console.log("Login button clicked");
});
```

### React.js

- **Role:** JavaScript library for building component-based user interfaces.
- **Key concept:** The UI is split into reusable, self-contained components (Header, Button, Card, Form).
- **Used by:** Instagram, Airbnb, Netflix, Atlassian, Meta.

### Next.js

- **Role:** React framework with built-in server-side rendering (SSR), static generation, and file-based routing.
- **Benefits:** Better SEO, faster initial page loads, simplified routing.
- **Used for:** Blogs, e-commerce, portfolios, dashboards.

### Tailwind CSS

- **Role:** Utility-first CSS framework for rapidly styling elements using predefined class names.
- **Example:**
```html
<div class="flex items-center justify-center p-6 bg-gray-100 rounded-xl">
  Content
</div>
```

### Bootstrap

- **Role:** CSS framework providing pre-built, responsive UI components.
- **Example:**
```html
<button class="btn btn-primary">Submit</button>
```

---

## 4. Backend Technologies

The backend runs on a server — not in the browser. It handles all the logic, validates user input, enforces security rules, communicates with the database, and returns structured responses to the frontend.

| Technology       | Language       | Primary Use Case                                           |
|------------------|----------------|------------------------------------------------------------|
| Node.js          | JavaScript     | Runs JavaScript on the server; non-blocking I/O            |
| Express.js       | JavaScript     | Minimalist framework for building REST APIs on Node.js     |
| Django           | Python         | Full-featured framework with built-in admin, ORM, auth     |
| Flask            | Python         | Lightweight micro-framework; good for small APIs           |
| Spring Boot      | Java           | Enterprise-grade applications (banking, finance, ERP)      |
| NestJS           | TypeScript     | Structured, scalable backend framework (Angular-inspired)  |

### Choosing a Backend Framework

```
Small API or prototype        -->  Flask  or  Express.js
Full-featured web application -->  Django  or  NestJS
Enterprise-scale system       -->  Spring Boot
JavaScript across the stack   -->  Node.js + Express.js
TypeScript backend            -->  NestJS
```

---

## 5. Databases

A database is the persistent storage layer of an application. All data that must survive beyond a single request — user accounts, orders, messages, settings — is stored here.

### Two Major Categories

```
Databases
|
+-- SQL (Relational)
|      Data organized into tables with rows and columns.
|      Relationships between tables are enforced by the database.
|      +-- PostgreSQL
|      +-- MySQL
|      +-- SQLite
|      +-- Supabase (built on PostgreSQL)
|
+-- NoSQL (Non-Relational)
       Data stored in documents, key-value pairs, or graphs.
       Flexible schema — structure can vary between records.
       +-- MongoDB
       +-- Firebase
       +-- Redis
       +-- Cassandra
```

### Database Comparison

| Database     | Type     | Best Suited For                                        |
|--------------|----------|--------------------------------------------------------|
| MongoDB      | NoSQL    | Flexible JSON documents; great with Node.js            |
| PostgreSQL   | SQL      | Complex queries and relations; high reliability        |
| MySQL        | SQL      | Widely supported; standard for web applications        |
| Firebase     | NoSQL    | Real-time sync; mobile apps; Google ecosystem          |
| Redis        | NoSQL    | In-memory caching, sessions, real-time pub/sub         |
| Supabase     | SQL      | Open-source Firebase alternative built on PostgreSQL   |

### SQL vs NoSQL

| Feature         | SQL                                 | NoSQL                               |
|-----------------|-------------------------------------|-------------------------------------|
| Data format     | Tables (rows and columns)           | Documents, key-value, graphs        |
| Schema          | Fixed — defined strictly upfront    | Flexible — can vary per record      |
| Relationships   | Native (JOIN, foreign keys)         | Handled in application code         |
| Best for        | Financial data, ERP, structured data| Rapid dev, unstructured/varied data |
| Examples        | PostgreSQL, MySQL, SQLite           | MongoDB, Firebase, Redis            |

---

## 6. Popular Tech Stacks

A named stack is a shorthand label for a frequently used combination of technologies across the frontend, backend, and database layers.

| Stack Name   | Technologies                                        | Primary Use Case                         |
|--------------|-----------------------------------------------------|------------------------------------------|
| MERN         | MongoDB, Express, React, Node.js                    | Full-stack JavaScript web apps           |
| MEAN         | MongoDB, Express, Angular, Node.js                  | Full-stack JS with Angular frontend      |
| MARN         | MongoDB, Angular, React, Node.js                    | Hybrid frontend; enterprise apps         |
| LAMP         | Linux, Apache, MySQL, PHP                           | Traditional web apps; WordPress          |
| PERN         | PostgreSQL, Express, React, Node.js                 | Full-stack JS with a relational database |
| T3 Stack     | TypeScript, tRPC, Tailwind, Prisma, Next.js         | Type-safe modern full-stack apps         |
| Django Stack | Django, PostgreSQL, HTML/CSS                        | Python-based full-stack applications     |

---

## 7. MERN Stack

**MERN = MongoDB + Express.js + React.js + Node.js**

The MERN stack uses **JavaScript across the entire application** — one language for the frontend, backend, and database queries. This makes it one of the most accessible and widely taught full-stack setups.

### What Each Letter Represents

| Letter | Technology  | Layer    | Role                                            |
|--------|-------------|----------|-------------------------------------------------|
| M      | MongoDB     | Database | Stores data as flexible JSON-like documents     |
| E      | Express.js  | Backend  | Handles API routes and middleware               |
| R      | React.js    | Frontend | Builds the user interface using components      |
| N      | Node.js     | Runtime  | Runs JavaScript on the server                   |

### MERN Request Flow

```
+-------------------+
|  User (Browser)   |
+-------------------+
         |
         |  Opens the application
         v
+-------------------+
|    React.js       |  Renders the UI; handles user events
+-------------------+
         |
         |  User clicks "Load Users" -- React sends:
         |  GET /api/users
         v
+-------------------+
|   Express.js      |  Receives the request; routes to handler
+-------------------+
         |
         v
+-------------------+
|    Node.js        |  Runs server-side logic; processes the request
+-------------------+
         |
         v
+-------------------+
|    MongoDB        |  Queries the database; fetches user records
+-------------------+
         |
         |  Returns JSON data to Express
         v
+-------------------+
|   Express.js      |  Formats and sends JSON response
+-------------------+
         |
         v
+-------------------+
|    React.js       |  Receives data; updates component state; re-renders
+-------------------+
         |
         v
+-------------------+
|  User sees result |
+-------------------+
```

### Example — To-Do App Flow

| Step | Actor         | Action                                                     |
|------|---------------|------------------------------------------------------------|
| 1    | User          | Opens the app — React renders the to-do list               |
| 2    | User          | Types a new task and clicks "Add"                          |
| 3    | React.js      | Sends `POST /api/todos` with the task data                 |
| 4    | Express.js    | Receives and validates the request                         |
| 5    | Node.js       | Instructs MongoDB to save the new task                     |
| 6    | MongoDB       | Saves the document; returns success confirmation           |
| 7    | Express.js    | Sends the success response back to React                   |
| 8    | React.js      | Updates the list on screen; user sees the new task         |

### Why MERN Is Popular

- One language (JavaScript) used across frontend, backend, and queries
- Massive community — large number of tutorials, packages, and job listings
- Fast development cycle; well-suited to startups and MVPs
- React's component model scales well for complex interfaces

---

## 8. MARN Stack

**MARN = MongoDB + Angular + React + Node.js**

MARN is an advanced, less common configuration used when a project requires both Angular and React simultaneously. This typically occurs in large enterprise settings where different teams or micro-frontends use different frameworks.

### What Each Letter Represents

| Letter | Technology | Role                                                          |
|--------|------------|---------------------------------------------------------------|
| M      | MongoDB    | Stores data as flexible JSON-like documents                   |
| A      | Angular    | Full frontend framework by Google; TypeScript-first; opinionated |
| R      | React      | Flexible frontend library; component-based; JavaScript/TSX    |
| N      | Node.js    | Server-side runtime                                           |

### When MARN Is Used

- **Micro-frontend architecture:** Different teams own separate parts of the UI using their preferred framework.
- **Gradual migration:** A team is incrementally moving from an Angular codebase to React.
- **Enterprise systems:** Existing Angular applications being extended with new React-based features.

### MERN vs MARN

| Feature           | MERN                               | MARN                                       |
|-------------------|------------------------------------|--------------------------------------------|
| Frontend          | React only                         | Angular + React (both used)                |
| Languages         | JavaScript                         | JavaScript + TypeScript                    |
| Learning curve    | Moderate (one frontend framework)  | Steeper (two frontend frameworks)          |
| Team size         | Small to medium teams              | Large teams; enterprise environments       |
| Typical use case  | Startups, SPAs, standard web apps  | Large apps, micro-frontends, migrations    |
| Community size    | Very large                         | Smaller (less common combination)          |

> MERN is the standard choice for most web applications. MARN is a specialized architecture for specific enterprise scenarios.

---

## 9. DOM — Document Object Model

### Definition

The **DOM** (Document Object Model) is a programming interface created by the browser when it loads an HTML page. The browser reads the HTML and constructs a **tree-like structure in memory** where every HTML element becomes a **node** that JavaScript can read, modify, create, or delete — without reloading the page.

### Why the DOM Matters

| Purpose                      | Example                                              |
|------------------------------|------------------------------------------------------|
| Find elements on the page    | Locate a login button, an input field, or a div      |
| Modify content dynamically   | Update displayed text or swap an image source        |
| Add or remove elements       | Insert a new notification; delete a list item        |
| Respond to user events       | Handle clicks, keyboard input, form submissions      |

### DOM Tree Structure

When the browser parses this HTML:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Page</title>
  </head>
  <body>
    <h1>Hello World</h1>
    <div>
      <button id="btn">Click Me</button>
    </div>
  </body>
</html>
```

It builds this tree in memory:

```
Document
  |
  +-- <html>
        |
        +-- <head>
        |     |
        |     +-- <title>  "My Page"
        |
        +-- <body>
              |
              +-- <h1>  "Hello World"
              |
              +-- <div>
                    |
                    +-- <button id="btn">  "Click Me"
```

Every node in this tree can be targeted and manipulated by JavaScript.

### Common DOM Methods

| Method / Property                         | Description                                      |
|-------------------------------------------|--------------------------------------------------|
| `document.getElementById("id")`           | Find an element by its `id` attribute            |
| `document.querySelector(".class")`        | Find the first element matching a CSS selector   |
| `document.querySelectorAll("tag")`        | Find all elements matching a CSS selector        |
| `element.innerHTML`                       | Get or set the HTML content inside an element    |
| `element.textContent`                     | Get or set the plain text inside an element      |
| `element.style.color = "red"`             | Apply a CSS style directly via JavaScript        |
| `element.classList.add("active")`         | Add a CSS class to an element                    |
| `element.classList.remove("active")`      | Remove a CSS class from an element               |
| `element.addEventListener("click", fn)`   | Attach an event listener to an element           |
| `document.createElement("div")`           | Create a new HTML element                        |
| `parent.appendChild(child)`               | Add a new child element to a parent node         |
| `element.remove()`                        | Remove an element from the DOM entirely          |

### DOM Examples

**Change text on button click:**
```html
<h1 id="heading">Original Text</h1>
<button onclick="updateText()">Click Me</button>

<script>
  function updateText() {
    document.getElementById("heading").textContent = "Text was changed!";
  }
</script>
```

**Add a new item to a list:**
```javascript
let list = document.getElementById("taskList");
let item = document.createElement("li");
item.textContent = "New task added";
list.appendChild(item);
```

**Change background color:**
```javascript
let box = document.querySelector(".card");
box.style.backgroundColor = "#1a1a2e";
```

---

## 10. BOM — Browser Object Model

### Definition

The **BOM** (Browser Object Model) is the interface through which JavaScript communicates with the **browser itself** — not the HTML page content, but the browser's environment: the current URL, navigation history, screen dimensions, dialog boxes, and timers.

### BOM Structure

```
window   (global top-level object — represents the browser tab)
  |
  +-- window.location
  |     Controls the current URL
  |     +-- href          -->  Get or set the current URL
  |     +-- reload()      -->  Reload the current page
  |     +-- assign(url)   -->  Navigate to a new URL
  |
  +-- window.history
  |     Browser session navigation history
  |     +-- back()        -->  Go to previous page
  |     +-- forward()     -->  Go to next page
  |     +-- go(n)         -->  Go n steps in history
  |
  +-- window.navigator
  |     Information about the browser and operating system
  |     +-- userAgent     -->  Browser identification string
  |     +-- language      -->  User's preferred language
  |     +-- onLine        -->  Boolean: is the user online?
  |
  +-- window.screen
  |     Information about the user's display
  |     +-- width         -->  Screen width in pixels
  |     +-- height        -->  Screen height in pixels
  |
  +-- window.alert(msg)       -->  Display a popup alert
  +-- window.confirm(msg)     -->  Show a yes/no dialog; returns boolean
  +-- window.prompt(msg)      -->  Prompt user to enter text; returns string
  |
  +-- setTimeout(fn, ms)      -->  Run a function once after a delay
  +-- setInterval(fn, ms)     -->  Run a function repeatedly at an interval
  +-- clearTimeout(id)        -->  Cancel a pending setTimeout
  +-- clearInterval(id)       -->  Stop a running setInterval
```

> The `window` prefix is optional in most cases — `alert()` and `window.alert()` are equivalent because `window` is the global object.

### Common BOM Reference

| Object / Method              | Description                                          |
|------------------------------|------------------------------------------------------|
| `window.location.href`       | Get or set the current page URL                      |
| `window.location.reload()`   | Reload the current page                              |
| `window.history.back()`      | Navigate to the previous page                        |
| `window.history.forward()`   | Navigate to the next page                            |
| `window.navigator.userAgent` | Returns the browser and OS identification string     |
| `window.screen.width`        | Returns the screen width in pixels                   |
| `window.screen.height`       | Returns the screen height in pixels                  |
| `window.alert("message")`    | Display a popup alert box                            |
| `window.confirm("message")`  | Show a yes/no dialog; returns `true` or `false`      |
| `window.prompt("message")`   | Show a text input dialog; returns the string entered |
| `setTimeout(fn, ms)`         | Execute a function once after `ms` milliseconds      |
| `setInterval(fn, ms)`        | Execute a function repeatedly every `ms` ms          |

### BOM Examples

**Redirect to another page:**
```javascript
window.location.href = "https://www.example.com";
```

**Go back to the previous page:**
```javascript
window.history.back();
```

**Get screen dimensions:**
```javascript
console.log("Width:  " + window.screen.width + "px");
console.log("Height: " + window.screen.height + "px");
```

**Confirm before a destructive action:**
```javascript
let confirmed = window.confirm("Are you sure you want to delete this item?");
if (confirmed) {
  console.log("Item deleted.");
} else {
  console.log("Action cancelled.");
}
```

**Run code after a delay:**
```javascript
setTimeout(function() {
  console.log("This ran 3 seconds after the page loaded.");
}, 3000);
```

**Run code on a repeating interval:**
```javascript
let count = 0;
let timer = setInterval(function() {
  count++;
  console.log("Tick: " + count);
  if (count === 5) {
    clearInterval(timer);  // stop after 5 ticks
  }
}, 1000);
```

---

## 11. Comparison Tables

### DOM vs BOM

| Feature          | DOM                                         | BOM                                           |
|------------------|---------------------------------------------|-----------------------------------------------|
| Full form        | Document Object Model                       | Browser Object Model                          |
| Controls         | The HTML document (page content, elements)  | The browser environment (URL, history, screen)|
| Root object      | `document`                                  | `window`                                      |
| Key methods      | `getElementById`, `querySelector`, `addEventListener` | `location`, `history`, `navigator`, `setTimeout` |
| Primary use      | Manipulate page structure and content       | Control browser behavior and read environment |
| Standardised?    | Yes — W3C standard                          | No — browser-defined; no formal specification |

### MERN vs MARN

| Feature           | MERN Stack                          | MARN Stack                                |
|-------------------|-------------------------------------|-------------------------------------------|
| Full form         | MongoDB Express React Node          | MongoDB Angular React Node                |
| Frontend          | React only                          | Angular + React (both used together)      |
| Language          | JavaScript                          | JavaScript + TypeScript                   |
| Learning curve    | Moderate                            | Steeper (two frontend frameworks)         |
| Common use        | Startups, SPAs, standard apps       | Enterprise apps, micro-frontends          |
| Community size    | Very large                          | Smaller (less common)                     |

### SQL vs NoSQL

| Feature         | SQL                              | NoSQL                              |
|-----------------|----------------------------------|------------------------------------|
| Data format     | Rows and columns (tables)        | Documents, key-value, graphs       |
| Schema          | Fixed; defined strictly upfront  | Flexible; varies per record        |
| Relationships   | Native — JOIN, foreign keys      | Handled manually in application    |
| Best for        | Structured, relational data      | Flexible or rapidly changing data  |
| Examples        | PostgreSQL, MySQL, SQLite        | MongoDB, Firebase, Redis           |

---

## 12. Architecture Diagrams

### General Web Request Lifecycle

```
User
  |
  |  Types a URL or clicks a button
  v
Browser
  |
  |  Sends HTTP Request
  v
Frontend  (React / HTML / CSS)
  |
  |  Makes an API call
  v
API Layer  (Express route / REST endpoint)
  |
  |  Passes to backend handler
  v
Backend Logic  (Node.js / Django / etc.)
  |
  |  Queries the database
  v
Database  (MongoDB / PostgreSQL / MySQL)
  |
  |  Returns data
  v
Backend formats JSON response
  |
  v
API sends response to Frontend
  |
  v
Frontend updates the UI
  |
  v
User sees the updated result
```

### MERN Stack — Full Request Cycle

```
+------------------+
|  User (Browser)  |
+------------------+
         |
         v
+------------------+
|    React.js      |  Renders UI; captures user interactions
+------------------+
         |
         |  HTTP API call (fetch / axios)
         |  Example: GET /api/products
         v
+------------------+
|   Express.js     |  Routes request to the correct handler
+------------------+
         |
         v
+------------------+
|    Node.js       |  Executes server-side logic
+------------------+
         |
         v
+------------------+
|    MongoDB       |  Reads or writes data
+------------------+
         |
         |  Returns JSON
         v
+------------------+
|   Express.js     |  Sends JSON response
+------------------+
         |
         v
+------------------+
|    React.js      |  Updates state and re-renders components
+------------------+
         |
         v
+------------------+
|  User sees data  |
+------------------+
```

### DOM Tree

```
         document
             |
           <html>
           /     \
       <head>   <body>
         |       /    \
      <title>  <h1>  <div>
         |       |       \
       "Page" "Hello"  <button id="btn">
                            |
                         "Click Me"
```

### BOM Structure

```
                   window
          /    /    |     \     \
    location history navigator screen  timers
       |        |        |        |
     href    back()  userAgent  width    setTimeout()
    reload  forward  language   height   setInterval()
     assign   go(n)   onLine
```

---

## 13. Quick Revision

### Key Terms

| Term         | One-Line Definition                                                      |
|--------------|--------------------------------------------------------------------------|
| Tech Stack   | The full set of technologies chosen to build an application              |
| Frontend     | The visual layer users see and interact with (HTML, CSS, JS, React)     |
| Backend      | Server-side logic and processing (Node.js, Express, Django)              |
| Database     | Persistent storage layer for all application data                        |
| API          | The defined interface connecting frontend to backend                     |
| DOM          | The browser's in-memory tree of the HTML page, accessible by JavaScript  |
| BOM          | JavaScript's interface to the browser's own environment and features     |
| MERN         | MongoDB + Express + React + Node.js (JavaScript full-stack)              |
| MARN         | MongoDB + Angular + React + Node.js (hybrid enterprise frontend)         |
| Cache        | Temporary fast storage (e.g. Redis) to reduce repeated database queries  |

### True / False Check

| Statement                                           | Answer                                 |
|-----------------------------------------------------|----------------------------------------|
| JavaScript can manipulate the DOM                   | True                                   |
| The BOM controls HTML page elements                 | False — that is the DOM                |
| MERN uses Python as its backend language            | False — it uses JavaScript             |
| MongoDB is a NoSQL database                         | True                                   |
| React is a backend framework                        | False — it is a frontend library       |
| `window.location` is part of the BOM               | True                                   |
| Express.js runs on top of Node.js                   | True                                   |
| SQL databases use a flexible, schema-less format    | False — that describes NoSQL           |

### DOM Methods Reference

```
getElementById(id)        -->  Find element by its id attribute
querySelector(selector)   -->  Find first element matching a CSS selector
querySelectorAll(selector)-->  Find all elements matching a CSS selector
innerHTML                 -->  Get or set HTML content inside an element
textContent               -->  Get or set plain text inside an element
addEventListener(event)   -->  Attach an event handler (click, input, etc.)
createElement(tag)        -->  Create a new HTML element
appendChild(child)        -->  Add a child element to a parent node
remove()                  -->  Remove an element from the page
```

### BOM Reference

```
window.location.href       -->  Get or set the current page URL
window.location.reload()   -->  Reload the page
window.history.back()      -->  Navigate to the previous page
window.screen.width        -->  Get the user's screen width in pixels
window.alert("msg")        -->  Show a popup alert
window.confirm("msg")      -->  Show a yes/no confirmation dialog
window.prompt("msg")       -->  Prompt user to enter text
setTimeout(fn, ms)         -->  Run a function once after a delay
setInterval(fn, ms)        -->  Run a function repeatedly at an interval
window.navigator.userAgent -->  Get the browser and OS identification string
```

### Named Stacks at a Glance

```
MERN  =  MongoDB + Express + React + Node      Most popular JavaScript stack
MEAN  =  MongoDB + Express + Angular + Node    Angular instead of React
MARN  =  MongoDB + Angular + React + Node      Hybrid; enterprise use
LAMP  =  Linux + Apache + MySQL + PHP          Traditional; still widely deployed
PERN  =  PostgreSQL + Express + React + Node   MERN with a relational database
T3    =  TypeScript + tRPC + Tailwind + Prisma + Next.js   Type-safe modern stack
```

