# HTML, CSS, JS & API Fundamentals

> **Day 3 | Full Stack Development**
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


## 1. Introduction to Web Development

Web development is the process of building websites and applications for the internet. Whenever you open a website like YouTube or Amazon, you are looking at a **Web Application**. 

Every website is built using three core languages that work together to create the final experience you see on your screen. If you master these three, you can build almost any website!

### Analogy — Building a House

Imagine you are building a new house. You can't start with the paint; you need a structure first.

```
+---------------------------+      +---------------------------+
|   Building a House        |      |   Building a Web App      |
+---------------------------+      +---------------------------+
| Bricks, walls, foundation | ---> | HTML (Structure)          |
| Paint, decorations, style | ---> | CSS (Design)              |
| Electricity, plumbing     | ---> | JavaScript (Behavior)     |
+---------------------------+      +---------------------------+
```

| Technology    | Role          | Responsibility                                              |
|---------------|---------------|-------------------------------------------------------------|
| **HTML**      | Structure     | Gives the webpage its basic structure, text, and images.    |
| **CSS**       | Design        | Makes the webpage look beautiful (colors, layout, fonts).   |
| **JavaScript**| Behavior      | Makes the webpage interactive (button clicks, popups).      |


## 2. HTML Notes

### What is HTML?

- **HTML** stands for **HyperText Markup Language**.
- Think of it as the **skeleton** of your webpage. Without it, there is no webpage.
- It uses special words wrapped in angle brackets called **"tags"** (like `<h1>`) to tell the browser how to display content.

### Basic HTML Document Structure

```html
<!DOCTYPE html>     <!-- Tells the browser this is a modern HTML5 document -->
<html lang="en">    <!-- The root of the document, language set to English -->
<head>
    <meta charset="UTF-8">
    <title>My First Webpage</title>   <!-- Shows up in the browser tab -->
</head>
<body>
    <!-- Everything inside <body> is what the user sees on the screen -->
    <h1>Welcome to my website!</h1>
    <p>This is a paragraph of text.</p>
</body>
</html>
```

### HTML Elements Reference

> Most tags come in pairs: an opening tag `<p>` and a closing tag `</p>` with a slash.

| Element Type       | Example Tags                  | Description                                                       |
|--------------------|-------------------------------|-------------------------------------------------------------------|
| **Headings**       | `<h1>` to `<h6>`              | `<h1>` is the largest, most important title. `<h6>` is the smallest. |
| **Text content**   | `<p>`, `<br>`                 | `<p>` is for paragraph text. `<br>` forces a line break.          |
| **Lists**          | `<ul>`, `<ol>`, `<li>`        | `<ul>` = bulleted list. `<ol>` = numbered list. `<li>` = list item.|
| **Links**          | `<a href="url">`              | Turns text into a clickable link to another page.                 |
| **Images**         | `<img src="img.jpg" alt="">`  | Displays an image. `src` = file path, `alt` = text for screen readers.|

### Semantic Tags

In the past, developers used `<div>` (a generic container) for everything. Today, we use **Semantic Tags**. These tags have clear, human-readable names that tell the browser *exactly* what type of content is inside them.

> **Why Semantic HTML?** Better **SEO** (Google searches) and better **Accessibility** for screen readers.

```
+--------------------------------------------------+
|      <header>  (Website logo and main title)     |
+--------------------------------------------------+
|      <nav>     (Navigation menu / Home / About)  |
+--------------------------------------------------+
|                                                  |
|                   <main>                         |
|           (The core content of the page)         |
|                                                  |
|   +------------------------------------------+  |
|   |              <article>                   |  |
|   |       (A standalone blog post or news)   |  |
|   +------------------------------------------+  |
|                                                  |
+--------------------------------------------------+
|      <footer>  (Copyright info, bottom links)    |
+--------------------------------------------------+
```

### Forms and Inputs

Forms are how websites **collect information** from users (like a login or a contact page).

```html
<form>
    <!-- The label tells the user what to type -->
    <label for="username">Username:</label>
    <!-- The input is the actual text box -->
    <input type="text" id="username" name="username">
    
    <label for="password">Password:</label>
    <input type="password" id="password" name="password">
    
    <!-- The button submits the form data -->
    <button type="submit">Login</button>
</form>
```

---

## 3. CSS Notes

### What is CSS?

- **CSS** stands for **Cascading Style Sheets**.
- If HTML is the skeleton, CSS is the **skin and clothing**. It controls colors, fonts, spacing, and layouts.

### Types of CSS

