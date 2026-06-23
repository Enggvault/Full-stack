# 📚 Tech Stacks
> **Day 2 | Full Stack Development**
> _Simple notes, easy language, quick to revise!_


## 📌 Table of Contents
1. [What is a Tech Stack?](#1-what-is-a-tech-stack)
2. [Main Parts of a Tech Stack](#2-main-parts-of-a-tech-stack)
3. [Frontend Technologies](#3-frontend-technologies)
4. [Backend Technologies](#4-backend-technologies)
5. [Databases](#5-databases)
6. [Popular Tech Stacks](#6-popular-tech-stacks)
7. [MERN Stack Explained](#7-mern-stack-explained)
8. [MARN Stack Explained](#8-marn-stack-explained)
9. [DOM — Document Object Model](#9-dom--document-object-model)
10. [BOM — Browser Object Model](#10-bom--browser-object-model)
11. [Comparison Tables](#11-comparison-tables)
12. [Simple Diagrams](#12-simple-diagrams)
13. [Quick Revision](#13-quick-revision)

---

## 1. What is a Tech Stack?

> **Tech Stack** = the set of technologies used to build an application.

Think of it like a **recipe**. To make a cake, you need flour, eggs, sugar, and an oven. To build a web app, you need different tools — that combination is called a **tech stack**.

**Simple example:**
- A restaurant website uses:
  - HTML + CSS (looks nice)
  - JavaScript (buttons work)
  - Node.js (processes orders on server)
  - MongoDB (stores menu items)

All of these together = the **tech stack**.


## 2. Main Parts of a Tech Stack

Every tech stack is usually divided into these layers:

```
┌──────────────────────────────┐
│        Developer Tools       │  ← Git, VS Code, Docker
├──────────────────────────────┤
│           Hosting            │  ← Vercel, AWS, Netlify
├──────────────────────────────┤
│             API              │  ← Connects frontend ↔ backend
├──────────────────────────────┤
│           Backend            │  ← Server-side logic
├──────────────────────────────┤
│           Database           │  ← Stores all data
├──────────────────────────────┤
│          Frontend            │  ← What users see
└──────────────────────────────┘
```

| Part             | What it does                                    | Simple Example                  |
|------------------|-------------------------------------------------|---------------------------------|
| **Frontend**     | The visual part users interact with             | Buttons, forms, pages           |
| **Backend**      | Server logic, processes requests                | Checking your login password    |
| **Database**     | Stores all data permanently                     | Your user profile info          |
| **API**          | Messenger between frontend & backend            | "Send this request, get data"   |
| **Hosting**      | Puts your app on the internet                   | Vercel, AWS, Netlify            |
| **Dev Tools**    | Tools that help developers write code           | VS Code, Git, Postman           |


## 3. Frontend Technologies

> **Frontend** = everything the user can see and click on in the browser.

### 🔷 HTML — HyperText Markup Language
- **What:** The skeleton of a webpage
- **Used for:** Structure (headings, paragraphs, images, links)
- **Example:** `<h1>Hello World</h1>`

### 🎨 CSS — Cascading Style Sheets
- **What:** Styling language
- **Used for:** Making pages look beautiful (colors, fonts, layout)
- **Example:** `color: blue; font-size: 16px;`

### ⚡ JavaScript (JS)
- **What:** Programming language for browsers
- **Used for:** Making pages interactive (click events, animations, forms)
- **Example:** `document.getElementById("btn").addEventListener("click", ...)`

### ⚛️ React.js
- **What:** JavaScript library by Facebook/Meta
- **Used for:** Building fast, reusable UI components
- **Example:** Used by Instagram, Airbnb, Netflix

### 🔺 Next.js
- **What:** Framework built on top of React
- **Used for:** Server-side rendering (SSR), SEO-friendly React apps
- **Example:** Used for blogs, e-commerce, portfolios

### 💨 Tailwind CSS
- **What:** Utility-first CSS framework
- **Used for:** Quickly styling elements using class names
- **Example:** `<div class="text-blue-500 font-bold p-4">Hello</div>`

### 🅱️ Bootstrap
- **What:** CSS framework with pre-built components
- **Used for:** Quickly adding buttons, navbars, grids
- **Example:** `<button class="btn btn-primary">Click Me</button>`


## 4. Backend Technologies

> **Backend** = the server, the brain. Handles logic, databases, and security.

| Technology       | Language   | Used For                                        |
|------------------|------------|-------------------------------------------------|
| **Node.js**      | JavaScript | Running JS on server, fast non-blocking I/O     |
| **Express.js**   | JavaScript | Building REST APIs with Node.js (simple, fast)  |
| **Django**       | Python     | Full-featured web framework, batteries included |
| **Flask**        | Python     | Lightweight Python framework, easy to learn     |
| **Spring Boot**  | Java       | Enterprise-grade apps, used in banking/finance  |
| **NestJS**       | TypeScript | Structured, scalable Node.js backend framework  |

### Quick Notes:
- **Node.js + Express.js** → Most popular combo for JavaScript backend
- **Django** → Great when you need everything out of the box (admin panel, auth)
- **Flask** → Perfect for small APIs and beginners
- **Spring Boot** → Heavy duty, used by big companies
- **NestJS** → Angular-style architecture for backend


## 5. Databases

> **Database** = where all your data lives. Like a super-organized filing cabinet.

### Two Types of Databases:

```
Databases
├── SQL (Relational)      ← Data in tables (rows & columns)
│   ├── PostgreSQL
│   ├── MySQL
│   └── Supabase
│
└── NoSQL (Non-Relational) ← Data in documents, key-value, etc.
    ├── MongoDB
    ├── Firebase
    └── Redis
```

| Database         | Type     | Used For                                          |
|------------------|----------|---------------------------------------------------|
| **MongoDB**      | NoSQL    | Flexible JSON-like data, great with Node.js       |
| **PostgreSQL**   | SQL      | Complex queries, relations, enterprise apps       |
| **MySQL**        | SQL      | Most popular SQL database, web apps               |
| **Firebase**     | NoSQL    | Real-time apps, mobile apps, Google backend       |
| **Redis**        | NoSQL    | Super fast caching, sessions, temporary data      |
| **Supabase**     | SQL      | Open-source Firebase alternative using Postgres   |


## 6. Popular Tech Stacks

> A **named stack** is just a shortcut name for a common combination of tools.

| Stack Name      | Technologies                                     | Used For                              |
|-----------------|--------------------------------------------------|---------------------------------------|
| **MERN**        | MongoDB, Express, React, Node.js                 | Full-stack JS web apps                |
| **MEAN**        | MongoDB, Express, Angular, Node.js               | Full-stack JS with Angular UI         |
| **MARN**        | MongoDB, Angular, React, Node.js                 | Hybrid frontend (Angular + React)     |
| **LAMP**        | Linux, Apache, MySQL, PHP                        | Traditional web apps, WordPress       |
| **PERN**        | PostgreSQL, Express, React, Node.js              | Full-stack JS with relational DB      |
| **T3 Stack**    | TypeScript, tRPC, Tailwind, Prisma, Next.js      | Type-safe modern full-stack apps      |
| **Django Stack**| Django, PostgreSQL, HTML/CSS, Python             | Python-based full-stack web apps      |

---

## 7. MERN Stack Explained

> **MERN** = **M**ongoDB + **E**xpress.js + **R**eact.js + **N**ode.js

It is a **JavaScript-only** stack — you write JS everywhere (frontend + backend + database queries).

### Each Part Explained:

| Letter | Technology   | Role                                            |
|--------|--------------|-------------------------------------------------|
| **M**  | MongoDB      | Database — stores data as JSON-like documents   |
| **E**  | Express.js   | Backend framework — handles API routes          |
| **R**  | React.js     | Frontend — builds the UI users see              |
| **N**  | Node.js      | Runtime — runs JavaScript on the server         |

### How They Work Together:

```
[ User Browser ]
      |
      | (opens website)
      ▼
[ React.js ] ←── renders the UI ──→ User sees page
      |
      | (user clicks "Get Data" button)
      ▼
[ Express.js API ] ←── receives request from React
      |
      | (queries the database)
      ▼
[ Node.js + MongoDB ] ←── fetches/saves data
      |
      | (sends data back)
      ▼
[ React.js ] ←── updates the UI with new data
```

### Example Project: Simple To-Do App

**Step-by-step flow:**

1. **User** opens the app → **React** renders the to-do list page
2. **User** types a task and clicks "Add"
3. **React** sends a `POST` request to Express API: `/api/todos`
4. **Express** receives request → validates it
5. **Node.js** tells **MongoDB**: "Save this new to-do"
6. **MongoDB** saves it → returns success
7. **Express** sends response back to React
8. **React** updates the list on screen — user sees new task! ✅

### Why People Love MERN:
- ✅ One language (JavaScript) everywhere
- ✅ Huge community and lots of tutorials
- ✅ Fast for building modern web apps
- ✅ Great for startups and projects


## 8. MARN Stack Explained

> **MARN** = **M**ongoDB + **A**ngular + **R**eact + **N**ode.js

Wait... **Angular AND React together?** Yes! Some projects use Angular for some parts and React for others — or use them in a hybrid/experimental setup.

### Each Part:

| Letter | Technology | Role                                                |
|--------|------------|-----------------------------------------------------|
| **M**  | MongoDB    | Database — stores data                              |
| **A**  | Angular    | Frontend framework by Google (full, opinionated)    |
| **R**  | React      | Frontend library (flexible, component-based)        |
| **N**  | Node.js    | Server runtime                                      |

### Where MARN Can Be Used:
- Large enterprise applications with **mixed teams**
- **Micro-frontend** architecture (different teams use different frameworks)
- Migration projects (moving from Angular to React gradually)

### MERN vs MARN — Key Differences:

| Feature           | MERN                          | MARN                                 |
|-------------------|-------------------------------|--------------------------------------|
| Frontend          | React only                    | Angular + React (hybrid)             |
| Learning Curve    | Easier (one framework)        | Harder (two frontend frameworks)     |
| Team Size         | Small to medium teams         | Large teams / enterprise             |
| Use Case          | Startups, SPAs, web apps      | Large apps, micro-frontends          |
| Language          | JavaScript                    | JavaScript + TypeScript (Angular)    |
| Community         | Very large                    | Smaller (less common combo)          |

> 💡 **Tip:** MERN is much more commonly used. MARN is used in specific enterprise scenarios.


## 9. DOM — Document Object Model

### Full Form:
> **DOM** = **D**ocument **O**bject **M**odel

### What is the DOM?

When a browser loads an HTML page, it creates a **tree-like structure** in memory called the **DOM**. It represents every element on the page as an **object** that JavaScript can read and change.

> Think of the DOM as a **live map** of your webpage. JavaScript uses this map to find, change, or delete elements.

### Why is the DOM Used?
- 🔍 To **find** elements on the page (get a button, an image, etc.)
- ✏️ To **change** content (update text, images dynamically)
- 🗑️ To **add or remove** elements without reloading the page
- 🎯 To **respond to events** (clicks, typing, scrolling)

### DOM Tree Example:

```
Document
└── <html>
    ├── <head>
    │   └── <title> My Page </title>
    └── <body>
        ├── <h1> Hello World </h1>
        ├── <p> This is a paragraph </p>
        └── <div>
            ├── <button id="btn"> Click Me </button>
            └── <img src="logo.png" />
```

Every box above is a **node** in the DOM tree.

### Common DOM Methods:

| Method / Property                        | What it does                              |
|------------------------------------------|-------------------------------------------|
| `document.getElementById("id")`          | Get element by its ID                     |
| `document.querySelector(".class")`       | Get first matching CSS selector           |
| `document.querySelectorAll("tag")`       | Get all matching elements                 |
| `element.innerHTML`                      | Get or set HTML inside an element         |
| `element.textContent`                    | Get or set plain text inside an element   |
| `element.style.color = "red"`            | Change CSS style with JavaScript          |
| `element.classList.add("active")`        | Add a CSS class                           |
| `element.addEventListener("click", fn)`  | Listen for click events                   |
| `document.createElement("div")`          | Create a new element                      |
| `parent.appendChild(child)`              | Add a new element to the page             |
| `element.remove()`                       | Remove an element from the page           |

### JavaScript Examples:

**Example 1 — Change text on button click:**
```html
<h1 id="title">Hello!</h1>
<button onclick="changeText()">Click Me</button>

<script>
  function changeText() {
    document.getElementById("title").textContent = "You clicked the button!";
  }
</script>
```

**Example 2 — Change color:**
```javascript
// Select the element
let box = document.querySelector(".box");

// Change its background color
box.style.backgroundColor = "blue";
```

**Example 3 — Add a new item to a list:**
```javascript
let list = document.getElementById("myList");

let newItem = document.createElement("li");
newItem.textContent = "New Task Added!";

list.appendChild(newItem);
```


## 10. BOM — Browser Object Model

### Full Form:
> **BOM** = **B**rowser **O**bject **M**odel

### What is the BOM?

The BOM represents everything the **browser** provides to JavaScript — not just the page, but the browser **window** itself: the URL, history, screen size, popups, timers, and more.

> Think of BOM as JavaScript's control panel for the **browser** (not just the webpage).

### Why is the BOM Used?
- 🌐 To get or change the **current URL**
- 🔙 To navigate **back/forward** in browser history
- 📏 To get the **screen size** of the user's device
- ⏰ To run code **after a delay** (timers)
- 📢 To show **alerts, prompts, and confirm** boxes
- 🔔 To set and get **cookies or local storage**

### Common BOM Objects:

| Object              | What it does                                          |
|---------------------|-------------------------------------------------------|
| `window`            | The top-level object — represents the browser window  |
| `window.location`   | Get/change the current URL                            |
| `window.history`    | Go back/forward in browser history                    |
| `window.navigator`  | Info about the browser (name, version, OS)            |
| `window.screen`     | Info about the user's screen (width, height)          |
| `window.alert()`    | Show a popup alert box                                |
| `window.confirm()`  | Show a yes/no confirmation box                        |
| `window.prompt()`   | Ask user to type something                            |
| `setTimeout()`      | Run code once after a delay (in milliseconds)         |
| `setInterval()`     | Run code repeatedly at a set interval                 |

> 💡 `window` is the global object in browsers. You can write `alert()` instead of `window.alert()` — they're the same!

### JavaScript Examples:

**Example 1 — Redirect to another page:**
```javascript
window.location.href = "https://www.google.com";
```

**Example 2 — Go back to the previous page:**
```javascript
window.history.back();
```

**Example 3 — Get screen width:**
```javascript
let width = window.screen.width;
console.log("Screen width: " + width + "px");
```

**Example 4 — Show a confirm dialog:**
```javascript
let result = window.confirm("Are you sure you want to delete?");
if (result === true) {
  console.log("User clicked OK — delete it!");
} else {
  console.log("User clicked Cancel — safe!");
}
```

**Example 5 — Timer (run code after 3 seconds):**
```javascript
setTimeout(function() {
  alert("3 seconds have passed!");
}, 3000);
```

**Example 6 — Repeated timer (every 1 second):**
```javascript
setInterval(function() {
  console.log("This runs every second!");
}, 1000);
```


## 11. Comparison Tables

### 🔁 DOM vs BOM

| Feature          | DOM                                    | BOM                                       |
|------------------|----------------------------------------|-------------------------------------------|
| Full Form        | Document Object Model                  | Browser Object Model                      |
| What it controls | The HTML page (elements, content)      | The browser itself (URL, history, screen) |
| Main object      | `document`                             | `window`                                  |
| Examples         | `getElementById`, `querySelector`      | `location`, `history`, `navigator`        |
| Used for         | Manipulating page content & structure  | Controlling browser behavior              |
| Standard?        | W3C Standard                           | No official standard (browser-defined)    |


### 🔁 MERN vs MARN

| Feature           | MERN Stack                        | MARN Stack                             |
|-------------------|-----------------------------------|----------------------------------------|
| Full form         | MongoDB Express React Node        | MongoDB Angular React Node             |
| Frontend          | React.js only                     | Angular + React (both)                 |
| Languages         | JavaScript                        | JavaScript + TypeScript                |
| Learning curve    | Moderate                          | Steeper (two frontend frameworks)      |
| Common use        | Web apps, SPAs, startups          | Large enterprise / micro-frontends     |
| Community size    | Very large                        | Smaller                                |
| Best for          | Beginners and mid-level devs      | Advanced / enterprise developers       |


## 12. Simple Diagrams

### 🌐 How a Web Request Works (General)

```
        User
          |
          | (types URL or clicks button)
          ▼
       Browser
          |
          | (sends HTTP Request)
          ▼
      Frontend (React / HTML)
          |
          | (calls API)
          ▼
      Backend / API (Express / Node)
          |
          | (queries database)
          ▼
       Database (MongoDB / PostgreSQL)
          |
          | (returns data)
          ▼
      Backend sends Response
          |
          ▼
      Frontend updates UI
          |
          ▼
        User sees result ✅
```


### ⚛️ MERN Stack Request Flow

```
[ Browser ]
    |
    ▼
[ React.js ]  ──── shows UI to user
    |
    | API Call (fetch / axios)
    ▼
[ Express.js ]  ──── handles route (e.g. GET /users)
    |
    ▼
[ Node.js ]  ──── runs server, processes logic
    |
    ▼
[ MongoDB ]  ──── stores/retrieves data
    |
    ▼
[ Node.js → Express → React → User ] ✅
```


### 🗂️ DOM Tree (Visual)

```
       document
           |
          html
         /    \
       head    body
        |      /  \
      title   h1   div
              |      \
           "Hello"   button
                       |
                    "Click Me"
```


### 🖥️ BOM Structure

```
       window  (top-level)
      /  |  |  \  \
location history navigator screen  setTimeout / setInterval
    |        |        |        |
  URL     back()  browser   width/height
         forward()  info
```


## 13. Quick Revision

> ⚡ Read this before your exam or interview!

### 🔑 Key Terms

| Term         | One-line Definition                                                   |
|--------------|-----------------------------------------------------------------------|
| Tech Stack   | Combination of tools/technologies used to build an app               |
| Frontend     | What users see (HTML, CSS, JavaScript, React)                        |
| Backend      | Server-side logic (Node.js, Express, Django)                         |
| Database     | Stores data (MongoDB, MySQL, PostgreSQL)                             |
| API          | Messenger between frontend and backend                               |
| DOM          | Tree structure of the HTML page; manipulated by JavaScript           |
| BOM          | Browser-level objects like URL, history, screen                      |
| MERN         | MongoDB + Express + React + Node.js                                  |
| MARN         | MongoDB + Angular + React + Node.js (hybrid frontend)                |


### ✅ Quick True/False Check

- JavaScript can change the DOM → **True** ✅
- BOM deals with HTML elements → **False** ❌ (that's DOM)
- MERN uses Python → **False** ❌ (it's JavaScript only)
- MongoDB is a NoSQL database → **True** ✅
- React is a backend framework → **False** ❌ (it's frontend)
- `window.location` is part of BOM → **True** ✅
- Express.js runs on Node.js → **True** ✅


### 📝 Common DOM Methods to Remember

```
getElementById()     → find by ID
querySelector()      → find by CSS selector
innerHTML            → change HTML content
textContent          → change text content
addEventListener()   → attach event (click, keyup, etc.)
createElement()      → make a new element
appendChild()        → add element to page
remove()             → delete element from page
```


### 📝 Common BOM Objects to Remember

```
window.location.href   → current URL
window.history.back()  → go back
window.screen.width    → screen width
window.alert()         → show popup
setTimeout()           → run code after delay
setInterval()          → run code repeatedly
window.navigator       → browser info
```


### 🧠 Remember These Stacks:

```
MERN  = Mongo + Express + React + Node    ✅ Most popular JS stack
MEAN  = Mongo + Express + Angular + Node  ✅ Angular instead of React
MARN  = Mongo + Angular + React + Node    🔷 Hybrid / enterprise
LAMP  = Linux + Apache + MySQL + PHP      🔷 Old school, still used
PERN  = Postgres + Express + React + Node ✅ SQL version of MERN
```


> 💬 **Remember:** Every big app — Instagram, Airbnb, Netflix — uses a tech stack. You are learning to build the same thing, step by step. Keep going! 🚀
