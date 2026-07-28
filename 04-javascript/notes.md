title: "JavaScript: Complete Beginner to Advanced"
subtitle: "From First Principles to Production-Grade Architecture"
author: "Principal Software Engineer — 15+ Years Industry Experience"
version: "2.0"
date: "2025"

# JavaScript
## Complete Beginner to Advanced Engineering Handbook

> A production-grade, book-quality reference covering JavaScript internals, ES6+, async, DOM, patterns, and TypeScript. Written for engineers at all levels — from beginners to FAANG system designers.

> **Prerequisites:** [03 — CSS ←](../03-css/notes.md) · **Next:** [05 — HTTP, JSON & Fetch →](../05-http-json-fetch/notes.md)


## Table of Contents

### Part I: Language Fundamentals
- [Chapter 1: Introduction](#chapter-1-introduction)
- [Chapter 2: ECMAScript Versions](#chapter-2-ecmascript-versions)
- [Chapter 3: Adding JavaScript to a Page](#chapter-3-adding-javascript-to-a-page)
- [Chapter 4: Variables](#chapter-4-variables)
- [Chapter 5: Data Types](#chapter-5-data-types)
- [Chapter 6: Operators](#chapter-6-operators)
- [Chapter 7: Strings](#chapter-7-strings)
- [Chapter 8: Numbers & Math](#chapter-8-numbers--math)
- [Chapter 9: Conditionals & Loops](#chapter-9-conditionals--loops)

### Part II: Functions, Scope & Objects
- [Chapter 10: Functions](#chapter-10-functions)
- [Chapter 11: Scope & Closures](#chapter-11-scope--closures)
- [Chapter 12: Hoisting](#chapter-12-hoisting)
- [Chapter 13: Arrays](#chapter-13-arrays)
- [Chapter 14: Objects](#chapter-14-objects)
- [Chapter 15: ES6+ Features](#chapter-15-es6-features)

### Part III: Browser & Async
- [Chapter 16: DOM Manipulation](#chapter-16-dom-manipulation)
- [Chapter 17: Events](#chapter-17-events)
- [Chapter 18: Browser Storage APIs](#chapter-18-browser-storage-apis)
- [Chapter 19: The Browser Object Model (BOM)](#chapter-19-the-browser-object-model-bom)
- [Chapter 20: Classes & OOP](#chapter-20-classes--oop)
- [Chapter 21: Modules](#chapter-21-modules)
- [Chapter 22: Async JavaScript — Introduction](#chapter-22-async-javascript--introduction)
- [Chapter 23: Interview Preparation](#chapter-23-interview-preparation)
- [Chapter 24: Production Checklist](#chapter-24-production-checklist)
- [Chapter 25: Cheat Sheet](#chapter-25-cheat-sheet)


# CHAPTER 1: Introduction

JavaScript (JS) is a dynamically typed, interpreted programming language. In a browser, it adds behavior to HTML and CSS — handling user events, manipulating the DOM, and communicating with servers.

```
HTML        →  Structure (what exists)
CSS         →  Presentation (how it looks)
JavaScript  →  Behavior (what it does)
```

Browsers execute JavaScript using a **JavaScript engine**: Chrome and Edge use **V8**, Firefox uses **SpiderMonkey**, Safari uses **JavaScriptCore**. Node.js also runs on V8, enabling JavaScript outside the browser.


# CHAPTER 2: ECMAScript Versions

JavaScript is standardized as **ECMAScript (ES)**. New versions are released yearly.

| Version | Year | Major Additions |
|:--------|:-----|:----------------|
| ES5 | 2009 | `strict mode`, `JSON`, `Array.forEach/map/filter` |
| **ES6 / ES2015** | 2015 | `let/const`, arrow functions, classes, Promises, template literals, modules — largest single update |
| ES2017 | 2017 | `async/await`, `Object.entries()` |
| ES2018–2022 | 2018–22 | Optional chaining (`?.`), nullish coalescing (`??`), `Promise.allSettled()` |
| ES2023+ | 2023+ | `Array.findLast()`, `Array.toSorted()`, ongoing improvements |


# CHAPTER 3: Adding JavaScript to a Page

```html
<!-- External script — best practice -->
<script src="main.js" defer></script>

<!-- Internal script — acceptable for small demos -->
<script>
  console.log('Hello');
</script>

<!-- Inline handler — avoid; difficult to maintain and test -->
<button onclick="alert('clicked')">Click</button>
```

| `<script>` attribute | Behavior |
|:--------------------|:---------|
| **`defer`** | Downloads in parallel with HTML parsing; executes after parsing is complete; maintains script order |
| **`async`** | Downloads in parallel; executes immediately when downloaded; does not guarantee order |

> Always use `defer` on scripts in `<head>`. Place scripts at the end of `<body>` only when `defer` is not available.


# CHAPTER 4: Variables

A variable is a named binding to a value in memory.

| Keyword | Scope | Reassignable | Redeclarable | Hoisted |
|:--------|:------|:------------:|:------------:|:-------:|
| `const` | Block | ✗ | ✗ | TDZ |
| `let` | Block | ✓ | ✗ | TDZ |
| `var` | Function/Global | ✓ | ✓ | As `undefined` |

> TDZ = Temporal Dead Zone — accessing the variable before its declaration throws a `ReferenceError`.

```js
const API_URL = 'https://api.example.com'; // Use const by default
let requestCount = 0;                       // Use let when value will change
// Never use var in modern JavaScript
```


# CHAPTER 5: Data Types

JavaScript is **dynamically typed** — the type of a variable is determined at runtime.

### Primitive Types (stored by value, immutable)

| Type | Example | Notes |
|:-----|:--------|:------|
| `string` | `'hello'` | Immutable sequence of characters |
| `number` | `42`, `3.14` | All numbers are 64-bit floats; no separate integer type |
| `bigint` | `9007199254740991n` | Integers beyond `Number.MAX_SAFE_INTEGER` |
| `boolean` | `true`, `false` | Logical values |
| `undefined` | `undefined` | Variable declared but no value assigned |
| `null` | `null` | Intentional absence of a value |
| `symbol` | `Symbol('id')` | Unique, immutable identifier |

### Reference Types (stored by reference, mutable)

- `Object` — `{ key: value }`
- `Array` — `[1, 2, 3]`
- `Function` — a callable object

### Truthy and Falsy

```js
// Falsy values — evaluate to false in a boolean context
false, 0, -0, 0n, '', null, undefined, NaN

// Everything else is truthy, including:
'0', [], {}, function() {}
```

### Type Checking

```js
typeof 'hello'      // 'string'
typeof 42           // 'number'
typeof null         // 'object' — known historical bug in JavaScript
typeof []           // 'object'
Array.isArray([])   // true — correct way to check for an array
```


# CHAPTER 6: Operators

```js
// Arithmetic
+  -  *  /  %  **         // ** = exponentiation (2 ** 3 = 8)

// Assignment
=  +=  -=  *=  /=  **=  ??=

// Comparison — always use strict equality
===   // Strict equal (checks value AND type) — always use this
!==   // Strict not equal
<  >  <=  >=

// Logical
&&    // AND — returns first falsy value, or last value
||    // OR  — returns first truthy value, or last value
!     // NOT
??    // Nullish coalescing — returns right side if left is null/undefined

// Ternary
const label = age >= 18 ? 'adult' : 'minor';

// Optional chaining — short-circuits to undefined instead of throwing
const city = user?.address?.city;
const first = arr?.[0];
const result = obj?.method?.();
```


# CHAPTER 7: Strings

```js
const single   = 'Single quotes';
const double   = "Double quotes";
const template = `Hello, ${name}! You have ${count} messages.`; // Template literal

// Multiline without escape characters
const html = `
  <div class="card">
    <h2>${title}</h2>
  </div>
`;
```

### String Methods

| Method | Description |
|:-------|:------------|
| `.length` | Character count |
| `.at(i)` | Character at index — supports negative indices |
| `.includes(str)` | Returns boolean |
| `.startsWith(str)` / `.endsWith(str)` | Returns boolean |
| `.indexOf(str)` | First index or `-1` |
| `.slice(start, end)` | Extract substring |
| `.toUpperCase()` / `.toLowerCase()` | Case conversion |
| `.trim()` / `.trimStart()` / `.trimEnd()` | Remove whitespace |
| `.replace(s, r)` / `.replaceAll(s, r)` | Replace occurrences |
| `.split(sep)` | Split into array |
| `.padStart(len, char)` / `.padEnd(len, char)` | Pad to length |


# CHAPTER 8: Numbers & Math

```js
// Special values
NaN           // Not a Number — result of invalid arithmetic
Infinity      // Result of division by zero
Number.MAX_SAFE_INTEGER  // 2^53 - 1

// Conversion
Number('42')       // 42
parseInt('10.9')   // 10  (integer portion)
parseFloat('10.9') // 10.9
(3.14159).toFixed(2) // '3.14'
isNaN('abc')       // true

// Math object
Math.round(4.5)    // 5
Math.floor(4.9)    // 4
Math.ceil(4.1)     // 5
Math.abs(-7)       // 7
Math.max(1, 5, 3)  // 5
Math.min(1, 5, 3)  // 1
Math.random()      // Random float [0, 1)
Math.sqrt(16)      // 4
Math.pow(2, 10)    // 1024
```


# CHAPTER 9: Conditionals & Loops

```js
// if / else if / else
if (score >= 90) {
  grade = 'A';
} else if (score >= 80) {
  grade = 'B';
} else {
  grade = 'F';
}

// switch
switch (status) {
  case 'active': console.log('Active'); break;
  case 'inactive': console.log('Inactive'); break;
  default: console.log('Unknown');
}

// for
for (let i = 0; i < 5; i++) { ... }

// while
while (queue.length > 0) { queue.shift(); }

// for...of — iterate values (Arrays, Strings, Maps, Sets)
for (const item of items) { console.log(item); }

// for...in — iterate keys (Objects)
for (const key in config) { console.log(key, config[key]); }
```


# CHAPTER 10: Functions

```js
// 1. Function Declaration — hoisted; callable before the declaration
function greet(name) {
  return `Hello, ${name}`;
}

// 2. Function Expression — not hoisted
const greet = function(name) {
  return `Hello, ${name}`;
};

// 3. Arrow Function — concise; no own `this`, `arguments`, `super`
const greet = (name) => `Hello, ${name}`;
const add = (a, b) => a + b;                 // Implicit return for single expressions
const double = n => n * 2;                   // Parentheses optional for single parameter

// Default parameters
function createUser(name, role = 'viewer') {
  return { name, role };
}

// Rest parameters — collect remaining arguments into an array
function sum(first, ...rest) {
  return rest.reduce((acc, n) => acc + n, first);
}
sum(1, 2, 3, 4); // 10

// IIFE — Immediately Invoked Function Expression
(function() {
  // Runs immediately; does not pollute global scope
})();
```

### Higher-Order Functions

A higher-order function takes a function as an argument or returns one.

```js
// Callback — function passed as argument
[1, 2, 3].forEach(n => console.log(n));

// Returning a function (factory pattern)
function makeMultiplier(factor) {
  return (n) => n * factor;
}
const triple = makeMultiplier(3);
triple(5); // 15
```


# CHAPTER 11: Scope & Closures

### Scope

```
Global scope    — accessible everywhere
Module scope    — limited to the file/module
Function scope  — inside a function (var, let, const)
Block scope     — inside { } (let, const only)
```

### Closures

A **closure** is a function that retains access to its enclosing scope after the outer function has returned.

```js
function makeCounter(initialValue = 0) {
  let count = initialValue; // Enclosed variable

  return {
    increment() { return ++count; },
    decrement() { return --count; },
    value()     { return count; },
  };
}

const counter = makeCounter();
counter.increment(); // 1
counter.increment(); // 2
counter.value();     // 2
```

Closures are the foundation of module patterns, memoization, and event handlers that retain state.


# CHAPTER 12: Hoisting

JavaScript moves declarations to the top of their scope before execution.

| Declaration | Hoisted? | Initialised? |
|:------------|:--------:|:------------:|
| `var` | ✓ | As `undefined` |
| `let` / `const` | ✓ | ✗ — Temporal Dead Zone |
| `function` declaration | ✓ | ✓ Fully |
| `function` expression | Variable only | ✗ |

```js
console.log(x); // undefined — var hoisted, but not its value
var x = 10;

console.log(y); // ReferenceError — TDZ
let y = 10;

greet(); // 'Hello' — function declaration is fully hoisted
function greet() { return 'Hello'; }
```


# CHAPTER 13: Arrays

```js
const fruits = ['apple', 'banana', 'cherry'];
fruits[0];         // 'apple'
fruits.length;     // 3
fruits.at(-1);     // 'cherry' — last element
```

### Mutating Methods

```js
.push(item)               // Add to end; returns new length
.pop()                    // Remove from end; returns removed item
.unshift(item)            // Add to beginning
.shift()                  // Remove from beginning
.splice(index, count, ...items) // Remove/insert at index
.sort((a, b) => a - b)   // Sort numerically ascending
.reverse()                // Reverse in place
.fill(value, start, end)  // Fill with value
```

### Non-Mutating Methods

```js
.slice(start, end)        // Copy a portion (end exclusive)
.concat(arr)              // Merge arrays
.join(separator)          // Join into string
.includes(value)          // Boolean presence check
.indexOf(value)           // First index or -1
.find(fn)                 // First element passing test
.findIndex(fn)            // Index of first element passing test
.flat(depth)              // Flatten nested arrays
.flatMap(fn)              // map then flat(1)
```

### Higher-Order Array Methods

```js
// forEach — execute for side effects; returns undefined
[1, 2, 3].forEach(n => console.log(n));

// map — transform each element; returns NEW array of same length
const doubled = [1, 2, 3].map(n => n * 2); // [2, 4, 6]

// filter — keep elements passing test; returns NEW array
const evens = [1, 2, 3, 4].filter(n => n % 2 === 0); // [2, 4]

// reduce — accumulate to a single value
const sum = [1, 2, 3].reduce((acc, n) => acc + n, 0); // 6

// some — true if ANY element passes test
[1, 2, 3].some(n => n > 2);    // true

// every — true if ALL elements pass test
[1, 2, 3].every(n => n > 0);   // true
```


# CHAPTER 14: Objects

```js
const user = {
  id: 1,
  name: 'Alice',
  role: 'admin',
  greet() {              // Method shorthand
    return `Hi, I'm ${this.name}`;
  },
};

// Property access
user.name;               // Dot notation
user['role'];            // Bracket notation — required for dynamic keys

// Add / update / delete
user.email = 'alice@example.com';
user.role = 'editor';
delete user.role;
```

### Object Methods

```js
Object.keys(obj)                  // Array of own enumerable keys
Object.values(obj)                // Array of own enumerable values
Object.entries(obj)               // Array of [key, value] pairs
Object.fromEntries(entries)       // Construct object from entries
Object.assign(target, ...sources) // Shallow copy properties
Object.freeze(obj)                // Prevents all modifications
obj.hasOwnProperty('key')         // Check for own property
```


# CHAPTER 15: ES6+ Features

### Destructuring

```js
// Array destructuring
const [first, second, ...rest] = [10, 20, 30, 40];

// Object destructuring
const { name, role, id: userId } = user;  // Rename: id → userId
const { theme = 'light' } = settings;     // Default value

// Function parameter destructuring
function display({ name, email }) {
  console.log(name, email);
}
```

### Spread and Rest

```js
// Spread — expand into individual elements
const merged = [...arr1, ...arr2];
const updated = { ...original, role: 'admin' }; // Shallow clone + override
Math.max(...numbers);

// Rest — collect into an array (in function parameters)
function log(level, ...messages) { ... }
```

### Optional Chaining `?.`

```js
const zip = user?.address?.zipCode;   // undefined instead of TypeError
const item = arr?.[index];            // Array access
const result = obj?.method?.();       // Method call
```

### Nullish Coalescing `??`

```js
// Returns right side only if left side is null or undefined
const count = response.count ?? 0;   // 0 only if count is null/undefined
// Unlike ||, it does NOT treat 0 or '' as falsy
```


# CHAPTER 16: DOM Manipulation

The **DOM (Document Object Model)** is a tree of nodes that the browser builds from the HTML document. JavaScript can read and modify this tree to update the UI dynamically.

> The DOM itself is introduced in [01 — Full Stack Fundamentals](../01-full-stack-fundamentals/notes.md#the-browser). This section covers the JavaScript API.

### Selecting Elements

```js
document.getElementById('header')           // Single element by id
document.querySelector('.card')             // First element matching CSS selector
document.querySelectorAll('.card')          // NodeList of all matches
document.getElementsByClassName('active')  // HTMLCollection by class
```

### Reading & Modifying Elements

```js
element.textContent = 'New text';      // Set text content (safe — no HTML parsing)
element.innerHTML = '<strong>Bold</strong>'; // Set HTML — ⚠️ XSS risk with user input

element.getAttribute('href');
element.setAttribute('data-id', '42');
element.removeAttribute('disabled');

element.style.color = '#1d4ed8';       // Inline style — prefer classes

element.classList.add('active');
element.classList.remove('hidden');
element.classList.toggle('open');
element.classList.contains('active');  // Boolean check
```

### Creating & Removing Elements

```js
const card = document.createElement('div');
card.className = 'card';
card.textContent = 'New card';

document.body.appendChild(card);               // Append to end
container.insertBefore(card, referenceNode);   // Insert before another element
parent.insertAdjacentHTML('beforeend', html);  // Insert parsed HTML — efficient

card.remove();                                 // Remove element from DOM
```


# CHAPTER 17: Events

```js
const button = document.querySelector('#submit-btn');

button.addEventListener('click', function(event) {
  event.preventDefault();          // Cancel default action (e.g., form submission)
  event.stopPropagation();         // Stop the event from bubbling to parent elements
  console.log(event.target);       // Element that triggered the event
  console.log(event.currentTarget); // Element the listener is attached to
});
```

### Common Event Types

```
Mouse:    click  dblclick  mouseenter  mouseleave  mousemove
Keyboard: keydown  keyup  keypress
Form:     submit  change  input  focus  blur  reset
Window:   load  DOMContentLoaded  resize  scroll  hashchange
```

### Event Bubbling & Delegation

Events propagate **upward** (bubble) through the DOM from the target to the document root.

**Event delegation** attaches a single listener to a parent to handle events on its children — including dynamically added ones:

```js
// Instead of attaching a listener to every <li>:
document.getElementById('task-list').addEventListener('click', (event) => {
  const item = event.target.closest('li');
  if (!item) return;
  item.classList.toggle('completed');
});
```


# CHAPTER 18: Browser Storage APIs

### localStorage

Persistent key-value store. Data persists until explicitly cleared. Never sent to the server.

```js
// Strings
localStorage.setItem('theme', 'dark');
const theme = localStorage.getItem('theme'); // 'dark'
localStorage.removeItem('theme');
localStorage.clear();                        // Clear all items

// Objects — must be serialised
const prefs = { theme: 'dark', lang: 'en' };
localStorage.setItem('prefs', JSON.stringify(prefs));
const saved = JSON.parse(localStorage.getItem('prefs'));
```

### sessionStorage

Same API as `localStorage`. Data is cleared when the browser tab is closed.

```js
sessionStorage.setItem('draft', draftContent);
```

### Comparison

| | `localStorage` | `sessionStorage` | Cookie |
|:--|:--------------|:----------------|:-------|
| **Expires** | Never (manual only) | Tab close | On `Max-Age` or manually |
| **Sent to server** | ✗ | ✗ | ✓ |
| **Size** | ~5 MB | ~5 MB | ~4 KB |

> **Note:** Cookies are an HTTP state management mechanism. They are sent automatically with every request via the `Cookie` header. For HTTP headers and request metadata, see [Module 05 — HTTP, JSON & Fetch](../05-http-json-fetch/notes.md#1-http-in-depth).


# CHAPTER 19: The Browser Object Model (BOM)

The **BOM** is the interface through which JavaScript communicates with the browser itself — not the document content, but the browser environment.

```
window  (global object — everything is a property of window)
├── window.location   — current URL and navigation
├── window.history    — session navigation history
├── window.navigator  — browser and device information
├── window.screen     — display information
├── window.alert()    — modal alert dialog
├── window.confirm()  — yes/no dialog; returns boolean
├── window.prompt()   — text input dialog; returns string
├── setTimeout(fn, ms)    — run once after delay
├── setInterval(fn, ms)   — run repeatedly on interval
├── clearTimeout(id)
└── clearInterval(id)
```

```js
// Navigation
window.location.href = '/dashboard';     // Navigate to URL
window.location.reload();                // Reload the page
window.history.back();                   // Go back in history

// Browser information
navigator.userAgent;   // Browser identification string
navigator.language;    // User's preferred language ('en-US')
navigator.onLine;      // Boolean — is user online?

// Screen
screen.width;   // Physical screen width in pixels
screen.height;

// Timers
const timerId = setTimeout(() => {
  console.log('Runs once after 2 seconds');
}, 2000);

const intervalId = setInterval(() => {
  console.log('Runs every 1 second');
}, 1000);

clearInterval(intervalId); // Stop the interval
```


# CHAPTER 20: Classes & OOP

JavaScript classes are syntactic sugar over prototype-based inheritance.

```js
class Animal {
  #sound; // Private field (ES2022)

  constructor(name, sound) {
    this.name = name;
    this.#sound = sound;
  }

  speak() {
    return `${this.name} says ${this.#sound}`;
  }

  static create(name, sound) { // Static factory method
    return new Animal(name, sound);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name, 'woof');       // Call parent constructor
    this.breed = breed;
  }

  fetch(item) {                // Additional method
    return `${this.name} fetches the ${item}!`;
  }
}

const dog = new Dog('Rex', 'German Shepherd');
dog.speak();  // 'Rex says woof'
dog.fetch('ball'); // 'Rex fetches the ball!'
```

### `this` Keyword

| Context | `this` refers to |
|:--------|:-----------------|
| Object method | The object the method is called on |
| Regular function | `undefined` (strict mode) or `window` (non-strict) |
| Arrow function | Inherited from enclosing lexical scope — **no own `this`** |
| Event handler | The element the listener is attached to |
| Constructor | The newly created instance |


# CHAPTER 21: Modules

ES modules split code across files for organisation and reusability. Each file is its own scope.

```js
// math.js — Named and default exports
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export default function square(n) { return n * n; }

// app.js — Importing
import square, { PI, add } from './math.js'; // Default + named
import * as math from './math.js';            // Namespace import

// Dynamic import — loads module on demand (code splitting)
const module = await import('./heavy-module.js');
```

> Scripts using ES modules must have `type="module"` on the `<script>` tag, or be loaded via a bundler (Vite, webpack).


# CHAPTER 22: Async JavaScript — Introduction

JavaScript is **single-threaded**. Long operations (network requests, file reads) would block the UI if run synchronously. Async patterns allow the runtime to continue executing other code while waiting for the operation to complete.

```
Call Stack → Web APIs → Task Queue → Event Loop → Call Stack
```

**Callbacks (legacy pattern — leads to deeply nested "callback hell"):**

```js
fetchUser(id, function(user) {
  fetchPosts(user.id, function(posts) {
    renderPosts(posts, function() { ... }); // Deeply nested
  });
});
```

**Promises (ES2015 — flat and composable):**

```js
fetchUser(id)
  .then(user => fetchPosts(user.id))
  .then(posts => renderPosts(posts))
  .catch(err => console.error(err))
  .finally(() => hideLoader());
```

**`async`/`await` (ES2017 — reads like synchronous code):**

```js
async function loadUserPosts(id) {
  try {
    const user  = await fetchUser(id);
    const posts = await fetchPosts(user.id);
    renderPosts(posts);
  } catch (err) {
    console.error('Failed to load:', err);
  }
}
```

> The Fetch API, JSON, Promises in depth, error handling patterns, and CRUD via Fetch are fully covered in [05 — HTTP, JSON & Fetch →](../05-http-json-fetch/notes.md)

# CHAPTER 23: Interview Preparation

### Beginner
1. **What is the difference between `let`, `const`, and `var`?**
   *Answer:* `var` is function/globally scoped, hoisted, and can be redeclared. `let` and `const` are block-scoped and subject to the Temporal Dead Zone. `let` can be reassigned; `const` cannot.
2. **Explain the difference between `==` and `===`.**
   *Answer:* `==` performs type coercion before comparing (e.g., `'5' == 5` is true). `===` strictly compares both value and type without coercion.

### Intermediate
3. **What is a closure? Provide an example.**
   *Answer:* A closure is a function that retains access to its lexical scope even after the outer function has finished executing. It is used for data privacy and currying.
4. **How does the `this` keyword work?**
   *Answer:* `this` dynamically refers to the object calling the function. In methods, it's the object. In standard functions (non-strict mode), it defaults to `window`. Arrow functions do not have their own `this`; they inherit it from their enclosing lexical context.

### Advanced
5. **Describe the Event Loop in Node.js / Browser.**
   *Answer:* JavaScript is single-threaded. Synchronous code goes into the Call Stack. Asynchronous tasks (timers, fetch) are passed to Web APIs. Once complete, their callbacks enter the Microtask Queue (Promises) or Callback Queue (Timers). When the Call Stack is empty, the Event Loop pushes tasks from the queues into the stack.

# CHAPTER 24: Production Checklist

Before pushing JavaScript code to production:
- [ ] Use `const` by default. Only use `let` when reassignment is explicitly needed.
- [ ] Ensure `Strict Mode` (`"use strict";`) is enabled or use ES modules (where it is enabled by default).
- [ ] Avoid modifying global prototypes (`Array.prototype.customMap = ...`) to prevent collisions.
- [ ] Implement `try/catch` blocks for all `async/await` operations.
- [ ] Do not expose sensitive API keys in frontend JavaScript.
- [ ] Ensure all unused imports and dead code are stripped out during the build process (Tree Shaking).
- [ ] Run code through an AST-based linter like ESLint to catch syntax errors and anti-patterns.

# CHAPTER 25: Cheat Sheet

### Array Iteration
```js
const numbers = [1, 2, 3, 4, 5];
// Map - Transforms items
const doubled = numbers.map(n => n * 2);
// Filter - Removes items
const evens = numbers.filter(n => n % 2 === 0);
// Reduce - Accumulates
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
```

### Object Destructuring
```js
const user = { id: 1, name: 'Alice', role: 'admin' };
const { name, role: userRole, status = 'active' } = user;
```

### Async / Await
```js
async function getData() {
  try {
    const res = await fetch('https://api.example.com/data');
    if (!res.ok) throw new Error('Network response was not ok');
    const data = await res.json();
    return data;
  } catch (error) {
    console.error('Fetch error:', error);
  }
}
```

> **Next:** [05 — HTTP, JSON & Fetch →](../05-http-json-fetch/notes.md)