| Type         | Description                                          | Example                                       |
|--------------|------------------------------------------------------|-----------------------------------------------|
| **Inline**   | Styles applied directly inside the HTML tag. (Messy) | `<h1 style="color: blue;">Heading</h1>`       |
| **Internal** | Styles inside a `<style>` block in the `<head>`.     | `<style> h1 { color: blue; } </style>`        |
| **External** | Styles in a separate `.css` file. (**Best practice**)| `<link rel="stylesheet" href="styles.css">`   |

### Selectors Reference

Selectors tell CSS *which* HTML elements you want to style.

```css
/* 1. Element Selector: Targets EVERY <p> tag on the page */
p { color: red; }

/* 2. Class Selector: Targets elements with class="my-button" (use a dot) */
/* Classes can be reused on many different elements! */
.my-button { font-size: 16px; }

/* 3. ID Selector: Targets ONE specific element with id="main-header" (use a hash) */
/* IDs must be UNIQUE. Only one per page! */
#main-header { background: black; }
```

### The Box Model

> **Most important concept in CSS!** Every HTML element is essentially a rectangular box.

```
+-------------------------------------------+
|                   Margin                  |   ← Invisible outer space (pushes elements apart)
|  +-------------------------------------+  |
|  |              Border                 |  |   ← Physical visible line
|  |  +-------------------------------+  |  |
|  |  |           Padding             |  |  |   ← Inner space (between border and content)
|  |  |  +-------------------------+  |  |  |
|  |  |  |                         |  |  |  |
|  |  |  |       CONTENT           |  |  |  |   ← Actual text / image
|  |  |  |                         |  |  |  |
|  |  |  +-------------------------+  |  |  |
|  |  +-------------------------------+  |  |
|  +-------------------------------------+  |
+-------------------------------------------+
```

### Layouts & Positioning

How do we arrange boxes on the screen?

| Concept          | Description (When to use it)                                                           |
|------------------|---------------------------------------------------------------------------------------|
| **Flexbox**      | Arranges items in a single **row OR column**. Perfect for navigation menus.            |
| **Grid**         | Arranges items in **rows AND columns** (like a checkerboard). Great for galleries.    |
| **`static`**     | Default. Elements just stack naturally down the page.                                  |
| **`relative`**   | Allows nudging an element slightly away from its normal position.                      |
| **`absolute`**   | Completely removes the element from normal flow. Placed exactly where you want.        |
| **`fixed`**      | Locks the element to the browser screen. Won't move even when scrolling.               |

### Responsive Design

Websites need to look good on massive monitors and tiny smartphones. We use **Media Queries** to change CSS rules based on screen size.

```css
/* Default styles apply to mobile phones first (Mobile-first approach) */
body { font-size: 14px; }

/* If the screen is wider than 768px (tablet/laptop), use this font size */
@media (min-width: 768px) {
    body { font-size: 18px; }
}
```

---

## 4. JavaScript Notes

### What is JavaScript?

JavaScript (JS) is the **brain of the webpage**. It allows you to implement complex features, do math, hide/show elements, and fetch data from the internet.

### Variables

Variables are like **labeled boxes** where we store data so we can use it later.

| Keyword   | Scope           | Can be Reassigned? | Notes                                        |
|-----------|-----------------|--------------------|----------------------------------------------|
| **`let`** | Block-scoped    | Yes                | Use when a value might change (e.g., a score).|
| **`const`**| Block-scoped   | No                 | Use for values that will **NEVER** change.    |
| **`var`** | Function-scoped | Yes                | Old way. **Avoid using it today!**            |

### Data Types

What kind of data can we store in our variables?

| Type          | Example                          | Description                               |
|---------------|----------------------------------|-------------------------------------------|
| **String**    | `"Hello World"`                  | Text data, always wrapped in quotes.      |
| **Number**    | `42`, `3.14`                     | Math numbers (no quotes needed).          |
| **Boolean**   | `true`, `false`                  | Simple yes/no logic.                      |
| **Null**      | `null`                           | You explicitly set a variable to be empty.|
| **Undefined** | `undefined`                      | Variable exists but has no value yet.     |

### Arrays and Objects

```javascript
// Array: An ordered list of items (like a grocery list).
// Items are accessed by their position (starting at 0).
const colors = ["red", "green", "blue"];
console.log(colors[0]); // Prints "red"

// Object: A collection of key-value pairs (like a dictionary).
// Great for representing real-world things.
const user = {
    firstName: "John",
    age: 30
};
console.log(user.firstName); // Prints "John"
```

### DOM Manipulation & Events

The **DOM** (Document Object Model) is how JavaScript sees your HTML. It turns your HTML tags into JavaScript objects so you can change them on the fly!

