# The Complete JavaScript Reference Guide

Welcome to the ultimate guide to JavaScript. This comprehensive reference is designed to take you from a complete beginner to an advanced JavaScript developer.

## 1. Introduction to JavaScript

### What is JavaScript?
JavaScript (JS) is a lightweight, cross-platform, and interpreted programming language primarily known as the scripting language for web pages.

### Why JavaScript is used
It allows developers to implement complex features on web pages, such as interactive forms, animations, 2D/3D graphics, and dynamic content updates without reloading the page.

### Role of JavaScript in web development
JavaScript provides the behavior and interactivity to the static structure provided by HTML and CSS.

### JavaScript vs HTML vs CSS
*   **HTML:** Defines the structure (e.g., paragraphs, headings).
*   **CSS:** Defines the presentation (e.g., colors, layouts).
*   **JavaScript:** Defines the behavior (e.g., clicking a button shows a popup).

### Client-side vs Server-side JavaScript
*   **Client-side:** Runs in the user's web browser to control the user interface.
*   **Server-side:** Runs on a web server (using Node.js) to handle database queries, server logic, and APIs.

### How JavaScript works in the browser
Browsers have built-in JS engines (like Chrome's V8) that read, compile, and execute JS code on the fly (Just-In-Time compilation).

### Advantages of JavaScript
Speed (executes on the client side), simplicity, versatility (frontend and backend), and a massive ecosystem of libraries/frameworks.

### Limitations of JavaScript
Client-side security (code is visible), browser inconsistencies (historically, though mostly resolved now), and single-threaded nature (can block the UI if not careful).


## 2. JavaScript Versions and History

JavaScript was created by Brendan Eich in 1995 in just 10 days for the Netscape Navigator browser. To standardize the language, it was handed over to ECMA International.

**ECMAScript:** The official standard specification that JavaScript is based on.

| Version | Year | Major Features | Importance |
| ------- | ---- | -------------- | ---------- |
| **ES1-3** | 1997-1999 | Basic syntax, regex, try/catch. | Foundation of JS. |
| **ES5** | 2009 | strict mode, JSON support, Array methods (`forEach`, `map`). | Modernized the language. |
| **ES6 / ES2015**| 2015 | `let`, `const`, Arrow functions, Classes, Promises, Modules. | The biggest update in JS history. |
| **ES2016** | 2016 | `Array.includes()`, exponentiation operator (`**`). | Smaller iterative updates begin. |
| **ES2017** | 2017 | `async/await`, `Object.entries()`. | Revolutionized async programming. |
| **ES2018-2022**| 2018-2022| Rest/Spread for objects, Optional chaining (`?.`), Nullish coalescing (`??`), `Promise.allSettled()`. | Quality of life improvements. |
| **ES2023+**| Present | Array `findLast()`, Records & Tuples (upcoming). | Continuous evolution (Modern JS). |


## 3. Ways to Add JavaScript

### Inline JavaScript
Placed directly inside HTML tags using event attributes. (Not recommended).
```html
<button onclick="alert('Hello!')">Click Me</button>
```

### Internal JavaScript
Placed inside a `<script>` tag within the HTML file.
```html
<script>
    console.log("Hello World");
</script>
```

### External JavaScript
Linked from an external `.js` file. (Best practice).
```html
<script src="script.js"></script>
```

### Script Tag Attributes
*   **`defer`:** The script is downloaded asynchronously while HTML parses, but executed *after* HTML parsing is fully complete. Maintains script order.
*   **`async`:** The script is downloaded asynchronously and executed immediately upon download, pausing HTML parsing. Does not guarantee order.

```html
<script src="script.js" defer></script>
```


## 4. JavaScript Syntax Basics

*   **Statements:** Instructions performed by the computer.
*   **Semicolons:** Used to separate statements. JS has Automatic Semicolon Insertion (ASI), but using them explicitly is best practice.
*   **Comments:** `// single line` or `/* multi line */`.
*   **Case sensitivity:** `Name` and `name` are different variables.
*   **Whitespace:** Ignored by JS, used for readability.
*   **Blocks:** Code grouped inside curly braces `{ ... }`.
*   **Expressions:** Code that resolves to a value (e.g., `5 + 5`).
*   **Identifiers:** Names given to variables and functions (must start with a letter, `_`, or `$`).

```js
let name = "Tushar";
console.log(name);
```


## 5. Variables

A variable is a container for storing data values.

### `var`, `let`, and `const`
*   `var`: Old way to declare variables. Function-scoped. Avoid using in modern JS.
*   `let`: Modern way. Block-scoped. Can be reassigned.
*   `const`: Modern way. Block-scoped. Cannot be reassigned.

| Feature | `var` | `let` | `const` |
| :--- | :--- | :--- | :--- |
| **Scope** | Function / Global | Block | Block |
| **Redeclaration** | Yes | No | No |
| **Reassignment** | Yes | Yes | No |
| **Hoisted** | Yes (initialized as `undefined`) | Yes (Temporal Dead Zone) | Yes (Temporal Dead Zone) |

### Best Practices
Always use `const` by default. If you know the value will change, use `let`. Never use `var`.


## 6. Data Types

JavaScript is dynamically typed (types are assigned at runtime).

### Primitive Data Types
Stored directly in the location that the variable accesses.
*   **String:** Text data (`"Hello"`).
*   **Number:** Integers and floats (`42`, `3.14`).
*   **BigInt:** For numbers larger than `Number.MAX_SAFE_INTEGER`.
*   **Boolean:** Logical entity (`true` or `false`).
*   **Undefined:** A variable declared but not assigned a value.
*   **Null:** Intentional absence of any object value.
*   **Symbol:** Unique and immutable identifier (advanced use cases).

### Non-Primitive (Reference) Data Types
Stored in the heap. The variable holds a pointer to the location in memory.
*   **Object:** Key-value pairs (`{name: "John"}`).
*   **Array:** Ordered list of values (`[1, 2, 3]`).
*   **Function:** A callable object.
*   **Date, Map, Set:** Specialized objects.

### Type Conversion and Coercion
*   **Conversion:** Explicitly changing types (e.g., `Number("5")`).
*   **Coercion:** JS automatically converting types behind the scenes (e.g., `"5" + 5` results in `"55"`).
*   **Truthy/Falsy:** Values that evaluate to `true` or `false` in a boolean context. Falsy values are: `false`, `0`, `""`, `null`, `undefined`, `NaN`. Everything else is truthy.


## 7. Operators

*   **Arithmetic:** `+` (add), `-` (subtract), `*` (multiply), `/` (divide), `%` (remainder), `**` (exponentiation).
*   **Assignment:** `=`, `+=`, `-=`, `*=`, `/=`.
*   **Comparison:** `==` (loose equal), `===` (strict equal, checks type), `!=`, `!==`, `>`, `<`, `>=`, `<=`.
*   **Logical:** `&&` (AND), `||` (OR), `!` (NOT).
*   **String:** `+` concatenates strings.
*   **Ternary:** `condition ? exprIfTrue : exprIfFalse`.
*   **Nullish Coalescing (`??`):** Returns right side if left side is `null` or `undefined`.
*   **Optional Chaining (`?.`):** Safely accesses nested object properties without throwing an error if a reference is missing.
*   **Spread (`...`):** Expands an iterable into individual elements.
*   **Rest (`...`):** Condenses multiple elements into an array.
*   **Type Operators:** `typeof` (returns type string), `instanceof` (checks object prototype).
*   **`delete` / `in`:** `delete obj.prop` removes a property. `"prop" in obj` checks if it exists.


## 8. Strings

Strings are sequences of characters. They are **immutable** (cannot be changed once created).

### Creation
```js
let single = 'Single quotes';
let double = "Double quotes";
let template = `Template literal allows ${single}`; // String interpolation
```

### String Methods
*   `length`: Property returning character count.
*   `charAt(index)` / `at(index)`: Gets character at index. (`at` supports negative indexes).
*   `indexOf(str)` / `lastIndexOf(str)`: Finds position of substring.
*   `includes(str)`: Returns true if string contains substring.
*   `startsWith(str)` / `endsWith(str)`: Checks start/end.
*   `slice(start, end)`: Extracts a section of a string.
*   `toUpperCase()` / `toLowerCase()`: Changes case.
*   `trim()` / `trimStart()` / `trimEnd()`: Removes whitespace.
*   `replace(search, replace)` / `replaceAll()`: Replaces substring(s).
*   `split(separator)`: Splits string into an array of substrings.
*   `repeat(count)`: Repeats the string.


## 9. Numbers and Math

JS has only one number type, which includes both integers and floating-point decimals.

*   `NaN`: Not-a-Number (result of an invalid math operation).
*   `Infinity`: Result of dividing by zero.
*   **Precision:** JS uses 64-bit floating-point format, which can cause precision issues (`0.1 + 0.2 === 0.30000000000000004`).

### Number Methods
*   `Number("5")`: Converts string to number.
*   `parseInt("10.5")`: Parses integer (returns 10).
*   `parseFloat("10.5")`: Parses float.
*   `isNaN(val)`: Checks if value is NaN.
*   `toFixed(decimals)`: Formats number to specified decimal places (returns string).

### Math Object
*   `Math.round(x)`: Rounds to nearest integer.
*   `Math.floor(x)`: Rounds down.
*   `Math.ceil(x)`: Rounds up.
*   `Math.random()`: Random number between 0 (inclusive) and 1 (exclusive).
*   `Math.max(x, y, ...)` / `Math.min(x, y, ...)`: Finds highest/lowest.


## 10. Booleans and Conditions

### Conditional Statements
*   `if`: Executes block if condition is true.
*   `else`: Executes block if `if` condition is false.
*   `else if`: Specifies a new condition to test.
*   `switch`: Evaluates an expression, matching its value to `case` clauses.

```js
let age = 18;
if (age < 18) {
    console.log("Minor");
} else if (age === 18) {
    console.log("Just became adult");
} else {
    console.log("Adult");
}
```


## 11. Loops

Loops execute a block of code repeatedly.

*   `for`: Loops a specific number of times.
*   `while`: Loops while a condition is true.
*   `do...while`: Loops at least once, then repeats while condition is true.
*   `for...of`: Loops through the values of an iterable (Arrays, Strings).
*   `for...in`: Loops through the properties (keys) of an Object.
*   `break`: Exits the loop entirely.
*   `continue`: Skips the current iteration and moves to the next.

```js
const fruits = ["Apple", "Banana"];
for (const fruit of fruits) {
    console.log(fruit);
}
```


## 12. Functions

A function is a reusable block of code designed to perform a particular task.

*   **Function Declaration:** Hoisted to the top.
    ```js
    function greet(name) { return "Hello " + name; }
    ```
*   **Function Expression:** Stored in a variable. Not hoisted.
    ```js
    const greet = function(name) { return "Hello " + name; };
    ```
*   **Arrow Function (ES6):** Shorter syntax, does not bind its own `this`.
    ```js
    const greet = (name) => "Hello " + name;
    ```
*   **Parameters vs Arguments:** Parameters are the names in the definition; arguments are the real values passed in.
*   **Default Parameters:** `function greet(name = "Guest") { ... }`
*   **Callback Function:** A function passed as an argument to another function.
*   **Higher-Order Function:** A function that takes another function as an argument, or returns a function.
*   **IIFE (Immediately Invoked Function Expression):** Executes immediately after being created. `(function() { ... })();`


## 13. Scope

Scope determines the accessibility (visibility) of variables.

*   **Global Scope:** Variables declared outside any function or block. Accessible everywhere.
*   **Function Scope:** Variables declared inside a function (using `var`, `let`, `const`). Only accessible inside that function.
*   **Block Scope:** Variables declared inside a `{}` block (only applies to `let` and `const`).
*   **Lexical Scope:** Inner functions have access to variables in their outer functions' scopes.
*   **Closures:** A function bundled together with references to its surrounding state (lexical environment). It gives you access to an outer function's scope from an inner function, even after the outer function has finished executing.


## 14. Hoisting

Hoisting is JavaScript's default behavior of moving declarations to the top of the current scope before execution.

*   **`var`:** Hoisted and initialized with `undefined`. Accessing it before declaration returns `undefined`.
*   **`let` and `const`:** Hoisted, but NOT initialized. Accessing them before declaration causes a `ReferenceError` (they are in the Temporal Dead Zone).
*   **Function Declarations:** Completely hoisted. Can be called before they are defined.
*   **Function Expressions:** Treated like variables. Only the variable name is hoisted, not the function body.


## 15. Arrays

An array is a special variable that can hold more than one value at a time.

```js
const cars = ["Saab", "Volvo", "BMW"];
console.log(cars[0]); // Accessing: Saab
cars[0] = "Opel"; // Updating
```

### Mutating Array Methods (Change original array)
*   `push()`: Adds element to end.
*   `pop()`: Removes element from end.
*   `unshift()`: Adds element to beginning.
*   `shift()`: Removes element from beginning.
*   `splice(start, deleteCount, items...)`: Adds/removes items at specific index.
*   `sort()` / `reverse()`: Sorts/reverses the array in place.

### Non-Mutating Array Methods (Return new array/value)
*   `slice(start, end)`: Returns a shallow copy of a portion of the array.
*   `concat()`: Merges arrays.
*   `join(separator)`: Joins elements into a string.
*   `includes(val)` / `indexOf(val)`: Searches array.
*   `find()` / `findIndex()`: Finds element/index matching a condition.

### Iteration Methods (Higher-Order Functions)
*   `forEach(callback)`: Executes a provided function once for each element.
*   `map(callback)`: Creates a new array populated with the results of calling a provided function.
*   `filter(callback)`: Creates a new array with all elements that pass the test.
*   `reduce(callback, initialValue)`: Executes a reducer function on each element, resulting in a single output value.
*   `some()` / `every()`: Checks if some/all elements pass a test (returns boolean).


## 16. Objects

Objects are variables too, but they contain many values in the form of key:value pairs.

```js
const person = {
    firstName: "John",
    lastName: "Doe",
    age: 50,
    fullName: function() { return this.firstName + " " + this.lastName; }
};

// Accessing properties
console.log(person.firstName); // Dot notation
console.log(person["lastName"]); // Bracket notation (useful for dynamic keys)

// Adding/Updating
person.age = 51;
person.job = "Developer";
```

### Object Methods
*   `Object.keys(obj)`: Returns an array of keys.
*   `Object.values(obj)`: Returns an array of values.
*   `Object.entries(obj)`: Returns an array of [key, value] pairs.
*   `Object.assign(target, source)`: Copies properties from source to target.
*   `Object.freeze(obj)`: Prevents adding, deleting, or modifying properties.
*   `Object.seal(obj)`: Prevents adding/deleting properties, but allows modifying existing ones.
*   `obj.hasOwnProperty(prop)`: Checks if object has a direct property.


## 17. Destructuring

Extracts values from arrays or properties from objects into distinct variables.

```js
// Array Destructuring
const fruits = ["Apple", "Banana"];
const [fruit1, fruit2] = fruits;

// Object Destructuring
const user = { name: "Alice", age: 25 };
const { name, age, job = "Unemployed" } = user; // job gets default value
const { name: fullName } = user; // Renaming variable
```


## 18. Spread and Rest Operators

Both use the `...` syntax but act oppositely.

### Spread (Expands)
*   **Arrays:** `const arr2 = [...arr1, 4, 5];` (Copies/merges arrays)
*   **Objects:** `const obj2 = { ...obj1, newProp: true };`

### Rest (Condenses)
Used in function parameters to gather remaining arguments into an array.
```js
function sum(first, ...restOfNumbers) {
    // restOfNumbers is an array
}
```


## 19. DOM Manipulation

**DOM (Document Object Model):** The browser's internal representation of the HTML document as a tree structure of objects. JS can modify this tree.

### Selecting Elements
*   `document.getElementById("id")`
*   `document.querySelector(".class")`: Selects the first matching element.
*   `document.querySelectorAll("div")`: Selects all matching elements (NodeList).

### Modifying Elements
*   `element.innerText`: Gets/sets visible text.
*   `element.textContent`: Gets/sets all text.
*   `element.innerHTML`: Gets/sets HTML markup inside element.
*   `element.style.color = "red"`: Changes inline CSS.
*   `element.classList.add("active")`, `.remove()`, `.toggle()`: Modifies CSS classes.
*   `element.setAttribute("src", "img.png")` / `getAttribute("src")`.

### Creating/Appending Elements
```js
const newDiv = document.createElement("div");
newDiv.innerText = "Hello";
document.body.appendChild(newDiv); // Appends to body
newDiv.remove(); // Removes the element
```


## 20. Events

Events are "things" that happen to HTML elements (clicks, typing, loading).

### Event Listeners
```js
const btn = document.querySelector("button");
btn.addEventListener("click", function(event) {
    console.log("Clicked!");
    console.log(event.target); // The element that triggered the event
});
```

*   `event.preventDefault()`: Stops the default action (e.g., stops a form from submitting/refreshing page).
*   `event.stopPropagation()`: Stops event bubbling.
*   **Event Bubbling:** An event triggers on the innermost element, then bubbles up to its parents.
*   **Event Delegation:** Attaching a single event listener to a parent element to handle events on its children (useful for dynamically created elements).


## 21. Forms and Validation

Forms are how users submit data. JS can validate this data before sending it to the server.

```js
const form = document.getElementById("myForm");
form.addEventListener("submit", (e) => {
    e.preventDefault(); // Prevent page reload
    
    const email = document.getElementById("email").value;
    if (!email.includes("@")) {
        alert("Invalid email");
    } else {
        console.log("Form submitted successfully!");
    }
});
```


## 22. Browser BOM

**BOM (Browser Object Model):** Interacts with the browser outside the DOM. The global object is `window`.

*   `window.screen`: Info about user's screen.
*   `window.location`: Get current URL or redirect (`location.href = 'new.html'`).
*   `window.history`: Go back/forward (`history.back()`).
*   `window.navigator`: Info about the browser (user agent).
*   **Dialogs:** `alert()`, `confirm()` (returns true/false), `prompt()` (returns string input).
*   **Timers:** 
    *   `setTimeout(callback, delayMs)`: Runs once after delay.
    *   `setInterval(callback, intervalMs)`: Runs repeatedly.
    *   `clearTimeout(id)` / `clearInterval(id)`: Stops the timer.


## 23. Date and Time

```js
const now = new Date(); // Current date/time
const specificDate = new Date("2023-01-01");

console.log(now.getFullYear()); // 2023
console.log(now.getMonth()); // 0-11 (Jan is 0)
console.log(now.toLocaleDateString()); // Formatted string based on locale
```


## 24. Error Handling

When JS encounters an error, it usually stops executing. We can handle errors gracefully.

```js
try {
    // Code that might throw an error
    let x = y + 1; // y is not defined
} catch (error) {
    // Runs if an error occurs
    console.error("An error occurred:", error.message);
} finally {
    // Runs regardless of success or error
    console.log("Execution finished.");
}

// Custom Error
throw new Error("Invalid Input!");
```


## 25. JSON

**JSON (JavaScript Object Notation):** A lightweight data-interchange format. It looks like a JS object, but keys must be wrapped in double quotes.

*   `JSON.stringify(obj)`: Converts a JS object into a JSON string (for sending to a server/localStorage).
*   `JSON.parse(jsonString)`: Converts a JSON string back into a JS object.


## 26. Asynchronous JavaScript

JS is single-threaded (executes one command at a time). Asynchronous JS allows operations (like fetching data) to happen in the background without blocking the main thread.

*   **Event Loop / Web APIs / Callback Queue:** JS offloads async tasks (like `setTimeout` or `fetch`) to the browser's Web APIs. When finished, callbacks are pushed to the Queue, and the Event Loop pushes them back to the Call Stack when it's empty.

### Callbacks
Historically used for async logic, leading to deeply nested, unreadable code ("Callback Hell").

### Promises
An object representing the eventual completion (or failure) of an asynchronous operation.
*   **States:** Pending, Fulfilled, Rejected.
```js
fetchData()
    .then(data => console.log(data))
    .catch(error => console.error(error))
    .finally(() => console.log("Done"));
```

### Async / Await (Modern way)
Syntactic sugar over Promises. Makes async code look synchronous.
```js
async function getData() {
    try {
        const response = await fetch('url'); // execution pauses here until resolved
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}
```


## 27. Fetch API

Modern native API to make HTTP requests. It returns a Promise.

```js
// GET Request
fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => console.log(data));

// POST Request
fetch('https://api.example.com/data', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name: "Tushar" })
});
```


## 28. Local Storage and Session Storage

Store data directly in the user's browser as key-value pairs (strings only).

*   `localStorage`: Data persists even after the browser is closed.
*   `sessionStorage`: Data is cleared when the browser tab is closed.

```js
// Storing an object requires stringify
const user = { name: "Tushar" };
localStorage.setItem("user", JSON.stringify(user));

// Reading
const savedUser = JSON.parse(localStorage.getItem("user"));

// Removing
localStorage.removeItem("user");
localStorage.clear(); // Clears all
```


## 29. Cookies

Small strings of data stored in the browser, mainly used for session management (logging in) and tracking.
*   Unlike localStorage, cookies are sent to the server with every HTTP request.
*   Created/read via `document.cookie`.


## 30. ES6 and Modern JavaScript Features

A quick recap of modern features: `let/const`, Arrow Functions, Template Literals, Destructuring, Spread/Rest, Modules, Promises, Classes, `Map`/`Set`, Optional Chaining (`?.`), Nullish Coalescing (`??`), and Dynamic Imports.


## 31. Classes and Object-Oriented JavaScript

Classes in JS are syntactic sugar over the existing prototype-based inheritance.

```js
class Animal {
    constructor(name) {
        this.name = name;
    }
    speak() { console.log(this.name + " makes a noise."); }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name); // Calls parent constructor
        this.breed = breed;
    }
    speak() { console.log(this.name + " barks."); }
}

const myDog = new Dog("Rex", "German Shepherd");
myDog.speak();
```


## 32. `this` Keyword

`this` refers to the object it belongs to. Its value depends on *where* and *how* it is called.

*   In a method, `this` refers to the **owner object**.
*   Alone, `this` refers to the **global object** (Window).
*   In a function, `this` refers to the **global object** (or `undefined` in strict mode).
*   In an event, `this` refers to the **element** that received the event.
*   **Arrow functions** DO NOT bind their own `this`. They inherit `this` from the enclosing lexical scope.

Methods to explicitly bind `this`: `call()`, `apply()`, `bind()`.


## 33. Modules

Modules allow you to break code into separate files.

**file1.js (Exporting):**
```js
export const name = "Tushar"; // Named export
export default function sayHi() {} // Default export
```

**file2.js (Importing):**
```js
import sayHi, { name } from './file1.js';
```


## 34. Advanced Functions

*   **Currying:** Transforming a function with multiple arguments into a sequence of functions, each with a single argument. `func(a,b,c)` becomes `func(a)(b)(c)`.
*   **Memoization:** Caching the results of expensive function calls.
*   **Debouncing:** Ensuring a function is only executed after a certain amount of time has passed since it was last invoked (e.g., waiting for user to stop typing before searching).
*   **Throttling:** Ensuring a function is called at most once in a specified time period (e.g., handling scroll events).


## 35. Advanced Objects

*   **Shallow Copy:** Copies top-level properties. (using `...spread` or `Object.assign()`). Nested objects are still referenced.
*   **Deep Copy:** Creates a completely independent copy. (Historically `JSON.parse(JSON.stringify(obj))`, modern JS has `structuredClone(obj)`).
*   **Getters / Setters:** `get propName() {}` and `set propName(val) {}` allow defining methods that are accessed like properties.


## 36. Map, Set, WeakMap, WeakSet

*   **Map:** Holds key-value pairs where keys can be *any* data type (unlike objects where keys must be strings/symbols). Maintains insertion order.
*   **Set:** Collection of *unique* values. Prevents duplicates automatically.
*   **WeakMap / WeakSet:** Similar to Map/Set but keys/values must be objects, and they do not prevent garbage collection of those objects.


## 37. Regular Expressions

Patterns used to match character combinations in strings. Created between slashes `/pattern/flags`.

*   `/hello/i`: Case-insensitive match.
*   `/^[a-z]+$/`: Matches only lowercase letters.
*   Methods: `regex.test("string")` (returns boolean), `string.match(regex)` (returns array).


## 38. Browser APIs Overview

*   **DOM / BOM / Fetch API:** Covered previously.
*   **Geolocation API:** Gets user's physical location (requires permission).
*   **Canvas API:** Draw 2D/3D graphics via JS.
*   **Web Workers:** Run JS scripts in background threads (prevents UI blocking).


## 39. JavaScript in Backend

JavaScript isn't just for browsers anymore. 
**Node.js** is a runtime environment that allows you to run JS on a server. It uses the V8 engine and has built-in modules for handling file systems, networks, and HTTP requests. **npm** (Node Package Manager) is used to install third-party libraries.


## 40. JavaScript Best Practices

*   Use `const` for variables that don't change, `let` for variables that do. Avoid `var`.
*   Use strict equality (`===`) instead of loose equality (`==`).
*   Keep functions small and focused on a single task.
*   Avoid global variables to prevent scope pollution.
*   Handle errors properly using `try/catch` or `.catch()` on promises.
*   Use descriptive variable and function names.


## 41. Common Beginner Mistakes

*   **Confusing `==` and `===`:** Always use `===`.
*   **Accidentally mutating arrays:** E.g., using `sort()` modifies the original array. Use `[...arr].sort()` if you need a copy.
*   **Losing `this` context:** When passing object methods as callbacks, `this` is lost. Use `.bind(this)` or arrow functions.
*   **Not understanding async behavior:** Trying to return a value from a `fetch` directly instead of using `.then` or `await`.
*   **Accessing DOM before it loads:** Put `<script defer>` in `<head>` or put `<script>` at the end of the `<body>`.


## 42. Practical JavaScript Projects

### 1. Simple Counter App
```html
<button id="dec">-</button>
<span id="count">0</span>
<button id="inc">+</button>

<script>
    let count = 0;
    const countDisplay = document.getElementById('count');
    document.getElementById('inc').addEventListener('click', () => {
        count++;
        countDisplay.innerText = count;
    });
    document.getElementById('dec').addEventListener('click', () => {
        count--;
        countDisplay.innerText = count;
    });
</script>
```
*Concept:* DOM selection, event listeners, state manipulation.


## 43. JavaScript Cheat Sheet

| Category | Important Examples / Methods |
| :--- | :--- |
| **Variables** | `let`, `const` |
| **Data Types**| String, Number, Boolean, Object, Array, Null, Undefined |
| **Arrays** | `.push()`, `.pop()`, `.map()`, `.filter()`, `.reduce()` |
| **Objects** | `Object.keys()`, `Object.values()`, destructuring |
| **DOM** | `querySelector()`, `addEventListener()`, `createElement()` |
| **Async** | `fetch()`, `Promises`, `async / await` |
| **Storage** | `localStorage.setItem(key, val)`, `localStorage.getItem(key)` |

---
*End of JavaScript Complete Notes.*
