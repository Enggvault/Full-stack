# HTML, CSS, JS & API Fundamentals

> **Day 1 | Full Stack Development**
> Structured notes covering HTML, CSS, JavaScript, APIs, JSON, Fetch API, and Async JS.


## Table of Contents

1. [Introduction to Web Development](#1-introduction-to-web-development)
2. [HTML Notes](#2-html-notes)
3. [CSS Notes](#3-css-notes)
4. [JavaScript Notes](#4-javascript-notes)
5. [API Notes](#5-api-notes)
6. [JSON Notes](#6-json-notes)
7. [Fetch API Notes](#7-fetch-api-notes)
8. [Async JavaScript Notes](#8-async-javascript-notes)
9. [Mini Project](#9-mini-project)
10. [Practice Questions](#10-practice-questions)

---

## 1. Introduction to Web Development

Web development is the process of building websites and applications for the internet. A web application is typically built using three core languages that work together to create the experience you see on your screen.

### Analogy — Building a House

```
+---------------------------+      +---------------------------+
|   Building a House        |      |   Building a Web App      |
+---------------------------+      +---------------------------+
| Bricks, walls, foundation | ---> | HTML (Structure)          |
| Paint, decorations, style | ---> | CSS (Design)              |
| Electricity, plumbing     | ---> | JavaScript (Behavior)     |
+---------------------------+      +---------------------------+
```

| Technology   | Role          | Responsibility                                           |
|--------------|---------------|----------------------------------------------------------|
| **HTML**     | Structure     | Gives the webpage its basic structure and content.       |
| **CSS**      | Design        | Makes the webpage look beautiful and visually appealing. |
| **JavaScript**| Behavior     | Makes the webpage interactive (e.g., clicking a button). |


## 2. HTML Notes

### What is HTML?

- **HTML** stands for **HyperText Markup Language**.
- It is the standard language used to create the structure of a webpage.
- It uses "tags" to wrap content and tell the browser how to display it.

### Basic HTML Document Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>My First Webpage</title>
</head>
<body>
    <h1>Welcome to my website!</h1>
    <p>This is a paragraph of text.</p>
</body>
</html>
```

### HTML Elements Reference

| Element Type       | Example Tags                  | Description                                      |
|--------------------|-------------------------------|--------------------------------------------------|
| **Headings**       | `<h1>` to `<h6>`              | Headings from largest (`h1`) to smallest (`h6`). |
| **Text content**   | `<p>`, `<br>`                 | Paragraph blocks and line breaks.                |
| **Lists**          | `<ul>`, `<ol>`, `<li>`        | Unordered (bullets), Ordered (numbers) lists.    |
| **Links**          | `<a href="url">`              | Used to navigate to other pages or websites.     |
| **Images**         | `<img src="img.jpg" alt="">`  | Used to display pictures.                        |

### Semantic Tags

Semantic tags clearly describe their meaning to both the browser and the developer. They help with accessibility and SEO.

```
+--------------------------------------------------+
|                   <header>                       |
+--------------------------------------------------+
|                    <nav>                         |
+--------------------------------------------------+
|                                                  |
|                   <main>                         |
|                                                  |
|   +------------------------------------------+   |
|   |              <article>                   |   |
|   +------------------------------------------+   |
|                                                  |
+--------------------------------------------------+
|                   <footer>                       |
+--------------------------------------------------+
```

### Forms and Inputs

Forms are used to collect user input.

```html
<form>
    <label for="username">Username:</label>
    <input type="text" id="username" name="username">
    
    <label for="password">Password:</label>
    <input type="password" id="password" name="password">
    
    <button type="submit">Login</button>
</form>
```


## 3. CSS Notes

### What is CSS?

- **CSS** stands for **Cascading Style Sheets**.
- It controls the visual presentation of HTML elements (colors, fonts, spacing, layout).

### Types of CSS

| Type         | Description                                          | Example                                       |
|--------------|------------------------------------------------------|-----------------------------------------------|
| **Inline**   | Styles applied directly inside the HTML tag.         | `<h1 style="color: blue;">Heading</h1>`       |
| **Internal** | Styles inside a `<style>` block in the `<head>`.     | `<style> h1 { color: blue; } </style>`        |
| **External** | Styles in a separate `.css` file. (Best practice).   | `<link rel="stylesheet" href="styles.css">`   |

### Selectors Reference

```css
/* Element Selector */
p { color: red; } 

/* Class Selector (Prefix with a dot) */
.my-button { font-size: 16px; }

/* ID Selector (Prefix with a hash) */
#main-header { background: black; }
```

### The Box Model

Every HTML element is essentially a box.

```
+-----------------------------------+
|              Margin               |
|  +-----------------------------+  |
|  |           Border            |  |
|  |  +-----------------------+  |  |
|  |  |        Padding        |  |  |
|  |  |  +-----------------+  |  |  |
|  |  |  |     Content     |  |  |  |
|  |  |  +-----------------+  |  |  |
|  |  +-----------------------+  |  |
|  +-----------------------------+  |
+-----------------------------------+
```

### Layouts & Positioning

| Concept          | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| **Flexbox**      | 1-dimensional layout (row or column). Great for alignment inside containers.|
| **Grid**         | 2-dimensional layout (rows and columns). Great for overall page structure.  |
| **static**       | Default positioning. Flows naturally down the page.                         |
| **relative**     | Positioned relative to its normal static position.                          |
| **absolute**     | Positioned relative to its closest positioned ancestor.                     |
| **fixed**        | Positioned relative to the browser window (stays on scroll).                |
| **sticky**       | Toggles between relative and fixed depending on scroll position.            |

### Responsive Design

Use **Media Queries** to apply different CSS rules based on screen size.

```css
body { font-size: 14px; }

@media (min-width: 768px) {
    body { font-size: 18px; }
}
```


## 4. JavaScript Notes

### What is JavaScript?

JavaScript (JS) is a programming language that allows you to implement complex features and interactions on web pages.

### Variables

| Keyword   | Scope           | Can be Reassigned? | Notes                                      |
|-----------|-----------------|--------------------|--------------------------------------------|
| `let`     | Block-scoped    | Yes                | Standard way to declare variables today.   |
| `const`   | Block-scoped    | No                 | Used for variables that will never change. |
| `var`     | Function-scoped | Yes                | Old way. Avoid using in modern JS.         |

### Data Types

| Type          | Example                          | Description                               |
|---------------|----------------------------------|-------------------------------------------|
| **String**    | `"Hello World"`                  | Text data.                                |
| **Number**    | `42`, `3.14`                     | Numeric data.                             |
| **Boolean**   | `true`, `false`                  | True or false logic.                      |
| **Null**      | `null`                           | Intentional absence of any value.         |
| **Undefined** | `undefined`                      | Variable declared but no value assigned.  |

### Arrays and Objects

```javascript
// Array (ordered list)
const colors = ["red", "green", "blue"];

// Object (key-value pairs)
const user = {
    firstName: "John",
    age: 30
};
```

### DOM Manipulation & Events

```html
<button id="myButton">Click Me!</button>
<p id="message"></p>

<script>
  const btn = document.getElementById("myButton");
  const msg = document.getElementById("message");

  btn.addEventListener("click", function() {
      msg.textContent = "Button was clicked!";
  });
</script>
```

### localStorage

```javascript
localStorage.setItem("theme", "dark");
let currentTheme = localStorage.getItem("theme");
localStorage.removeItem("theme");
```


## 5. API Notes

### What is an API?

API stands for **Application Programming Interface**. It allows your frontend application to communicate with backend servers or third-party services.

### Frontend and Backend Communication

```
+----------------+            +----------------+            +----------------+
|    Frontend    |            |       API      |            |    Backend     |
| (You/Customer) |  Request   |    (Waiter)    |  Query     |   (Kitchen)    |
|                | ---------> |                | ---------> |                |
| Asks for data  |            | Carries request|            | Processes data |
|                | <--------- |                | <--------- |                |
| Displays UI    |  Response  | Brings food    |  Result    | Sends DB data  |
+----------------+            +----------------+            +----------------+
```

### Real-life Examples

| Use Case                        | API Function                                         |
|---------------------------------|------------------------------------------------------|
| **Weather App**                 | Fetches current forecast from a remote weather server|
| **Google/Facebook Login**       | Verifies user identity with secure auth servers      |
| **E-commerce Payments**         | Securely processes credit cards (e.g., Stripe API)   |


## 6. JSON Notes

### What is JSON?

JSON stands for **JavaScript Object Notation**. It is the standard text-based format for storing and transporting data across the web.

### JSON vs JavaScript Object

| Feature           | JavaScript Object                          | JSON                                       |
|-------------------|--------------------------------------------|--------------------------------------------|
| **Format**        | Code running in memory                     | Pure string of text                        |
| **Quotes**        | Keys do not require quotes                 | **Keys MUST be wrapped in double quotes**  |
| **Functions**     | Can contain functions as values            | Cannot contain functions                   |
| **Use Case**      | Logic and state within an app              | Data transfer between client and server    |

### Example JSON

```json
{
    "user": {
        "id": 1,
        "name": "Leanne Graham",
        "isActive": true,
        "hobbies": ["coding", "reading"]
    }
}
```


## 7. Fetch API Notes

### What is Fetch?

The `fetch()` method is a built-in JavaScript way to make network requests to a server.

### Basic Fetch Request

```javascript
fetch('https://jsonplaceholder.typicode.com/users')
  .then(response => {
      // Step 1: Convert raw network response to JSON
      return response.json(); 
  })
  .then(data => {
      // Step 2: Use the JavaScript data object
      console.log(data);
  });
```

### Error Handling

```javascript
fetch('https://jsonplaceholder.typicode.com/users')
  .then(response => {
      if (!response.ok) {
          throw new Error('Network response was not ok');
      }
      return response.json();
  })
  .then(users => console.log(users))
  .catch(error => console.error('Fetch failed:', error));
```


## 8. Async JavaScript Notes

### Synchronous vs Asynchronous

| Paradigm       | Behavior                                                       | Analogy                             |
|----------------|----------------------------------------------------------------|-------------------------------------|
| **Synchronous**| Code runs line-by-line. Blocks execution until done.           | Waiting in a single checkout line.  |
| **Asynchronous**| Starts a task, moves on, handles result when ready (non-blocking)| Taking a pager at a restaurant.     |

**Why is async important?** API calls take time. If JavaScript was synchronous, the browser would freeze while waiting for data.

### `async` / `await` Flow

```javascript
async function fetchUsers() {
    try {
        // Pauses execution of THIS function until fetch completes
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        
        if (!response.ok) {
            throw new Error('Failed to fetch data');
        }
        
        // Pauses again to parse JSON
        const data = await response.json(); 
        console.log(data);
        
    } catch (error) {
        // Catch network or custom errors
        console.error("An error occurred:", error);
    }
}
```


## 9. Mini Project

### Project: API User Card App

**Goal:** Create a web app that fetches user data and displays it beautifully in cards.

| Requirement       | Details                                                                       |
|-------------------|-------------------------------------------------------------------------------|
| **Tech Stack**    | HTML, CSS, JavaScript                                                         |
| **API Endpoint**  | `https://jsonplaceholder.typicode.com/users`                                  |
| **UI Layout**     | CSS Grid/Flexbox to display user data as cards (Name, Email, Company).        |
| **Loading State** | Show a "Loading..." message before data arrives.                              |
| **Error Handling**| Show a user-friendly error message on the screen if the fetch fails.          |
| **Search Feature**| Input box that filters the displayed cards by user name in real-time.         |
| **Responsiveness**| Use Media Queries so cards stack on mobile but form a grid on desktop.        |


## 10. Practice Questions

### Theory Questions

| #  | Question                                                                                          |
|----|---------------------------------------------------------------------------------------------------|
| 1  | Explain the distinct roles of HTML, CSS, and JavaScript in a webpage.                             |
| 2  | What is the purpose of semantic HTML tags? Provide three examples.                                |
| 3  | Explain the CSS Box Model and its four main components.                                           |
| 4  | What is the difference between Flexbox and CSS Grid? When would you use each?                     |
| 5  | What is the difference between `let`, `const`, and `var` in JavaScript?                           |
| 6  | Explain what the DOM is and how JavaScript interacts with it.                                     |
| 7  | In your own words, describe the relationship between a Frontend, an API, and a Backend.           |
| 8  | Why do keys in JSON format require double quotes, unlike JavaScript objects?                      |
| 9  | Why is Asynchronous JavaScript necessary for web development?                                     |
| 10 | What is the advantage of using `async/await` over `.then()/.catch()` Promise chains?              |

### Coding Practice Tasks

| #  | Task Description                                                                                  |
|----|---------------------------------------------------------------------------------------------------|
| 1  | Create a simple HTML form with fields for Name, Email, Password, and a Submit button.             |
| 2  | Style an HTML `<div>` to look like a button with a hover effect (changes background color).       |
| 3  | Write a JavaScript function that takes two numbers as arguments and returns their sum.            |
| 4  | Write a `for` loop that prints the numbers 1 to 10 in the console.                                |
| 5  | Create an array of your top 3 favorite movies. Write a loop to print each movie name.             |
| 6  | Create a JS object representing a car (make, model, year). Log the car's model to the console.    |
| 7  | Create an HTML file with an empty `<ul>`. Write JavaScript to add three `<li>` elements dynamically.|
| 8  | Create a button in HTML. Write JS to show an `alert("Hello!")` when the button is clicked.        |
| 9  | Write a `fetch` request to `https://jsonplaceholder.typicode.com/posts/1` and log the JSON.       |
| 10 | Rewrite the fetch request from Task 9 using an `async` function and the `await` keyword.          |

### Final Assignment

**Build a Random Advice Generator:**
1. Create an HTML layout with a heading, text area for advice, and a "Get New Advice" button.
2. Style it nicely with CSS (center it, add nice fonts, shadows).
3. Use JavaScript and `fetch` to get a random quote from `https://api.adviceslip.com/advice`.
4. Handle loading states ("Thinking...") and network errors using a `try/catch` block.
