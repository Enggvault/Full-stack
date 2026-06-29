# The Complete JavaScript Reference Guide

> **JavaScript (JS)** is a lightweight, cross-platform, interpreted programming language — the **language of the web**. It adds behavior and interactivity to static HTML/CSS pages.

---

## Table of Contents
1. [Introduction](#1-introduction)
2. [JS Versions (ECMAScript)](#2-js-versions-ecmascript)
3. [Ways to Add JavaScript](#3-ways-to-add-javascript)
4. [Syntax Basics](#4-syntax-basics)
5. [Variables](#5-variables)
6. [Data Types](#6-data-types)
7. [Operators](#7-operators)
8. [Strings](#8-strings)
9. [Numbers & Math](#9-numbers--math)
10. [Conditionals & Loops](#10-conditionals--loops)
11. [Functions](#11-functions)
12. [Scope & Closures](#12-scope--closures)
13. [Hoisting](#13-hoisting)
14. [Arrays](#14-arrays)
15. [Objects](#15-objects)
16. [Destructuring & Spread/Rest](#16-destructuring--spreadrest)
17. [DOM Manipulation](#17-dom-manipulation)
18. [Events](#18-events)
19. [Async JavaScript](#19-async-javascript)
20. [Fetch API](#20-fetch-api)
21. [Local Storage & Cookies](#21-local-storage--cookies)
22. [Classes & OOP](#22-classes--oop)
23. [Modules](#23-modules)
24. [Advanced Concepts](#24-advanced-concepts)
25. [Best Practices & Mistakes](#25-best-practices--common-mistakes)
26. [JavaScript Cheat Sheet](#26-javascript-cheat-sheet)

---

## 1. Introduction

### Role of JavaScript
```
HTML        →  Structure  (what exists)
CSS         →  Presentation  (how it looks)
JavaScript  →  Behavior  (what it does)
```

### Client-side vs Server-side
| | Client-side | Server-side |
|-|-------------|-------------|
| **Runs in** | User's browser | Web server |
| **Controls** | UI, DOM, interactivity | Database, APIs, logic |
| **Runtime** | Browser (V8, SpiderMonkey) | Node.js |

### How JS Works in the Browser
Browsers have built-in JS engines (Chrome uses **V8**) that read, compile, and execute JS code using **Just-In-Time (JIT) compilation**.

---

## 2. JS Versions (ECMAScript)

| Version | Year | Major Features |
|---------|------|----------------|
| **ES5** | 2009 | `strict mode`, JSON, Array methods (`forEach`, `map`) |
| **ES6 / ES2015** | 2015 | `let/const`, Arrow functions, Classes, **Promises**, Modules — Biggest update |
| **ES2017** | 2017 | **`async/await`**, `Object.entries()` |
| **ES2018-2022** | 2018-22 | Optional chaining (`?.`), Nullish coalescing (`??`), `Promise.allSettled()` |
| **ES2023+** | Present | Array `findLast()`, continuous improvements |

---

## 3. Ways to Add JavaScript

```html
<!-- 1. Inline JS (Not recommended) -->
<button onclick="alert('Hello!')">Click Me</button>

<!-- 2. Internal JS -->
<script>
    console.log("Hello World");
</script>

<!-- 3. External JS (Best practice) -->
<script src="script.js" defer></script>
```

### Script Tag Attributes
| Attribute | Behavior |
|-----------|----------|
| **`defer`** | Downloads while HTML parses, executes **after** HTML is fully parsed. Maintains order. |
| **`async`** | Downloads while HTML parses, executes **immediately** when downloaded. Does not guarantee order. |

> **Best practice:** Use `defer` on `<script>` tags placed in `<head>`.

---

## 4. Syntax Basics

```js
// Single-line comment
/* Multi-line comment */

let name = "Tushar";  // Statement ends with semicolon
console.log(name);    // Output to browser console

// Case sensitive: Name ≠ name
// Identifiers must start with a letter, _ , or $
```

---

## 5. Variables

> A **variable** is a named container for storing data values.

### `var`, `let`, and `const`

| Feature | `var` | `let` | `const` |
|---------|-------|-------|---------|
| **Scope** | Function / Global | Block | Block |
| **Redeclaration** | Yes | No | No |
| **Reassignment** | Yes | Yes | **No** |
| **Hoisted** | Yes (as `undefined`) | Yes (Temporal Dead Zone) | Yes (Temporal Dead Zone) |

### Best Practice
```js
const PI = 3.14;       // Use const by default
let count = 0;         // Use let when value will change
// NEVER use var in modern JS
```

---

## 6. Data Types

JavaScript is **dynamically typed** — types are assigned at runtime.

### Primitive Types (stored by value)
| Type | Example | Description |
|------|---------|-------------|
| **String** | `"Hello"` | Text data — immutable |
| **Number** | `42`, `3.14` | Integers and floats |
| **BigInt** | `9007199254740991n` | Numbers larger than `Number.MAX_SAFE_INTEGER` |
| **Boolean** | `true`, `false` | Logical yes/no |
| **Undefined** | `undefined` | Variable declared but not assigned |
| **Null** | `null` | Intentional absence of value |
| **Symbol** | `Symbol("id")` | Unique, immutable identifier |

### Reference Types (stored by reference)
- **Object** — `{ name: "John" }`
- **Array** — `[1, 2, 3]`
- **Function** — callable object

### Truthy / Falsy Values
```
Falsy:   false, 0, "", null, undefined, NaN
Truthy:  Everything else (including "0", [], {})
```

### Type Conversion
```js
Number("5")     // 5   (explicit)
"5" + 5         // "55" (coercion — string wins)
Number(true)    // 1
Boolean("")     // false
typeof "hello"  // "string"
```

---

## 7. Operators

```js
// Arithmetic
+  -  *  /  %  **    // ** = exponentiation (2**3 = 8)

// Assignment
=  +=  -=  *=  /=

// Comparison
==   // Loose equal (ignores type) — AVOID
===  // Strict equal (checks type) — ALWAYS USE THIS
!=   !== > < >= <=

// Logical
&&   // AND
||   // OR
!    // NOT

// Ternary
let result = age >= 18 ? "Adult" : "Minor";

// Nullish Coalescing
let name = user.name ?? "Guest"; // Returns right side if left is null/undefined

// Optional Chaining
let city = user?.address?.city;  // No error if address is undefined

// Spread / Rest
...  // (see Destructuring section)
```

---

## 8. Strings

> Strings are **sequences of characters** and are **immutable**.

```js
let single   = 'Single quotes';
let double   = "Double quotes";
let template = `Template: ${single}`;  // String interpolation
```

### String Methods

| Method | Description |
|--------|-------------|
| `.length` | Character count |
| `.charAt(i)` / `.at(i)` | Get char at index (`at` supports negatives) |
| `.indexOf()` / `.includes()` | Find substring |
| `.startsWith()` / `.endsWith()` | Check start/end |
| `.slice(start, end)` | Extract a section |
| `.toUpperCase()` / `.toLowerCase()` | Change case |
| `.trim()` | Remove whitespace from both ends |
| `.replace(s, r)` / `.replaceAll()` | Replace substring(s) |
| `.split(sep)` | Split into array |
| `.repeat(n)` | Repeat string n times |

---

## 9. Numbers & Math

```js
// Special values
NaN       // Not-a-Number (result of invalid math)
Infinity  // Result of dividing by zero

// Number methods
Number("5")       // 5
parseInt("10.9")  // 10
parseFloat("10.9")// 10.9
(3.14159).toFixed(2)  // "3.14"
isNaN("abc")      // true

// Math object
Math.round(4.7)   // 5
Math.floor(4.7)   // 4
Math.ceil(4.1)    // 5
Math.random()     // Random 0 to <1
Math.max(1, 5, 3) // 5
Math.min(1, 5, 3) // 1
Math.abs(-5)      // 5
Math.pow(2, 10)   // 1024
```

---

## 10. Conditionals & Loops

### Conditionals
```js
let age = 18;

if (age < 18) {
    console.log("Minor");
} else if (age === 18) {
    console.log("Just became adult");
} else {
    console.log("Adult");
}

// Switch
switch(day) {
    case "Mon": console.log("Monday"); break;
    case "Fri": console.log("Friday"); break;
    default: console.log("Other day");
}
```

### Loops
```js
// for loop
for (let i = 0; i < 5; i++) { console.log(i); }

// while loop
while (count < 10) { count++; }

// do...while — runs at least once
do { count++; } while (count < 10);

// for...of — iterates VALUES (Arrays, Strings)
const fruits = ["Apple", "Banana"];
for (const fruit of fruits) { console.log(fruit); }

// for...in — iterates KEYS (Objects)
const person = { name: "John", age: 30 };
for (const key in person) { console.log(key, person[key]); }
```

| Statement | Description |
|-----------|-------------|
| `break` | Exits the loop entirely |
| `continue` | Skips current iteration, moves to next |

---

## 11. Functions

```js
// 1. Function Declaration (hoisted — can be called before declaration)
function greet(name) {
    return "Hello " + name;
}

// 2. Function Expression (not hoisted)
const greet = function(name) {
    return "Hello " + name;
};

// 3. Arrow Function (ES6) — shorter syntax, no own `this`
const greet = (name) => "Hello " + name;
const add = (a, b) => a + b;

// Default Parameters
function greet(name = "Guest") { return "Hello " + name; }

// Callback Function — a function passed as an argument
[1,2,3].forEach(function(num) { console.log(num); });

// Higher-Order Function — takes/returns a function
function makeMultiplier(x) { return (y) => x * y; }
const double = makeMultiplier(2);
double(5); // 10

// IIFE (Immediately Invoked Function Expression)
(function() { console.log("Runs immediately!"); })();
```

---

## 12. Scope & Closures

### Scope
```
Global Scope   → Accessible everywhere
Function Scope → Inside a function (var, let, const)
Block Scope    → Inside { } — only let and const
Lexical Scope  → Inner functions access outer function variables
```

### Closures
> A **closure** is a function that **remembers its outer scope** even after the outer function has finished executing.

```js
function makeCounter() {
    let count = 0;              // count is in outer scope
    return function() {
        count++;                // inner function "closes over" count
        return count;
    };
}
const counter = makeCounter();
counter(); // 1
counter(); // 2  ← count persists!
```

---

## 13. Hoisting

> JavaScript's behavior of **moving declarations to the top** of the scope before execution.

| Declaration | Hoisted? | Initialized? |
|-------------|----------|--------------|
| `var` | ✅ Yes | As `undefined` — accessing before declaration returns `undefined` |
| `let` / `const` | ✅ Yes | ❌ No — **Temporal Dead Zone** — accessing before declaration = `ReferenceError` |
| `function` declaration | ✅ Yes | ✅ Fully — can call before it's written |
| `function` expression | Variable only | ❌ Not the function body |

---

## 14. Arrays

> An **array** is a special variable that holds **multiple values** in an ordered list (zero-indexed).

```js
const cars = ["Saab", "Volvo", "BMW"];
console.log(cars[0]);  // "Saab"
cars.push("Tesla");    // Add to end
```

### Mutating Methods (change original array)
```js
.push(item)         // Add to end
.pop()              // Remove from end
.unshift(item)      // Add to beginning
.shift()            // Remove from beginning
.splice(i, n, ...items)  // Add/remove at index
.sort()             // Sort in place
.reverse()          // Reverse in place
```

### Non-Mutating Methods (return new array/value)
```js
.slice(start, end)  // Copy a portion
.concat(arr)        // Merge arrays
.join(sep)          // Join into string
.includes(val)      // Boolean search
.indexOf(val)       // Find index
.find(fn)           // First element matching condition
.findIndex(fn)      // Index of first match
```

### Higher-Order Array Methods
```js
// forEach — execute for each element (no return)
[1,2,3].forEach(n => console.log(n));

// map — transform each element, return NEW array
const doubled = [1,2,3].map(n => n * 2);  // [2,4,6]

// filter — return NEW array of elements that pass test
const evens = [1,2,3,4].filter(n => n % 2 === 0);  // [2,4]

// reduce — reduce to single value
const sum = [1,2,3].reduce((acc, n) => acc + n, 0);  // 6

// some — true if ANY element passes test
// every — true if ALL elements pass test
[1,2,3].some(n => n > 2);   // true
[1,2,3].every(n => n > 0);  // true
```

---

## 15. Objects

> Objects store data as **key-value pairs**.

```js
const person = {
    firstName: "John",
    lastName: "Doe",
    age: 50,
    fullName() { return this.firstName + " " + this.lastName; }
};

// Accessing properties
person.firstName        // Dot notation
person["lastName"]      // Bracket notation (use for dynamic keys)

// Add / Update
person.job = "Developer";
person.age = 51;
```

### Object Methods
```js
Object.keys(obj)              // Array of keys
Object.values(obj)            // Array of values
Object.entries(obj)           // Array of [key, value] pairs
Object.assign(target, source) // Copy properties
Object.freeze(obj)            // No add/delete/modify
Object.seal(obj)              // No add/delete, CAN modify
obj.hasOwnProperty("prop")    // Check if own property
```

---

## 16. Destructuring & Spread/Rest

### Destructuring
```js
// Array Destructuring
const [first, second] = ["Apple", "Banana"];

// Object Destructuring
const { name, age, job = "Unemployed" } = user; // job gets default
const { name: fullName } = user;                 // Rename variable
```

### Spread (expands)
```js
// Arrays
const arr2 = [...arr1, 4, 5];

// Objects
const obj2 = { ...obj1, newProp: true };

// Function call
Math.max(...[1, 5, 3]);  // 5
```

### Rest (condenses)
```js
// Gather remaining function arguments
function sum(first, ...rest) {
    return rest.reduce((a, b) => a + b, first);
}
sum(1, 2, 3, 4);  // 10
```

---

## 17. DOM Manipulation

> **DOM** = Document Object Model. The browser's in-memory tree of HTML elements that JavaScript can read and modify.

### Selecting Elements
```js
document.getElementById("id")          // Single element by ID
document.querySelector(".class")       // First match (CSS selector)
document.querySelectorAll("div")       // All matches (NodeList)
document.getElementsByClassName("cls") // HTMLCollection by class
```

### Modifying Elements
```js
element.innerText = "New text";        // Visible text only
element.textContent = "All text";      // All text (including hidden)
element.innerHTML = "<b>Bold</b>";     // HTML content (⚠️ XSS risk)

element.style.color = "red";           // Inline CSS
element.classList.add("active");
element.classList.remove("hidden");
element.classList.toggle("open");

element.setAttribute("src", "img.png");
element.getAttribute("href");
```

### Creating & Removing Elements
```js
const div = document.createElement("div");
div.innerText = "Hello";
document.body.appendChild(div);   // Add to end of body
div.remove();                      // Remove element
```

---

## 18. Events

```js
const btn = document.querySelector("button");

btn.addEventListener("click", function(event) {
    console.log("Clicked!");
    console.log(event.target);          // Element that triggered the event
    event.preventDefault();            // Stop default action (e.g., form submit)
    event.stopPropagation();           // Stop event from bubbling up
});
```

### Common Events
```
click         Mouse click
dblclick      Double click
mouseover     Mouse enters element
keydown       Key pressed
keyup         Key released
submit        Form submitted
input         Input value changes
change        Input loses focus with new value
load          Page/resource loaded
resize        Window resized
scroll        Page scrolled
```

### Event Bubbling & Delegation
```
Event Bubbling: Event triggers on the innermost element, then bubbles UP to parents.

Event Delegation: Attach ONE listener to a parent to handle events on its children.
```
```js
// Event Delegation — handles dynamically created children
document.getElementById("list").addEventListener("click", (e) => {
    if (e.target.tagName === "LI") {
        console.log("Clicked:", e.target.textContent);
    }
});
```

---

## 19. Async JavaScript

> JavaScript is **single-threaded** — executes one command at a time. Async JS allows operations (like fetching data) to run in the **background** without blocking the UI.

### The Async Flow
```
Call Stack → Web APIs (setTimeout, fetch) → Callback Queue → Event Loop → Call Stack
```

### Callbacks (old way — leads to "Callback Hell")
```js
fetchData(function(data) {
    processData(data, function(result) {
        saveResult(result, function() { ... }); // deeply nested!
    });
});
```

### Promises (modern way)
```js
// States: Pending → Fulfilled | Rejected

fetchData()
    .then(data => console.log(data))      // On success
    .catch(error => console.error(error)) // On failure
    .finally(() => console.log("Done"));  // Always runs
```

### async / await (cleanest way)
> **Syntactic sugar over Promises.** Makes async code look synchronous.

```js
async function getData() {
    try {
        const response = await fetch('https://api.example.com/data');
        // ↑ Pauses THIS function until response arrives (but page stays responsive)

        if (!response.ok) throw new Error("Failed!");

        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error("Error:", error);
    }
}
getData();
```

---

## 20. Fetch API

> The **Fetch API** is a modern native tool for making **HTTP requests**. Returns a Promise.

### GET Request
```js
fetch('https://api.example.com/users')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));
```

### POST Request
```js
fetch('https://api.example.com/users', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name: "Tushar", age: 22 })
})
.then(res => res.json())
.then(data => console.log(data));
```

### With async/await
```js
async function createUser(userData) {
    try {
        const res = await fetch('/api/users', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(userData)
        });
        const data = await res.json();
        return data;
    } catch (err) {
        console.error("Fetch failed:", err);
    }
}
```

### JSON
```js
// JS Object → JSON string (for sending)
JSON.stringify({ name: "John", age: 30 });
// '{"name":"John","age":30}'

// JSON string → JS Object (for using)
JSON.parse('{"name":"John"}');
// { name: "John" }
```

---

## 21. Local Storage & Cookies

### localStorage
```js
// Stores data with NO expiration (persists after browser close)
localStorage.setItem("theme", "dark");
const theme = localStorage.getItem("theme");
localStorage.removeItem("theme");
localStorage.clear();

// Store objects (must stringify)
const user = { name: "Tushar" };
localStorage.setItem("user", JSON.stringify(user));
const savedUser = JSON.parse(localStorage.getItem("user"));
```

### sessionStorage
```js
// Same API, but data cleared when the TAB is closed
sessionStorage.setItem("key", "value");
```

### Cookies
```js
// Sent to server with every HTTP request
document.cookie = "username=Tushar; max-age=86400; Secure; SameSite=Strict";
```

| | localStorage | sessionStorage | Cookies |
|-|-------------|----------------|---------|
| **Expiry** | Never | Tab close | Set manually |
| **Sent to server** | No | No | **Yes** |
| **Size limit** | ~5MB | ~5MB | ~4KB |

---

## 22. Classes & OOP

> JavaScript classes are **syntactic sugar** over prototype-based inheritance.

```js
class Animal {
    constructor(name) {
        this.name = name;             // Instance property
    }
    speak() {
        console.log(`${this.name} makes a noise.`);
    }
    static describe() {
        console.log("Animals are living things.");
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name);                  // Call parent constructor
        this.breed = breed;
    }
    speak() {                         // Method override
        console.log(`${this.name} barks!`);
    }
}

const myDog = new Dog("Rex", "German Shepherd");
myDog.speak();       // "Rex barks!"
Animal.describe();   // Static method call
```

### `this` Keyword

| Context | `this` refers to |
|---------|-----------------|
| Object method | The **owner object** |
| Regular function | Global object (`window`) or `undefined` in strict mode |
| Arrow function | Inherited from **enclosing lexical scope** — no own `this` |
| Event handler | The **element** that received the event |

```js
// Explicit binding
fn.call(obj, arg1, arg2)   // Call fn with obj as this
fn.apply(obj, [arg1, arg2]) // Same, but args as array
const bound = fn.bind(obj)  // Return new function with this bound
```

---

## 23. Modules

> Modules allow **splitting code into separate files** for organization and reusability.

```js
// math.js — Exporting
export const PI = 3.14;
export function add(a, b) { return a + b; }
export default function sayHi() { console.log("Hi!"); }

// app.js — Importing
import sayHi, { PI, add } from './math.js';
import * as math from './math.js';  // Import all as namespace
```

---

## 24. Advanced Concepts

### Map & Set
```js
// Map: key-value pairs where keys can be ANY type
const map = new Map();
map.set("name", "John");
map.get("name");           // "John"
map.has("name");           // true

// Set: collection of UNIQUE values
const set = new Set([1, 2, 2, 3]);  // {1, 2, 3}
set.add(4);
set.has(2);   // true
```

### Currying
```js
// Transform multi-arg function into sequence of single-arg functions
const add = a => b => a + b;
add(2)(3);  // 5
```

### Debouncing vs Throttling
| | Debounce | Throttle |
|-|----------|----------|
| **When to use** | Wait until user stops typing | Limit scroll/resize events |
| **Behavior** | Executes **after** delay of no calls | Executes at most **once per interval** |

### Shallow vs Deep Copy
```js
// Shallow copy (nested objects still shared)
const copy = { ...original };
const copy = Object.assign({}, original);

// Deep copy (completely independent)
const deepCopy = structuredClone(original);  // Modern
const deepCopy = JSON.parse(JSON.stringify(original));  // Old way
```

### Regular Expressions
```js
const pattern = /hello/i;           // Case-insensitive match
pattern.test("Hello World");        // true
"Hello World".match(/hello/i);      // ["Hello"]
"a1b2".replace(/[0-9]/g, "X");     // "aXbX"
```

---

## 25. Best Practices & Common Mistakes

### ✅ Best Practices
- Use **`const`** by default; `let` when value changes; **never `var`**
- Use **strict equality** (`===`) instead of loose (`==`)
- Keep functions **small and focused** on one task
- **Avoid global variables** to prevent scope pollution
- Handle errors with **`try/catch`** or `.catch()` on Promises
- Use **descriptive names**: `getUserById()` not `getData()`
- Use `defer` on script tags, or place `<script>` at the end of `<body>`

### ❌ Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using `==` instead of `===` | Always use `===` |
| Mutating arrays with `sort()` | Use `[...arr].sort()` to keep original |
| Losing `this` in callbacks | Use arrow functions or `.bind(this)` |
| Not handling async errors | Always use `try/catch` with `await` |
| Accessing DOM before it loads | Use `defer` or put script at end of body |
| `null` vs `undefined` confusion | `null` = intentional empty; `undefined` = not assigned |

---

## 26. JavaScript Cheat Sheet

### Variables & Types
```js
const name = "Tushar";    let count = 0;
typeof "hello" // "string"    typeof 42 // "number"
```

### Arrays
```js
.push()  .pop()  .shift()  .unshift()  .splice()
.map()  .filter()  .reduce()  .find()  .includes()
```

### Objects
```js
Object.keys(obj)   Object.values(obj)   Object.entries(obj)
const { name, age } = user;    // Destructuring
const copy = { ...obj };       // Spread
```

### DOM
```js
document.querySelector(".class")
element.textContent = "text"
element.classList.toggle("active")
element.addEventListener("click", fn)
document.createElement("div")
parent.appendChild(child)
```

### Async
```js
async function fetchData() {
    const res = await fetch(url);
    const data = await res.json();
    return data;
}
```

### Quick Reference

| Category | Key Items |
|----------|-----------|
| **Variables** | `const`, `let` |
| **Data Types** | String, Number, Boolean, Object, Array, Null, Undefined |
| **Arrays** | `.push()`, `.pop()`, `.map()`, `.filter()`, `.reduce()` |
| **Objects** | `Object.keys()`, `Object.values()`, destructuring |
| **DOM** | `querySelector()`, `addEventListener()`, `createElement()` |
| **Async** | `fetch()`, Promises, `async/await` |
| **Storage** | `localStorage.setItem(k, v)`, `.getItem(k)` |
| **Classes** | `class`, `extends`, `super`, `constructor` |
| **Modules** | `export`, `import`, `export default` |