```html
<!-- The HTML -->
<button id="myButton">Click Me!</button>
<p id="message"></p>

<script>
  // 1. Grab the HTML elements and store them in JS variables
  const btn = document.getElementById("myButton");
  const msg = document.getElementById("message");

  // 2. Add an 'Event Listener'. Tells JS to listen for a specific action (like a 'click').
  btn.addEventListener("click", function() {
      // 3. When clicked, change the text inside the paragraph!
      msg.textContent = "Button was clicked!";
  });
</script>
```

### localStorage

Need to save user preferences (like Dark Mode) even if they close the browser? Use `localStorage`.

```javascript
// Save data to the browser
localStorage.setItem("theme", "dark");

// Retrieve the data later
let currentTheme = localStorage.getItem("theme");

// Delete the data
localStorage.removeItem("theme");
```

---

## 5. API Notes

### What is an API?

**API** stands for **Application Programming Interface**. It is a **secure bridge** that allows your frontend (what the user sees) to talk to backend servers (where databases live).

### 🍽️ Frontend and Backend Communication

Think of an API exactly like a **waiter in a restaurant**:

```
+----------------+            +----------------+            +----------------+
|    Frontend    |            |       API      |            |    Backend     |
| (You/Customer) |  Request   |    (Waiter)    |  Query     |   (Kitchen)    |
|                | ---------> |                | ---------> |                |
| Looks at Menu, |            | Carries your   |            | Cooks the food |
| asks for food  | <--------- | order to back  | <--------- | (Gets the data)|
|                |  Response  | Brings food    |  Result    |                |
+----------------+            +----------------+            +----------------+
```

### Real-life Examples

| Use Case                        | How the API helps                                    |
|---------------------------------|------------------------------------------------------|
| **Weather App**                 | Your phone asks a remote weather server: "What is the temp in London?" |
| **Google/Facebook Login**       | An app asks Google: "Is this user's password correct?" Google replies Yes/No.|
| **E-commerce Payments**         | A store sends your card details to Stripe's API. Stripe processes it and replies. |

---

## 6. JSON Notes

### What is JSON?

**JSON** stands for **JavaScript Object Notation**. When APIs send data across the internet, they need a universal language that all computers understand. That language is **JSON**. It is purely **text-based**.

### JSON vs JavaScript Object

They look similar, but have strict differences:

| Feature           | JavaScript Object                          | JSON                                       |
|-------------------|--------------------------------------------|---------------------------------------------|
| **Where it lives**| Lives inside active code memory            | It is just a string of text                |
| **Quotes Rule**   | Keys like `name` don't need quotes         | **Keys MUST be in double quotes**          |
| **Functions**     | Can contain functions                      | Cannot contain functions (just data)       |
| **Primary Use**   | Using data inside your app                 | Transferring data **over the internet**    |

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

---

## 7. Fetch API Notes

### What is Fetch?

`fetch()` is the tool built into JavaScript that acts as our **"waiter"**. We use it to make **network requests** to an API. It returns a **Promise**.

### Basic Fetch Request

When we ask for data, the API replies with raw text. We convert it into a usable JavaScript object using `.json()`.

```javascript
// Step 1: Call the API URL
fetch('https://jsonplaceholder.typicode.com/users')
  .then(response => {
      // Step 2: Convert raw data to JSON
      return response.json();
  })
  .then(data => {
      // Step 3: Now we have a usable JavaScript array of users!
      console.log(data);
  });
```

### Error Handling

What if the internet is down? We must handle errors gracefully using `.catch()`.

```javascript
fetch('https://jsonplaceholder.typicode.com/users')
  .then(response => {
      if (!response.ok) {
          throw new Error('Network response was not ok');
      }
      return response.json();
  })
  .then(users => console.log("Here are the users:", users))
  .catch(error => console.error('Uh oh, the fetch failed:', error));
```

---

## 8. Async JavaScript Notes

### Synchronous vs Asynchronous

| Paradigm        | Behavior                                                       | Analogy                             |
|-----------------|----------------------------------------------------------------|--------------------------------------|
| **Synchronous** | Code runs strictly line-by-line. If line 1 takes 5 seconds, line 2 MUST wait. | Waiting in a single checkout line. |
| **Asynchronous**| Starts a task, moves on immediately, handles result whenever it finishes. | Taking a buzzer at a restaurant. You can chat while waiting for food. |

> **Why is async important?** Fetching data from an API takes a few seconds. If JavaScript was purely Synchronous, your entire webpage would **freeze** while waiting for data!

### `async` / `await` Flow

The **modern, cleanest way** to write Asynchronous code. It makes async code read like a normal, top-to-bottom book.

```javascript
// Adding the word 'async' unlocks the magic 'await' keyword inside
async function fetchUsers() {
    try {
        // 'await' tells JS: "Pause THIS function until fetch finishes,
        //  but keep the rest of the website running normally."
        const response = await fetch('https://jsonplaceholder.typicode.com/users');

        if (!response.ok) {
            throw new Error('Failed to fetch data');
        }

        // Wait again to parse the JSON
        const data = await response.json();
        console.log("Success! Data:", data);

    } catch (error) {
        // If ANYTHING goes wrong above, code immediately jumps here
        console.error("An error occurred:", error);
    }
}
```

### Async Flow Diagram

```
async function fetchUsers() is called
          |
          v
     await fetch(url)          ← JS pauses this function, but NOT the page
          |
          |  (Network request in background...)
          |
          v
   Response received
          |
          v
     await response.json()     ← JS pauses again to parse JSON
          |
          v
    data is ready!
          |
          v
   console.log(data) / Update DOM
```

---

## 9. Mini Project

### Project: API User Card App

**Goal:** Create a web app that fetches user data and displays it beautifully in cards.

| Requirement         | Details                                                                       |
|---------------------|-------------------------------------------------------------------------------|
| **Tech Stack**      | HTML, CSS, JavaScript.                                                        |
| **API Endpoint**    | `https://jsonplaceholder.typicode.com/users` (fake user data)                |
| **UI Layout**       | Use CSS Grid or Flexbox to display data as cards (Name, Email, Company).     |
| **Loading State**   | Display a "Loading..." message before the data arrives.                       |
| **Error Handling**  | Show a red, user-friendly error message on the screen if the internet fails.  |
| **Search Feature**  | Add an input box. As the user types a name, filter the cards in real-time.   |
| **Responsiveness**  | Cards stack in a single column on mobile, 3-column grid on desktop.          |

---

## 10. Practice Questions

### Theory Questions

| #  | Question                                                                                          |
|----|---------------------------------------------------------------------------------------------------|
| 1  | Explain the distinct roles of HTML, CSS, and JavaScript as if explaining to a 10-year-old.        |
| 2  | What is the purpose of Semantic HTML tags? Provide three examples.                                |
| 3  | Explain the CSS Box Model and list its four main components.                                      |
| 4  | What is the primary difference between **Flexbox** and **CSS Grid**?                              |
| 5  | What is the difference between `let` and `const` in JavaScript? Why avoid `var`?                  |
| 6  | Explain what the **DOM** is and how JavaScript interacts with it.                                 |
| 7  | In your own words, describe the relationship between a Frontend, an API, and a Backend.           |
| 8  | Why do keys in **JSON** format require double quotes, unlike standard JavaScript objects?         |
| 9  | Why is **Asynchronous JavaScript** (like Promises or async/await) necessary for web development?  |
| 10 | What is the advantage of using `async/await` syntax over `.then()` Promise chains?                |

### Coding Practice Tasks

| #  | Task Description                                                                                  |
|----|---------------------------------------------------------------------------------------------------|
| 1  | **HTML**: Create a simple HTML form with fields for Name, Email, Password, and a Submit button.   |
| 2  | **CSS**: Style an HTML `<div>` to look like a button with a hover effect (changes background color).|
| 3  | **JS Basics**: Write a function that takes two numbers as arguments and returns their sum.        |
| 4  | **JS Loops**: Write a `for` loop that prints the numbers 1 to 10 in the console.                  |
| 5  | **JS Arrays**: Create an array of your top 3 favorite movies. Write a loop to print each movie.  |
| 6  | **JS Objects**: Create an object representing a car (make, model, year). Log the car's model.    |
| 7  | **DOM**: Create an HTML file with an empty `<ul>`. Write JavaScript to add three `<li>` elements dynamically.|
| 8  | **Events**: Create a button in HTML. Write JS to show an `alert("Hello!")` when clicked.          |
| 9  | **Fetch**: Write a `fetch` request to `https://jsonplaceholder.typicode.com/posts/1` and log the JSON.|
| 10 | **Async**: Rewrite the fetch request from Task 9 using an `async` function and the `await` keyword.|

### Final Assignment

**Build a Random Advice Generator:**

1. **HTML**: Create a layout with a heading, a large text area for advice, and a "Get New Advice" button.
2. **CSS**: Style it — center on the screen, add nice fonts, and maybe a shadow behind the text box.
3. **JS API**: Use JavaScript and `fetch` to get a random quote from: `https://api.adviceslip.com/advice`
4. **JS Logic**: Handle loading states (show "Thinking...") and handle network errors using a `try/catch` block.
