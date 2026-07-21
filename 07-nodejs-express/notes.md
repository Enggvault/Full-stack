title: "Node.js & Express: Complete Beginner to Advanced"
subtitle: "From First Principles to Production-Grade Architecture"
author: "Principal Node.js Engineer — 15+ Years Industry Experience"
version: "2.0"
date: "2025"

# Node.js & Express
## Complete Beginner to Advanced Engineering Handbook

> A production-grade, book-quality reference covering Node.js internals, Express routing, middleware, authentication, and project architecture. Written for engineers at all levels — from beginners to FAANG system designers.

> **Prerequisites:** [06 — API Design ←](../06-api-design/notes.md) · **Next:** [08 — Databases →](../08-databases/notes.md)


## Table of Contents

### Part I: Node.js Foundations
- [Chapter 1: Introduction to Node.js](#chapter-1-introduction-to-nodejs)
- [Chapter 2: Installing Node.js](#chapter-2-installing-nodejs)
- [Chapter 3: Node.js Internals](#chapter-3-nodejs-internals)

### Part II: Core Ecosystem
- [Chapter 4: Modules](#chapter-4-modules)
- [Chapter 5: npm Deep Dive](#chapter-5-npm-deep-dive)

### Part III: Express.js Mastery
- [Chapter 6: Express.js Introduction](#chapter-6-expressjs-introduction)
- [Chapter 7: Express Application Structure](#chapter-7-express-application-structure)
- [Chapter 8: Routing](#chapter-8-routing)
- [Chapter 9: Request & Response Objects](#chapter-9-request--response-objects)
- [Chapter 10: Middleware Deep Dive](#chapter-10-middleware-deep-dive)
- [Chapter 11: Static Files](#chapter-11-static-files)
- [Chapter 12: Template Engines](#chapter-12-template-engines)
- [Chapter 13: Environment Variables](#chapter-13-environment-variables)

### Part IV: Production Engineering
- [Chapter 14: Error Handling](#chapter-14-error-handling)
- [Chapter 15: Async Programming](#chapter-15-async-programming)
- [Chapter 16: File System Module](#chapter-16-file-system-module)
- [Chapter 17: Path Module](#chapter-17-path-module)
- [Chapter 18: HTTP Module](#chapter-18-http-module)
- [Chapter 19: Events](#chapter-19-events)
- [Chapter 20: Streams](#chapter-20-streams)
- [Chapter 21: Buffers](#chapter-21-buffers)

### Part V: Real-World Application
- [Chapter 22: Authentication Basics](#chapter-22-authentication-basics)
- [Chapter 23: REST API Best Practices](#chapter-23-rest-api-best-practices)
- [Chapter 24: Express Project — Student Management API](#chapter-24-express-project--student-management-api)

### Part VI: Reference
- [Chapter 25: Interview Questions](#chapter-25-interview-questions)
- [Chapter 26: Best Practices](#chapter-26-best-practices)
- [Chapter 27: Common Mistakes](#chapter-27-common-mistakes)
- [Chapter 28: Cheat Sheet](#chapter-28-cheat-sheet)


# CHAPTER 1: Introduction to Node.js

### What is Node.js?

**Node.js** is a **runtime environment** that lets you run JavaScript code **outside of a web browser** — directly on a computer or server.

Before Node.js existed, JavaScript could only run inside browsers (Chrome, Firefox, Safari). Node.js changed that by taking V8, Chrome's JavaScript engine, and embedding it into a standalone program you can install on any machine.

> **Analogy:** Think of JavaScript as a language that only a specific person (the browser) could speak. Node.js trained a new person (the server) to also speak that language — now JavaScript can work everywhere.

### History

| Year | Event |
|:-----|:------|
| **1995** | Brendan Eich creates JavaScript in 10 days for Netscape |
| **2008** | Google open-sources V8, the fastest JS engine ever built |
| **2009** | Ryan Dahl creates Node.js, embedding V8 into a server runtime |
| **2010** | npm (Node Package Manager) is released |
| **2015** | Node.js Foundation formed; io.js merges back into Node |
| **2020s** | Node.js powers millions of production servers globally |

### Why Was Node.js Created?

The web in 2009 had a fundamental problem: **servers blocked**.

Traditional web servers (Apache, PHP) handled each request with a dedicated thread. When that request waited for a database query, the thread sat idle — doing nothing but consuming memory.

Ryan Dahl's insight: **"What if the server never waited? What if it handled other requests while waiting for I/O?"**

Node.js was his answer — a server that is **event-driven** and **non-blocking**. It processes many requests concurrently using a single thread and an event loop.

```
Traditional Server (Blocking):
  Request 1 → Thread 1 → [waiting for DB... 200ms] → Response
  Request 2 → Thread 2 → [waiting for DB... 200ms] → Response
  Request 3 → Thread 3 → [waiting for DB... 200ms] → Response
  (Need 3 threads for 3 concurrent requests)

Node.js Server (Non-Blocking):
  Request 1 → [ask DB] → move on
  Request 2 → [ask DB] → move on
  Request 3 → [ask DB] → move on
  DB returns for Request 1 → send response
  DB returns for Request 2 → send response
  DB returns for Request 3 → send response
  (One thread handles all 3 concurrently)
```

### The V8 Engine

**V8** is Google's open-source JavaScript engine, written in C++. It:
- Compiles JavaScript directly to native machine code (not interpreted line by line)
- Applies aggressive optimizations at runtime
- Powers both Google Chrome and Node.js

Node.js uses V8 to execute JavaScript, then adds its own layer (written in C++) for:
- File system access
- Network sockets
- OS APIs
- The event loop (via libuv)

### Runtime vs Browser

| Feature | Browser | Node.js |
|:--------|:--------|:--------|
| **JavaScript Engine** | V8 (Chrome), SpiderMonkey (Firefox) | V8 |
| **DOM** | ✅ Yes — `document`, `window` | ❌ No |
| **File System** | ❌ No (security restriction) | ✅ Yes — `fs` module |
| **Network** | Fetch API | `http` / `https` module |
| **Global object** | `window` | `global` |
| **Module system** | ES Modules (`import/export`) | CommonJS + ES Modules |
| **Purpose** | Running UI code in the browser | Running server code on the machine |

### Single-Threaded

Node.js runs your JavaScript code on a **single thread** — meaning only one piece of JavaScript executes at a time.

> **Wait, doesn't that make it slow?**

No — because Node.js **never blocks** that thread waiting for slow operations. When it needs to read a file or query a database, it delegates that work to the OS (via libuv's thread pool) and continues executing other JavaScript. When the result is ready, it picks it back up.

### Event-Driven Architecture

Node.js is **event-driven** — it reacts to events rather than following a sequential script.

```
Events → Event Queue → Event Loop → Your Code
```

Examples of events:
- HTTP request arrives
- File read completes
- Timer fires (`setTimeout`)
- Database query returns
- User sends a message via WebSocket

### Non-Blocking I/O

**I/O** = Input/Output — reading/writing files, network requests, database queries. These are the **slow operations** in a server.

**Blocking I/O:** the thread stops and waits for the operation to finish.
**Non-blocking I/O:** the operation is started, and the thread moves on. A callback fires when it's done.

```javascript
// Blocking (bad in a server context)
const data = fs.readFileSync('huge-file.txt'); // Thread freezes here
console.log('Done');

// Non-blocking (correct Node.js style)
fs.readFile('huge-file.txt', (err, data) => {
  console.log('Done');  // Called when file is ready
});
console.log('Continuing...'); // Runs immediately, before file is read
```

### Advantages of Node.js

| Advantage | Detail |
|:----------|:-------|
| **High concurrency** | Handles thousands of concurrent connections with one thread |
| **Same language everywhere** | JavaScript on both frontend and backend |
| **Fast startup** | No compilation step; starts in milliseconds |
| **Huge ecosystem** | npm has over 2 million packages |
| **Real-time capable** | WebSockets and SSE work naturally |
| **JSON-native** | JavaScript and JSON are the same mental model |

### Limitations of Node.js

| Limitation | Why | Workaround |
|:-----------|:----|:-----------|
| **CPU-intensive tasks** | Blocks the single thread | Worker Threads, separate microservice |
| **Callback hell** | Deep nesting of async code | Promises, async/await |
| **Not ideal for heavy computation** | Single-threaded | Go, Rust, or a dedicated worker |
| **Immaturity of some packages** | Anyone can publish to npm | Vet dependencies carefully |

### Use Cases

**Node.js excels at:**
- REST APIs and web servers
- Real-time applications (chat, live dashboards)
- API gateways and proxies
- Streaming services
- CLI tools
- Microservices

**Avoid Node.js for:**
- Video encoding
- Machine learning inference
- Heavy mathematical computation


# CHAPTER 2: Installing Node.js

### Direct Installation

Download the LTS (Long-Term Support) version from [nodejs.org](https://nodejs.org).

| Platform | Installation |
|:---------|:------------|
| **macOS** | Download `.pkg` installer, or `brew install node` |
| **Linux** | Use NodeSource package or nvm (preferred) |
| **Windows** | Download `.msi` installer from nodejs.org |

### Version Manager — nvm (Recommended)

**nvm** (Node Version Manager) lets you install and switch between multiple Node.js versions. This is essential for professional development — different projects may require different Node versions.

**Install nvm (macOS / Linux):**
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

**Reload your shell:**
```bash
source ~/.bashrc   # or ~/.zshrc
```

**Common nvm commands:**
```bash
nvm install 20          # Install Node.js version 20 (LTS)
nvm install --lts       # Install the latest LTS version
nvm use 20              # Switch to Node 20 in this terminal
nvm alias default 20    # Make Node 20 the default
nvm ls                  # List installed versions
nvm ls-remote           # List all available versions
```

### Verifying the Installation

```bash
node -v       # e.g., v20.11.0
npm -v        # e.g., 10.2.4
npx --version # e.g., 10.2.4
```

### npm (Node Package Manager)

**npm** is the default package manager for Node.js. It ships with Node.js automatically.

npm allows you to:
- Install third-party libraries (Express, Lodash, Joi)
- Manage project dependencies
- Run scripts (`npm start`, `npm test`)
- Publish your own packages

### npx

**npx** runs an npm package without installing it globally.

```bash
# Without npx: install globally, then run
npm install -g create-react-app
create-react-app my-app

# With npx: run directly (always uses latest version)
npx create-react-app my-app
```

Use npx for one-time scaffolding commands.

### Alternative Package Managers

| Manager | Speed | Strengths |
|:--------|:------|:----------|
| **npm** | Baseline | Ships with Node; universal compatibility |
| **yarn** | Faster | Deterministic lockfile; workspaces |
| **pnpm** | Fastest | Shared node_modules store; disk efficient |


# CHAPTER 3: Node.js Internals

This chapter explains how Node.js actually works under the hood — knowledge that separates beginners from professionals.

### The Big Picture

```
┌──────────────────────────────────────────────────┐
│                Your JavaScript Code              │
├──────────────────────────────────────────────────┤
│            Node.js APIs (Core Modules)           │
│  (fs, http, path, events, crypto, stream, ...)   │
├──────────────────────────────────────────────────┤
│                   Node.js Bindings               │
│         (C++ layer bridging JS and libuv)        │
├──────────────────────────────────────────────────┤
│         V8 Engine          │        libuv         │
│  (Executes JavaScript)     │  (Async I/O, Event  │
│                            │   Loop, Thread Pool) │
└──────────────────────────────────────────────────┘
```

### The Call Stack

The **Call Stack** is where JavaScript keeps track of what function is currently executing.

```javascript
function greet(name) {
  return `Hello, ${name}`;
}

function main() {
  const message = greet('Alice');
  console.log(message);
}

main();
```

**Call Stack execution:**
```
main() added to stack
  greet('Alice') added to stack
  greet returns 'Hello, Alice' → removed from stack
  console.log() added to stack
  console.log prints → removed from stack
main returns → removed from stack
Stack is empty
```

> **Rule:** JavaScript can only execute one thing at a time. The call stack processes one frame at a time.

### libuv

**libuv** is a C library that gives Node.js its superpowers. It provides:
- The **Event Loop** — the core mechanism driving async behavior
- A **Thread Pool** (4 threads by default) for expensive I/O operations
- Cross-platform OS abstractions (file system, networking, timers)

### The Event Loop

The **Event Loop** is the heart of Node.js. It continuously checks: "Is the call stack empty? Are there callbacks waiting to run?"

```
┌─────────────────────────────────────────────────────────┐
│                     EVENT LOOP                          │
│                                                         │
│  ┌─────────┐    ┌───────────────────────────────────┐  │
│  │  timers  │ → │ setTimeout / setInterval callbacks │  │
│  └─────────┘    └───────────────────────────────────┘  │
│       ↓                                                 │
│  ┌──────────────┐    ┌──────────────────────────────┐  │
│  │ pending I/O  │ → │ I/O callbacks from prev cycle │  │
│  └──────────────┘    └──────────────────────────────┘  │
│       ↓                                                 │
│  ┌──────────┐    ┌────────────────────────────────────┐ │
│  │   idle   │ → │ setImmediate prep                  │ │
│  └──────────┘    └────────────────────────────────────┘ │
│       ↓                                                 │
│  ┌──────────┐    ┌────────────────────────────────────┐ │
│  │   poll   │ → │ New I/O events; execute callbacks  │ │
│  └──────────┘    └────────────────────────────────────┘ │
│       ↓                                                 │
│  ┌──────────┐    ┌────────────────────────────────────┐ │
│  │  check   │ → │ setImmediate callbacks             │ │
│  └──────────┘    └────────────────────────────────────┘ │
│       ↓                                                 │
│  ┌───────────────┐  ┌──────────────────────────────┐   │
│  │ close events  │→ │ socket.close() callbacks     │   │
│  └───────────────┘  └──────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

**Between each phase**, the event loop checks the **microtask queue**:
- `Promise.then()` callbacks
- `process.nextTick()` callbacks

Microtasks always run **before** the next event loop phase.

### Queues — Priority Order

```
Highest priority
      │
      ▼
process.nextTick() queue    ← runs after current op, before anything else
      │
      ▼
Promise microtask queue     ← Promise.then(), async/await continuations
      │
      ▼
Timers phase                ← setTimeout, setInterval
      │
      ▼
I/O callbacks               ← fs.readFile, network
      │
      ▼
setImmediate               ← runs after I/O in the check phase
      │
      ▼
Lowest priority
```

### Execution Order Demo

```javascript
console.log('1: Start');

setTimeout(() => console.log('5: setTimeout'), 0);

setImmediate(() => console.log('6: setImmediate'));

Promise.resolve().then(() => console.log('3: Promise microtask'));

process.nextTick(() => console.log('2: nextTick'));

console.log('4: End');
```

**Output:**
```
1: Start
4: End
2: nextTick          ← nextTick queue (before microtasks)
3: Promise microtask ← Promise queue
5: setTimeout        ← timers phase
6: setImmediate      ← check phase
```

> **Key insight:** `process.nextTick` and Promises run before `setTimeout` even when `setTimeout` has a delay of 0.

### The Thread Pool

Some operations are too expensive or impossible to do non-blockingly at the OS level (file system encryption, DNS lookup, compression). libuv offloads these to a **thread pool** of 4 worker threads (configurable).

```
Your JS Code (Main Thread)
    │
    ├── fs.readFile()  ─────────────────────→  Thread Pool Worker 1
    │                                              [reads file from disk]
    ├── crypto.pbkdf2() ────────────────────→  Thread Pool Worker 2
    │                                              [hashes password]
    │
    │   (Main thread continues running)
    │
    ←───────────────────── Worker 1 done: fires callback
    ←───────────────────── Worker 2 done: fires callback
```

Thread pool operations: `fs` (most), `crypto`, `dns.lookup()`, `zlib`, `child_process`.

Network I/O (TCP, HTTP) does **not** use the thread pool — the OS handles it natively with kernel-level async calls.

### Timers

```javascript
// setTimeout — run callback after at least N milliseconds
setTimeout(() => {
  console.log('At least 1000ms have passed');
}, 1000);

// setInterval — run callback every N milliseconds
const interval = setInterval(() => {
  console.log('Runs every 500ms');
}, 500);

// Clear after 3 seconds
setTimeout(() => clearInterval(interval), 3000);

// setImmediate — run in the check phase of the current event loop tick
setImmediate(() => {
  console.log('Runs in the check phase');
});
```


# CHAPTER 4: Modules

A **module** is a reusable piece of JavaScript code contained in its own file. Modules help you organize code, avoid naming conflicts, and share functionality between files.

Node.js supports two module systems:
1. **CommonJS (CJS)** — the original Node.js module system
2. **ES Modules (ESM)** — the modern JavaScript standard

### CommonJS (CJS)

CommonJS is the default in Node.js when using `.js` files without `"type": "module"` in `package.json`.

**Exporting from a module:**

```javascript
// math.js

// Export a single function
function add(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

// Export multiple things via module.exports object
module.exports = { add, multiply };

// Alternative: export one thing as the default
// module.exports = add;
```

**Importing a module:**

```javascript
// app.js

const { add, multiply } = require('./math');
// or: const math = require('./math'); then math.add(...)

console.log(add(2, 3));      // 5
console.log(multiply(4, 5)); // 20
```

**Line-by-line explanation:**
- `module.exports` — every file in Node.js has a `module` object with an `exports` property
- `require('./math')` — Node.js reads the file at `./math.js`, executes it, and returns `module.exports`
- The `./` prefix means the file is in the same directory; omit it for core/npm modules

**Built-in core module (no path prefix needed):**
```javascript
const path = require('path');     // Node.js built-in
const express = require('express'); // npm package
const myModule = require('./my-module'); // Local file
```

**CommonJS is synchronous.** When `require()` is called, Node.js:
1. Resolves the file path
2. Reads the file
3. Wraps it in a function
4. Executes it
5. Returns `module.exports`

Node.js **caches** modules — requiring the same file twice returns the same object.

### ES Modules (ESM)

ES Modules are the official JavaScript standard (ES2015+). They use `import` and `export` syntax.

To use ESM in Node.js, either:
- Name the file with `.mjs` extension, OR
- Add `"type": "module"` to `package.json`

**Exporting:**

```javascript
// math.mjs (or math.js with "type": "module")

export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

// Default export
export default function divide(a, b) {
  return a / b;
}
```

**Importing:**

```javascript
// app.mjs

import { add, multiply } from './math.mjs';
import divide from './math.mjs'; // default import

console.log(add(2, 3));      // 5
console.log(divide(10, 2));  // 5
```

### CommonJS vs ES Modules

| Feature | CommonJS (CJS) | ES Modules (ESM) |
|:--------|:---------------|:-----------------|
| **Syntax** | `require()` / `module.exports` | `import` / `export` |
| **Loading** | Synchronous | Asynchronous |
| **File extension** | `.js` (default) | `.mjs` or `.js` with `"type":"module"` |
| **Top-level `await`** | ❌ Not supported | ✅ Supported |
| **Tree-shaking** | ❌ Not possible | ✅ Bundlers can eliminate dead code |
| **`__dirname`** | ✅ Available | ❌ Not available (use `import.meta.url`) |
| **Dynamic import** | `require()` anywhere | `await import()` |
| **Industry status** | Legacy (still dominant in Node) | Modern standard |

### When to Use Which?

- **Use CommonJS** when: working in an existing Node.js project, using packages that don't support ESM, writing simple scripts
- **Use ESM** when: starting a new project, working with modern tooling (Vite, Rollup), want tree-shaking

> **For this guide, all examples use CommonJS** (`require`/`module.exports`) as it remains the dominant convention in Node.js/Express projects.

---

# CHAPTER 5: npm Deep Dive

### package.json

`package.json` is the **manifest file** for every Node.js project. It records:
- Project metadata (name, version, description)
- Dependencies
- Scripts
- Entry point

**Create a new project:**
```bash
mkdir my-api
cd my-api
npm init -y    # -y answers all prompts with defaults
```

**Example package.json:**
```json
{
  "name": "my-api",
  "version": "1.0.0",
  "description": "A RESTful API built with Express",
  "main": "server.js",
  "scripts": {
    "start":   "node server.js",
    "dev":     "nodemon server.js",
    "test":    "jest"
  },
  "dependencies": {
    "express": "^4.18.2",
    "dotenv":  "^16.3.1"
  },
  "devDependencies": {
    "nodemon": "^3.0.1",
    "jest":    "^29.7.0"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
```

**Field explanations:**

| Field | Purpose |
|:------|:--------|
| `name` | Package name (lowercase, no spaces) |
| `version` | Current version following semver |
| `main` | Entry point file |
| `scripts` | Commands you run with `npm run <script>` |
| `dependencies` | Packages required in production |
| `devDependencies` | Packages only needed during development |
| `engines` | Minimum Node.js version required |

### Semantic Versioning (semver)

All npm packages follow the `MAJOR.MINOR.PATCH` format:

```
  1  .  4  .  3
  │     │     │
  │     │     └── PATCH: Bug fix (backward compatible)
  │     └──────── MINOR: New feature (backward compatible)
  └────────────── MAJOR: Breaking change
```

**Version prefix symbols in package.json:**

| Symbol | Meaning | Example: `^4.18.2` |
|:-------|:--------|:--------------------|
| `^` (caret) | Allow MINOR and PATCH updates | Installs `4.x.x` (not 5.x.x) |
| `~` (tilde) | Allow PATCH updates only | Installs `4.18.x` (not 4.19.x) |
| No prefix | Exact version only | Installs exactly `4.18.2` |
| `*` | Any version | Installs latest (dangerous!) |

### package-lock.json

When you run `npm install`, npm creates `package-lock.json`. This file records the **exact** version of every installed package and its dependencies.

- `package.json` says `"express": "^4.18.2"` (flexible)
- `package-lock.json` records exactly `4.18.3` was installed

**Why it matters:** Your team and CI servers install the same versions, avoiding "works on my machine" bugs.

> **Rule:** Always commit `package-lock.json` to version control. Never commit `node_modules/`.

### dependency types

```bash
npm install express             # Production dependency → goes into "dependencies"
npm install nodemon --save-dev  # Dev-only dependency   → goes into "devDependencies"
npm install -g typescript       # Global installation   → available as a CLI command
```

**devDependencies** are not installed when someone runs `npm install --production` (e.g., on a server).

### peerDependencies

`peerDependencies` declare that your package is compatible with a specific version of another package, without installing it. Used by plugins and libraries.

```json
"peerDependencies": {
  "react": ">=17.0.0"
}
```

### Useful npm Commands

```bash
npm init -y                    # Initialize a new project
npm install express            # Install a package
npm install                    # Install all packages from package.json
npm uninstall lodash           # Remove a package
npm update                     # Update all packages within semver range
npm list                       # List installed packages
npm list --depth=0             # List only top-level packages
npm outdated                   # Show packages with newer versions
npm audit                      # Check for security vulnerabilities
npm audit fix                  # Auto-fix vulnerabilities
npm run dev                    # Run the "dev" script
npm test                       # Run the "test" script
npx nodemon server.js          # Run without installing globally
```

### node_modules

When you `npm install`, packages are downloaded into `node_modules/`. This folder:
- Can contain thousands of files
- Should **never** be committed to git
- Is always reproducible from `package.json` + `package-lock.json`

**.gitignore:**
```
node_modules/
.env
dist/
```


# CHAPTER 6: Express.js Introduction

### What is Express?

**Express.js** is a minimal, unopinionated web framework for Node.js. It wraps Node's built-in `http` module and adds:
- A clean routing API
- Middleware support
- Request/response helpers
- Template engine integration

Without Express, building an API in pure Node.js requires parsing URLs manually, reading request bodies manually, and writing verbose boilerplate. Express handles all of that cleanly.

> **Analogy:** Node.js is the engine. Express is the car body — it makes the engine usable and comfortable without hiding what's underneath.

### Installing Express

```bash
npm init -y
npm install express
```

### The Minimal Server

```javascript
// server.js

const express = require('express'); // 1. Import Express

const app = express();              // 2. Create an Express application

app.get('/', (req, res) => {        // 3. Define a route
  res.send('Hello, World!');        // 4. Send a response
});

app.listen(3000, () => {            // 5. Start the server
  console.log('Server running on http://localhost:3000');
});
```

**Line-by-line:**

| Line | Explanation |
|:-----|:------------|
| `require('express')` | Loads the Express library from node_modules |
| `express()` | Creates an Express app instance — this is your entire server |
| `app.get('/', ...)` | Registers a route: when a GET request hits `/`, run this handler |
| `(req, res)` | `req` = incoming request, `res` = outgoing response |
| `res.send('...')` | Sends the string as the response body |
| `app.listen(3000, ...)` | Binds to port 3000 and starts accepting connections |

**Run it:**
```bash
node server.js
# Open browser: http://localhost:3000
```

### How Express Works Internally

When a request arrives:

```
HTTP Request
     │
     ▼
Node.js http.createServer()
     │
     ▼
Express receives (req, res)
     │
     ▼
Middleware Stack (runs top to bottom)
  ├── CORS middleware
  ├── JSON body parser
  ├── Auth middleware
  └── ...
     │
     ▼
Router matches method + path
     │
     ▼
Route handler executes
     │
     ▼
res.json() / res.send() sends response
```

### express.json() Middleware

Without this, `req.body` is `undefined` for POST/PUT/PATCH requests:

```javascript
app.use(express.json()); // Parse JSON request bodies
```


# CHAPTER 7: Express Application Structure

### Professional Folder Structure

```
my-api/
│
├── src/
│   ├── routes/
│   │   ├── users.routes.js
│   │   └── products.routes.js
│   │
│   ├── controllers/
│   │   ├── users.controller.js
│   │   └── products.controller.js
│   │
│   ├── services/
│   │   ├── users.service.js
│   │   └── products.service.js
│   │
│   ├── middleware/
│   │   ├── auth.middleware.js
│   │   ├── validate.middleware.js
│   │   └── error.middleware.js
│   │
│   ├── config/
│   │   └── db.config.js
│   │
│   ├── models/
│   │   └── user.model.js
│   │
│   ├── utils/
│   │   └── response.util.js
│   │
│   └── app.js           ← Express app setup (no listen call)
│
├── server.js             ← Entry point (calls app.listen)
├── package.json
├── package-lock.json
└── .env
```

### What Goes Where

| Folder/File | Responsibility |
|:------------|:---------------|
| `routes/` | Define URL patterns and map them to controller functions |
| `controllers/` | Handle HTTP request/response — call service functions |
| `services/` | Business logic — validation rules, calculations, database queries |
| `models/` | Database schemas (Mongoose, Sequelize, Prisma models) |
| `middleware/` | Reusable functions that run before route handlers |
| `config/` | Database connection, environment config |
| `utils/` | Shared helpers (response formatter, date utils) |
| `app.js` | Wire everything together — mount routes, middleware |
| `server.js` | Entry point — call `app.listen()` |

### app.js

```javascript
// src/app.js

const express = require('express');
const cors = require('cors');
const usersRouter = require('./routes/users.routes');

const app = express();

// Global middleware
app.use(cors());
app.use(express.json());

// Routes
app.use('/api/v1/users', usersRouter);

// 404 handler
app.use((req, res) => {
  res.status(404).json({
    success: false,
    error: { code: 'NOT_FOUND', message: `${req.method} ${req.path} not found.` }
  });
});

// Global error handler
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.statusCode || 500).json({
    success: false,
    error: { code: err.code || 'SERVER_ERROR', message: err.message || 'Server error.' }
  });
});

module.exports = app;
```

### server.js

```javascript
// server.js

require('dotenv').config();
const app = require('./src/app');

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```


# CHAPTER 8: Routing

### Basic Routes

```javascript
const express = require('express');
const app = express();
app.use(express.json());

// GET
app.get('/users', (req, res) => {
  res.status(200).json({ success: true, data: [] });
});

// POST
app.post('/users', (req, res) => {
  res.status(201).json({ success: true, data: req.body });
});

// PUT
app.put('/users/:id', (req, res) => {
  res.status(200).json({ success: true, id: req.params.id });
});

// PATCH
app.patch('/users/:id', (req, res) => {
  res.status(200).json({ success: true, updates: req.body });
});

// DELETE
app.delete('/users/:id', (req, res) => {
  res.status(204).send();
});
```

### Express Router

For large apps, split routes into separate files using `express.Router()`:

```javascript
// src/routes/users.routes.js

const express = require('express');
const router = express.Router();
const usersController = require('../controllers/users.controller');

router.get('/',     usersController.getAll);
router.get('/:id',  usersController.getOne);
router.post('/',    usersController.create);
router.put('/:id',  usersController.replace);
router.patch('/:id',usersController.update);
router.delete('/:id',usersController.remove);

module.exports = router;
```

```javascript
// src/app.js — mount the router
app.use('/api/v1/users', usersRouter);

// Result: GET /api/v1/users maps to router.get('/')
//         GET /api/v1/users/123 maps to router.get('/:id')
```

### Dynamic Routes

```javascript
// Route parameter
app.get('/users/:id', (req, res) => {
  console.log(req.params.id); // '123'
});

// Multiple parameters
app.get('/posts/:postId/comments/:commentId', (req, res) => {
  const { postId, commentId } = req.params;
});

// Optional parameter
app.get('/users/:id?', (req, res) => {
  if (req.params.id) {
    // get one
  } else {
    // get all
  }
});
```

### Route Chaining

```javascript
// Chain handlers for the same path
app.route('/users/:id')
  .get((req, res)    => res.json({ method: 'GET' }))
  .put((req, res)    => res.json({ method: 'PUT' }))
  .patch((req, res)  => res.json({ method: 'PATCH' }))
  .delete((req, res) => res.status(204).send());
```


# CHAPTER 9: Request & Response Objects

### The Request Object (req)

```javascript
app.post('/api/v1/users', (req, res) => {
  // Body — JSON payload (requires express.json() middleware)
  console.log(req.body);         // { name: 'Alice', email: 'alice@example.com' }

  // Route parameters
  console.log(req.params);       // { id: '123' } for /users/:id
  console.log(req.params.id);    // '123'

  // Query parameters
  console.log(req.query);        // { page: '2', limit: '10' }
  console.log(req.query.page);   // '2'

  // Headers
  console.log(req.headers);                         // all headers
  console.log(req.headers['content-type']);         // 'application/json'
  console.log(req.headers['authorization']);        // 'Bearer eyJ...'
  console.log(req.get('Authorization'));            // method form

  // Client info
  console.log(req.ip);           // '127.0.0.1'
  console.log(req.method);       // 'POST'
  console.log(req.path);         // '/api/v1/users'
  console.log(req.protocol);     // 'http' or 'https'
  console.log(req.hostname);     // 'localhost'
  console.log(req.url);          // '/api/v1/users?page=1'

  // Cookies (requires cookie-parser middleware)
  console.log(req.cookies);      // { sessionId: 'abc123' }
});
```

### The Response Object (res)

```javascript
app.get('/demo', (req, res) => {
  // Send JSON (most common in APIs)
  res.status(200).json({ success: true, data: [] });

  // Send plain text or HTML
  res.status(200).send('<h1>Hello</h1>');

  // Set status code only, then chain
  res.status(404).json({ error: 'Not found' });

  // Send file for download
  res.download('/path/to/file.pdf');

  // Send a file as response
  res.sendFile('/absolute/path/to/index.html');

  // Redirect
  res.redirect(301, '/new-url');
  res.redirect('/same-status-302');

  // Set a header
  res.set('X-Custom-Header', 'value');
  res.set('Location', '/api/v1/users/99');

  // Set cookie
  res.cookie('token', 'abc123', { httpOnly: true, secure: true });

  // Clear cookie
  res.clearCookie('token');

  // End without body (for 204)
  res.status(204).send();
});
```

### Important: Always send exactly ONE response

```javascript
// ❌ Wrong — sends two responses, causes "Cannot set headers after they are sent" error
app.get('/bad', (req, res) => {
  if (someCondition) {
    res.status(404).json({ error: 'Not found' });
    // Missing return! Code continues to line below:
  }
  res.status(200).json({ data: [] }); // Error!
});

// ✅ Correct — return after sending
app.get('/good', (req, res) => {
  if (someCondition) {
    return res.status(404).json({ error: 'Not found' }); // return exits handler
  }
  res.status(200).json({ data: [] });
});
```


# CHAPTER 10: Middleware Deep Dive

### What is Middleware?

Middleware is a function that has access to `req`, `res`, and `next`. It runs **between** the incoming request and the route handler.

```
Request → [Middleware 1] → [Middleware 2] → [Middleware 3] → Route Handler → Response
```

Each middleware can:
1. Execute any code
2. Modify `req` or `res`
3. End the request-response cycle
4. Call `next()` to pass to the next middleware

### Middleware Signature

```javascript
function myMiddleware(req, res, next) {
  // Do something
  next(); // Pass control to the next middleware
  // OR
  // res.status(401).json({ error: 'Unauthorized' }); // End the cycle
}
```

### Application-Level Middleware

Runs on every request:

```javascript
// Request logger
app.use((req, res, next) => {
  console.log(`${new Date().toISOString()} ${req.method} ${req.path}`);
  next(); // Must call next() or the request hangs!
});

// Body parser (built-in)
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

### Route-Level Middleware

Runs only on specific routes:

```javascript
const authMiddleware = (req, res, next) => {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) {
    return res.status(401).json({ error: 'Token required.' });
  }
  req.user = { id: 1 }; // Attach data to req for downstream use
  next();
};

// Apply to a single route
app.get('/protected', authMiddleware, (req, res) => {
  res.json({ user: req.user });
});

// Apply to a router
router.use(authMiddleware); // All routes in this router are protected
```

### Error Middleware

Has **four** parameters — Express identifies it as error middleware by the extra `err` parameter:

```javascript
// Must have exactly 4 params: (err, req, res, next)
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.statusCode || 500).json({
    success: false,
    error: { message: err.message || 'Internal Server Error' }
  });
});
```

Call it with `next(err)`:
```javascript
app.get('/risky', (req, res, next) => {
  try {
    throw new Error('Something broke');
  } catch (err) {
    next(err); // Passes error to error middleware
  }
});
```

### Middleware Execution Order

Order matters. Middleware runs in the order it is defined:

```javascript
app.use(cors());          // 1st — must be before routes
app.use(express.json());  // 2nd — must be before route handlers that need req.body
app.use(logger);          // 3rd

app.get('/users', handler); // 4th — route handler

app.use(notFound);        // 5th — catch-all 404
app.use(errorHandler);    // 6th (must be last) — error middleware
```

### Third-Party Middleware Examples

```javascript
const cors    = require('cors');
const helmet  = require('helmet');
const morgan  = require('morgan');
const rateLimit = require('express-rate-limit');

app.use(helmet());               // Security headers
app.use(cors({ origin: '*' }));  // CORS
app.use(morgan('dev'));           // HTTP request logging
app.use(rateLimit({ windowMs: 15 * 60 * 1000, max: 100 })); // Rate limit
```


# CHAPTER 11: Static Files

Express can serve static files (HTML, CSS, JavaScript, images) directly.

```javascript
// Serve files from the 'public' directory
app.use(express.static('public'));

// With a virtual prefix: http://localhost:3000/assets/logo.png
app.use('/assets', express.static('public'));
```

**Folder structure:**
```
project/
├── public/
│   ├── index.html
│   ├── style.css
│   ├── app.js
│   └── images/
│       └── logo.png
└── server.js
```

Express will serve `public/index.html` at `GET /index.html` (and `GET /` if it exists).


# CHAPTER 12: Template Engines

Template engines let the server render HTML with dynamic data. In REST API development, you typically return JSON — template engines are used for server-rendered apps (dashboards, email templates).

### EJS (Embedded JavaScript)

```bash
npm install ejs
```

```javascript
app.set('view engine', 'ejs');
app.set('views', './views');

app.get('/dashboard', (req, res) => {
  res.render('dashboard', {
    title: 'Dashboard',
    user: { name: 'Alice' }
  });
});
```

```html
<!-- views/dashboard.ejs -->
<h1><%= title %></h1>
<p>Welcome, <%= user.name %></p>
<% if (user.name === 'Alice') { %>
  <p>You are an admin.</p>
<% } %>
```

| Engine | Syntax | Notes |
|:-------|:-------|:------|
| **EJS** | `<%= value %>` | Close to HTML; easy for beginners |
| **Pug** | Indented; no closing tags | Concise but different syntax |
| **Handlebars** | `{{ value }}` | Logic-less; clean separation |

> **For REST APIs:** Skip template engines. Return JSON and let the frontend handle rendering.


# CHAPTER 13: Environment Variables

### Why Environment Variables?

Sensitive data (database passwords, JWT secrets, API keys) must **never** be hardcoded in source code — it would be visible in git history and to anyone who reads the code.

Environment variables are set outside the application and injected at runtime.

### dotenv

```bash
npm install dotenv
```

**.env file (never commit to git!):**
```
PORT=3000
NODE_ENV=development
JWT_SECRET=super_long_random_secret_string_here
DB_URL=postgresql://user:password@localhost:5432/mydb
API_KEY=sk_live_abc123
```

**Load in server.js (as early as possible):**
```javascript
require('dotenv').config(); // Must be called before anything else

const PORT = process.env.PORT || 3000;
const JWT_SECRET = process.env.JWT_SECRET;

if (!JWT_SECRET) {
  console.error('FATAL: JWT_SECRET is not defined');
  process.exit(1); // Exit if critical config is missing
}
```

**.gitignore:**
```
.env
.env.local
.env.production
```

**Provide a template (.env.example — DO commit this):**
```
PORT=3000
NODE_ENV=development
JWT_SECRET=replace_with_real_secret
DB_URL=postgresql://user:password@localhost:5432/mydb
```

### Config Module Pattern

```javascript
// src/config/env.js — centralize all env vars

require('dotenv').config();

module.exports = {
  port:      parseInt(process.env.PORT) || 3000,
  nodeEnv:   process.env.NODE_ENV || 'development',
  jwtSecret: process.env.JWT_SECRET,
  dbUrl:     process.env.DB_URL,
  isProduction: process.env.NODE_ENV === 'production'
};
```

```javascript
// Usage anywhere in the app
const config = require('./config/env');
console.log(config.port); // 3000
```


# CHAPTER 14: Error Handling

### Types of Errors

| Type | Description | Examples |
|:-----|:------------|:---------|
| **Operational** | Expected errors at runtime | 404, validation failure, wrong password |
| **Programmer** | Bugs in your code | TypeError, ReferenceError, off-by-one |

Operational errors should be communicated to the client. Programmer errors should be logged and a generic 500 returned.

### Custom Error Class

```javascript
// src/utils/AppError.js

class AppError extends Error {
  constructor(message, statusCode, code) {
    super(message);
    this.statusCode = statusCode;
    this.code = code;
    this.isOperational = true;
    Error.captureStackTrace(this, this.constructor);
  }
}

module.exports = AppError;
```

**Usage:**
```javascript
const AppError = require('../utils/AppError');

// Throw anywhere in a controller or service
if (!user) {
  throw new AppError('User not found.', 404, 'NOT_FOUND');
}
```

### Global Error Handler

```javascript
// src/middleware/error.middleware.js

const AppError = require('../utils/AppError');

module.exports = (err, req, res, next) => {
  // Log every error internally
  console.error(`[${new Date().toISOString()}] ERROR`, {
    message: err.message,
    stack: err.stack,
    path: req.path,
    method: req.method
  });

  // Operational: safe to expose to client
  if (err.isOperational) {
    return res.status(err.statusCode).json({
      success: false,
      error: { code: err.code, message: err.message }
    });
  }

  // Programmer error: generic message
  res.status(500).json({
    success: false,
    error: {
      code: 'INTERNAL_SERVER_ERROR',
      message: 'An unexpected error occurred.'
    }
  });
};
```

### Async Error Handling

Errors thrown in `async` functions must be caught and passed to `next()`:

```javascript
// Wrap async handlers to avoid try/catch everywhere
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Now any thrown error is automatically forwarded to error middleware
app.get('/users', asyncHandler(async (req, res) => {
  const users = await db.findAll(); // If this throws, error middleware catches it
  res.json({ success: true, data: users });
}));
```


# CHAPTER 15: Async Programming

### Callbacks (Legacy)

```javascript
const fs = require('fs');

fs.readFile('data.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error reading file:', err.message);
    return;
  }
  console.log(data);
});
```

**Callback hell** — what happens when callbacks are nested:
```javascript
fs.readFile('a.txt', 'utf8', (err, a) => {
  fs.readFile('b.txt', 'utf8', (err, b) => {
    fs.readFile('c.txt', 'utf8', (err, c) => {
      // Deeply nested, hard to read and debug
    });
  });
});
```

### Promises

A Promise is an object representing the eventual completion or failure of an async operation.

```javascript
const { promises: fsPromises } = require('fs');

fsPromises.readFile('data.txt', 'utf8')
  .then(data => {
    console.log(data);
    return fsPromises.readFile('data2.txt', 'utf8');
  })
  .then(data2 => console.log(data2))
  .catch(err => console.error('Error:', err.message))
  .finally(() => console.log('Done'));
```

### async/await

The cleanest syntax — makes async code read like synchronous code:

```javascript
const { promises: fsPromises } = require('fs');

async function readFiles() {
  try {
    const a = await fsPromises.readFile('a.txt', 'utf8');
    const b = await fsPromises.readFile('b.txt', 'utf8');
    const c = await fsPromises.readFile('c.txt', 'utf8');
    console.log(a, b, c);
  } catch (err) {
    console.error('Error:', err.message);
  }
}

readFiles();
```

### Promise.all — Parallel Execution

Run multiple async operations **simultaneously**:

```javascript
async function getMultipleUsers(ids) {
  // Sequential (slow — waits for each)
  // const user1 = await db.findUser(ids[0]);
  // const user2 = await db.findUser(ids[1]);

  // Parallel (fast — all requests fire at once)
  const [user1, user2, user3] = await Promise.all([
    db.findUser(ids[0]),
    db.findUser(ids[1]),
    db.findUser(ids[2])
  ]);

  return [user1, user2, user3];
}
```

> **Rule:** Use `Promise.all` when operations are independent. Use sequential `await` when each depends on the previous.

### Promise.race

Resolves/rejects with the result of whichever Promise settles first:

```javascript
const timeout = new Promise((_, reject) =>
  setTimeout(() => reject(new Error('Timeout!')), 5000)
);

const fetchData = fetch('https://api.example.com/data');

const result = await Promise.race([fetchData, timeout]);
// Throws if the fetch takes more than 5 seconds
```

### Promise.allSettled

Like `Promise.all` but never rejects — returns results for all Promises (fulfilled or rejected):

```javascript
const results = await Promise.allSettled([
  fetchUser(1),   // succeeds
  fetchUser(999), // fails (user not found)
  fetchUser(3)    // succeeds
]);

results.forEach(result => {
  if (result.status === 'fulfilled') console.log(result.value);
  else console.log('Failed:', result.reason.message);
});
```


# CHAPTER 16: File System Module

Node.js `fs` module provides APIs for interacting with the file system.

```javascript
const fs = require('fs');
const { promises: fsp } = require('fs'); // Promise-based API
```

### Reading Files

```javascript
// Async (callback) — non-blocking
fs.readFile('data.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log(data);
});

// Promise-based (preferred)
async function read() {
  const data = await fsp.readFile('data.txt', 'utf8');
  console.log(data);
}

// Sync (blocking — only use in startup scripts, never in request handlers)
const data = fs.readFileSync('config.json', 'utf8');
```

### Writing Files

```javascript
// Write (overwrites if file exists)
await fsp.writeFile('output.txt', 'Hello, World!', 'utf8');

// Append (adds to end of file)
await fsp.appendFile('log.txt', `${new Date().toISOString()} - Request received\n`);
```

### Other Operations

```javascript
// Check if file exists
try {
  await fsp.access('file.txt');
  console.log('File exists');
} catch {
  console.log('File does not exist');
}

// Rename / move
await fsp.rename('old-name.txt', 'new-name.txt');

// Delete file
await fsp.unlink('file.txt');

// Create directory
await fsp.mkdir('new-folder', { recursive: true });

// List directory contents
const files = await fsp.readdir('./src');
console.log(files); // ['app.js', 'routes', ...]

// Get file stats (size, dates, type)
const stats = await fsp.stat('data.txt');
console.log(stats.size);        // bytes
console.log(stats.isDirectory()); // false
console.log(stats.mtime);      // last modified date
```

### Practical Example: Simple Logger

```javascript
const { promises: fsp } = require('fs');
const path = require('path');

async function log(message) {
  const logLine = `[${new Date().toISOString()}] ${message}\n`;
  const logPath = path.join(__dirname, 'app.log');
  await fsp.appendFile(logPath, logLine);
}

// Usage
await log('Server started on port 3000');
await log('User 123 logged in');
```


# CHAPTER 17: Path Module

The `path` module provides utilities for working with file and directory paths in a cross-platform way (Windows uses `\`, Unix uses `/`).

```javascript
const path = require('path');
```

### Common Methods

```javascript
// join — combines path segments safely
path.join('/users', 'alice', 'documents', 'file.txt');
// '/users/alice/documents/file.txt'

path.join(__dirname, 'public', 'index.html');
// Absolute path to public/index.html relative to current file

// resolve — creates an absolute path
path.resolve('src', 'app.js');
// '/home/user/project/src/app.js'

// basename — last part of a path
path.basename('/users/alice/file.txt');     // 'file.txt'
path.basename('/users/alice/file.txt', '.txt'); // 'file' (strips extension)

// dirname — directory containing the file
path.dirname('/users/alice/file.txt');  // '/users/alice'

// extname — file extension
path.extname('index.html');  // '.html'
path.extname('style.css');   // '.css'
path.extname('server.js');   // '.js'

// parse — break path into its components
path.parse('/users/alice/file.txt');
// {
//   root: '/',
//   dir: '/users/alice',
//   base: 'file.txt',
//   ext: '.txt',
//   name: 'file'
// }
```

### __dirname and __filename

```javascript
console.log(__dirname);   // Absolute path to the current file's DIRECTORY
console.log(__filename);  // Absolute path to the current FILE

// Safe way to build paths:
const publicDir = path.join(__dirname, '..', 'public');
app.use(express.static(publicDir));
```

> **Note:** `__dirname` and `__filename` are not available in ES Modules. Use `import.meta.url` instead.


# CHAPTER 18: HTTP Module

The `http` module is what Express is built on top of. You can create a raw HTTP server without Express:

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  // req = IncomingMessage, res = ServerResponse

  // Manual routing
  if (req.method === 'GET' && req.url === '/') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ message: 'Hello World' }));
    return;
  }

  if (req.method === 'POST' && req.url === '/users') {
    let body = '';
    req.on('data', chunk => { body += chunk.toString(); });
    req.on('end', () => {
      const data = JSON.parse(body);
      res.writeHead(201, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify({ created: data }));
    });
    return;
  }

  // 404
  res.writeHead(404, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify({ error: 'Not Found' }));
});

server.listen(3000, () => console.log('Server on port 3000'));
```

This demonstrates why Express exists — doing this manually is tedious and error-prone. Express wraps all of this into a clean API.

### Making HTTP Requests (Built-in)

```javascript
const https = require('https');

https.get('https://jsonplaceholder.typicode.com/todos/1', (res) => {
  let data = '';
  res.on('data', chunk => data += chunk);
  res.on('end', () => console.log(JSON.parse(data)));
});
```

In practice, use `node-fetch`, `axios`, or Node 18+ built-in `fetch` for making requests.


# CHAPTER 19: Events

Node.js has an `EventEmitter` class for implementing the observer (pub/sub) pattern.

```javascript
const EventEmitter = require('events');

// Create an emitter
const emitter = new EventEmitter();

// Register event listener
emitter.on('userCreated', (user) => {
  console.log(`New user created: ${user.name}`);
  // Send welcome email, update analytics, etc.
});

// Emit the event
emitter.emit('userCreated', { name: 'Alice', email: 'alice@example.com' });
// Output: New user created: Alice
```

### Common EventEmitter Methods

```javascript
// Listen to an event (can fire multiple times)
emitter.on('data', (msg) => console.log(msg));

// Listen once (auto-removes after first trigger)
emitter.once('connected', () => console.log('Connected!'));

// Remove a specific listener
emitter.off('data', myHandler);

// Remove all listeners for an event
emitter.removeAllListeners('data');

// List current listeners
emitter.listeners('data'); // Array of functions

// Maximum listeners warning (default 10)
emitter.setMaxListeners(20);
```

### Real-World: Custom Logger with Events

```javascript
const EventEmitter = require('events');

class Logger extends EventEmitter {
  log(level, message) {
    const entry = { level, message, timestamp: new Date().toISOString() };
    this.emit('log', entry);
  }
}

const logger = new Logger();

logger.on('log', (entry) => {
  console.log(`[${entry.timestamp}] [${entry.level.toUpperCase()}] ${entry.message}`);
  // Could also write to file, send to logging service, etc.
});

logger.log('info', 'Server started');
logger.log('error', 'Database connection failed');
```


# CHAPTER 20: Streams

Streams allow processing data **piece by piece** instead of loading everything into memory at once. Essential for large files, video, and network data.

### Why Streams?

```javascript
// Without streams — loads entire 2GB file into RAM
const data = await fsp.readFile('huge-2gb-file.csv');

// With streams — processes chunks of ~64KB at a time
const stream = fs.createReadStream('huge-2gb-file.csv');
```

### Stream Types

| Type | Description | Example |
|:-----|:------------|:--------|
| **Readable** | Data flows out of it | `fs.createReadStream`, `http.IncomingMessage` |
| **Writable** | Data flows into it | `fs.createWriteStream`, `http.ServerResponse` |
| **Duplex** | Both readable and writable | TCP socket |
| **Transform** | Duplex that transforms data | `zlib.createGzip()` |

### Reading a File with Streams

```javascript
const fs = require('fs');

const readable = fs.createReadStream('large-file.txt', {
  encoding: 'utf8',
  highWaterMark: 64 * 1024 // 64KB chunks
});

readable.on('data', (chunk) => {
  console.log(`Received chunk: ${chunk.length} bytes`);
});

readable.on('end', () => {
  console.log('Finished reading');
});

readable.on('error', (err) => {
  console.error('Read error:', err);
});
```

### Piping Streams

`pipe()` connects a readable stream to a writable stream:

```javascript
const fs = require('fs');
const zlib = require('zlib');

// Compress a file (streaming — memory efficient)
fs.createReadStream('input.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('input.txt.gz'))
  .on('finish', () => console.log('File compressed!'));
```

### Streams in Express

HTTP responses in Express are writable streams:

```javascript
app.get('/download', (req, res) => {
  res.setHeader('Content-Type', 'text/plain');
  res.setHeader('Content-Disposition', 'attachment; filename="data.txt"');

  // Pipe file directly to response (memory efficient)
  fs.createReadStream('large-data.txt').pipe(res);
});
```


# CHAPTER 21: Buffers

A **Buffer** is a fixed-size chunk of memory used to store raw binary data — bytes. Used when working with binary data like files, images, network packets, or encrypted data.

```javascript
// Create a Buffer from a string
const buf1 = Buffer.from('Hello', 'utf8');
console.log(buf1);        // <Buffer 48 65 6c 6c 6f>
console.log(buf1.length); // 5

// Create an empty Buffer of N bytes
const buf2 = Buffer.alloc(10); // 10 zero bytes
console.log(buf2); // <Buffer 00 00 00 00 00 00 00 00 00 00>

// Convert Buffer back to string
console.log(buf1.toString('utf8'));   // 'Hello'
console.log(buf1.toString('base64')); // 'SGVsbG8='
console.log(buf1.toString('hex'));    // '48656c6c6f'

// Concatenate buffers
const a = Buffer.from('Hello, ');
const b = Buffer.from('World!');
const combined = Buffer.concat([a, b]);
console.log(combined.toString()); // 'Hello, World!'
```

### Why Buffers Matter

```javascript
// When you receive binary data over HTTP (image upload, file upload)
app.post('/upload', (req, res) => {
  const chunks = [];
  req.on('data', chunk => chunks.push(chunk));
  req.on('end', () => {
    const buffer = Buffer.concat(chunks);
    // buffer contains the raw binary data of the uploaded file
    fs.writeFileSync('uploaded-file.png', buffer);
    res.json({ success: true });
  });
});
```


# CHAPTER 22: Authentication Basics

### Session-Based Authentication

```
1. User POSTs credentials → server verifies → creates session in DB/Redis
2. Server sends Set-Cookie: sessionId=abc123 header
3. Browser automatically sends cookie on every subsequent request
4. Server looks up session in DB to identify user

Pros: Easy to revoke (delete session), secure with HttpOnly cookies
Cons: Server must store session state, hard to scale horizontally
```

### JWT Authentication

```
1. User POSTs credentials → server verifies → creates signed JWT
2. Server returns { token: "eyJhbGci..." } in response body
3. Client stores token (memory or localStorage)
4. Client sends Authorization: Bearer <token> on every request
5. Server verifies signature — no DB lookup needed

Pros: Stateless, scalable, works across microservices
Cons: Cannot revoke before expiry without a denylist
```

### Cookies vs JWT — Comparison

| Feature | Session + Cookie | JWT |
|:--------|:----------------|:----|
| **Storage** | Server-side (DB/Redis) | Client-side (token) |
| **Stateless** | ❌ No | ✅ Yes |
| **Revocation** | ✅ Easy (delete session) | ❌ Hard (needs denylist) |
| **Scale** | Requires shared store | ✅ Scales easily |
| **Mobile** | ❌ Cookies are browser-specific | ✅ Works everywhere |
| **XSS risk** | Low (HttpOnly cookie) | High if stored in localStorage |
| **CSRF risk** | High (auto-sent by browser) | Low (must be set manually) |

### Basic JWT Implementation

```javascript
const jwt = require('jsonwebtoken');

const JWT_SECRET = process.env.JWT_SECRET;

// Generate a token (on login)
function generateToken(userId) {
  return jwt.sign(
    { userId, iat: Math.floor(Date.now() / 1000) },
    JWT_SECRET,
    { expiresIn: '7d' }
  );
}

// Verify a token (middleware)
function authMiddleware(req, res, next) {
  const authHeader = req.headers.authorization;
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return res.status(401).json({ error: 'Token required.' });
  }

  const token = authHeader.split(' ')[1];

  try {
    const decoded = jwt.verify(token, JWT_SECRET);
    req.user = decoded; // { userId: 1, iat: ..., exp: ... }
    next();
  } catch (err) {
    return res.status(401).json({ error: 'Invalid or expired token.' });
  }
}
```


# CHAPTER 23: REST API Best Practices

*(Cross-reference with [Module 06 — API Design](../06-api-design/notes.md) for full coverage)*

### Quick Reference

| Category | Rule |
|:---------|:-----|
| **Naming** | Plural nouns, lowercase, hyphens, no verbs |
| **Methods** | GET=read, POST=create, PUT=replace, PATCH=update, DELETE=delete |
| **Status Codes** | 200, 201, 204, 400, 401, 403, 404, 409, 422, 429, 500 |
| **Versioning** | `/api/v1/` prefix from day one |
| **Pagination** | Always paginate collections (`page`, `limit`) |
| **Validation** | Validate every input, return 422 with field-level errors |
| **Errors** | Consistent envelope `{ success, error: { code, message } }` |
| **Security** | HTTPS, Helmet, CORS whitelist, rate limiting, env vars |
| **Filtering** | Query params for filter/sort/search |
| **Auth** | JWT in `Authorization: Bearer <token>` header |


# CHAPTER 24: Express Project — Student Management API

A complete, production-structured CRUD API.

### Project Setup

```bash
mkdir student-api && cd student-api
npm init -y
npm install express dotenv
npm install --save-dev nodemon
```

**package.json scripts:**
```json
{
  "scripts": {
    "start": "node server.js",
    "dev":   "nodemon server.js"
  }
}
```

### Folder Structure

```
student-api/
├── src/
│   ├── routes/
│   │   └── students.routes.js
│   ├── controllers/
│   │   └── students.controller.js
│   ├── services/
│   │   └── students.service.js
│   ├── middleware/
│   │   ├── validate.middleware.js
│   │   └── error.middleware.js
│   ├── utils/
│   │   └── AppError.js
│   └── app.js
├── server.js
├── .env
└── package.json
```

### .env

```
PORT=3000
NODE_ENV=development
```

### server.js

```javascript
require('dotenv').config();
const app = require('./src/app');

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Student API running on port ${PORT}`));
```

### src/utils/AppError.js

```javascript
class AppError extends Error {
  constructor(message, statusCode, code) {
    super(message);
    this.statusCode = statusCode;
    this.code = code;
    this.isOperational = true;
  }
}

module.exports = AppError;
```

### src/services/students.service.js

```javascript
// In-memory store (replace with database in production)
const AppError = require('../utils/AppError');

let students = [
  { id: 1, name: 'Alice Kumar',   email: 'alice@example.com',   grade: 'A', age: 20 },
  { id: 2, name: 'Bob Sharma',    email: 'bob@example.com',     grade: 'B', age: 21 },
  { id: 3, name: 'Charlie Singh', email: 'charlie@example.com', grade: 'A', age: 19 },
];
let nextId = 4;

exports.getAll = ({ search, grade, page = 1, limit = 10 }) => {
  let result = [...students];
  if (grade)  result = result.filter(s => s.grade === grade.toUpperCase());
  if (search) result = result.filter(s =>
    s.name.toLowerCase().includes(search.toLowerCase())
  );

  const pageNum = parseInt(page), limitNum = parseInt(limit);
  const start = (pageNum - 1) * limitNum;
  return {
    data:       result.slice(start, start + limitNum),
    total:      result.length,
    page:       pageNum,
    limit:      limitNum,
    totalPages: Math.ceil(result.length / limitNum)
  };
};

exports.getById = (id) => {
  const student = students.find(s => s.id === parseInt(id));
  if (!student) throw new AppError(`Student ${id} not found.`, 404, 'NOT_FOUND');
  return student;
};

exports.create = (data) => {
  if (students.some(s => s.email === data.email)) {
    throw new AppError('Email already registered.', 409, 'DUPLICATE_EMAIL');
  }
  const student = { id: nextId++, ...data };
  students.push(student);
  return student;
};

exports.update = (id, updates) => {
  const index = students.findIndex(s => s.id === parseInt(id));
  if (index === -1) throw new AppError(`Student ${id} not found.`, 404, 'NOT_FOUND');
  students[index] = { ...students[index], ...updates };
  return students[index];
};

exports.remove = (id) => {
  const index = students.findIndex(s => s.id === parseInt(id));
  if (index === -1) throw new AppError(`Student ${id} not found.`, 404, 'NOT_FOUND');
  students.splice(index, 1);
};
```

### src/middleware/validate.middleware.js

```javascript
const AppError = require('../utils/AppError');

exports.validateStudent = (req, res, next) => {
  const { name, email, grade, age } = req.body;
  const errors = [];

  if (!name || typeof name !== 'string' || name.trim().length < 2) {
    errors.push({ field: 'name', message: 'Name must be at least 2 characters.' });
  }
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!email || !emailRegex.test(email)) {
    errors.push({ field: 'email', message: 'Valid email is required.' });
  }
  const validGrades = ['A', 'B', 'C', 'D', 'F'];
  if (!grade || !validGrades.includes(grade.toUpperCase())) {
    errors.push({ field: 'grade', message: `Grade must be one of: ${validGrades.join(', ')}.` });
  }
  if (age !== undefined && (typeof age !== 'number' || age < 16 || age > 60)) {
    errors.push({ field: 'age', message: 'Age must be between 16 and 60.' });
  }

  if (errors.length) {
    return res.status(422).json({
      success: false,
      error: { code: 'VALIDATION_ERROR', message: 'Validation failed.', details: errors }
    });
  }
  next();
};
```

### src/controllers/students.controller.js

```javascript
const studentsService = require('../services/students.service');

const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

exports.getAll = asyncHandler(async (req, res) => {
  const result = studentsService.getAll(req.query);
  res.status(200).json({ success: true, ...result });
});

exports.getOne = asyncHandler(async (req, res) => {
  const student = studentsService.getById(req.params.id);
  res.status(200).json({ success: true, data: student });
});

exports.create = asyncHandler(async (req, res) => {
  const student = studentsService.create(req.body);
  res.status(201)
     .set('Location', `/api/v1/students/${student.id}`)
     .json({ success: true, data: student, message: 'Student created.' });
});

exports.update = asyncHandler(async (req, res) => {
  const student = studentsService.update(req.params.id, req.body);
  res.status(200).json({ success: true, data: student });
});

exports.remove = asyncHandler(async (req, res) => {
  studentsService.remove(req.params.id);
  res.status(204).send();
});
```

### src/routes/students.routes.js

```javascript
const express = require('express');
const router = express.Router();
const ctrl = require('../controllers/students.controller');
const { validateStudent } = require('../middleware/validate.middleware');

router.get('/',     ctrl.getAll);
router.get('/:id',  ctrl.getOne);
router.post('/',    validateStudent, ctrl.create);
router.patch('/:id',ctrl.update);
router.delete('/:id',ctrl.remove);

module.exports = router;
```

### src/middleware/error.middleware.js

```javascript
module.exports = (err, req, res, next) => {
  console.error(`[ERROR] ${req.method} ${req.path}:`, err.message);

  if (err.isOperational) {
    return res.status(err.statusCode).json({
      success: false,
      error: { code: err.code, message: err.message }
    });
  }

  res.status(500).json({
    success: false,
    error: { code: 'INTERNAL_SERVER_ERROR', message: 'An unexpected error occurred.' }
  });
};
```

### src/app.js

```javascript
const express = require('express');
const studentsRouter = require('./routes/students.routes');
const errorMiddleware = require('./middleware/error.middleware');

const app = express();

// Middleware
app.use(express.json());
app.use((req, res, next) => {
  console.log(`${new Date().toISOString()} ${req.method} ${req.path}`);
  next();
});

// Routes
app.use('/api/v1/students', studentsRouter);

// 404
app.use((req, res) => {
  res.status(404).json({
    success: false,
    error: { code: 'ROUTE_NOT_FOUND', message: `${req.method} ${req.path} not found.` }
  });
});

// Error handler (must be last)
app.use(errorMiddleware);

module.exports = app;
```

### Testing with cURL

```bash
# Start the server
npm run dev

# Get all students
curl http://localhost:3000/api/v1/students

# Filter by grade
curl "http://localhost:3000/api/v1/students?grade=A"

# Search by name
curl "http://localhost:3000/api/v1/students?search=alice"

# Get one student
curl http://localhost:3000/api/v1/students/1

# Create a student
curl -X POST http://localhost:3000/api/v1/students \
  -H "Content-Type: application/json" \
  -d '{"name":"Diana Patel","email":"diana@example.com","grade":"A","age":20}'

# Update (partial)
curl -X PATCH http://localhost:3000/api/v1/students/1 \
  -H "Content-Type: application/json" \
  -d '{"grade":"B"}'

# Delete
curl -X DELETE http://localhost:3000/api/v1/students/3
```


# CHAPTER 25: Interview Questions

### Node.js — Beginner

**Q1. What is Node.js?**
Node.js is a JavaScript runtime built on Chrome's V8 engine that lets you run JavaScript on the server. It uses an event-driven, non-blocking I/O model.

**Q2. What is the difference between Node.js and a browser?**
Both run JavaScript via V8, but: browsers have the DOM (`document`, `window`); Node has file system access, OS APIs, and the `http` module. The global object is `window` in browsers, `global` in Node.

**Q3. What is npm?**
Node Package Manager — ships with Node.js. Used to install packages, manage dependencies, and run project scripts.

**Q4. What is package.json?**
The manifest file for a Node.js project. Records name, version, dependencies, devDependencies, and scripts.

**Q5. What is the difference between `dependencies` and `devDependencies`?**
`dependencies` are required in production (Express, dotenv). `devDependencies` are only needed during development (nodemon, jest). `npm install --production` skips devDependencies.

**Q6. What is `require()` in Node.js?**
The CommonJS function to import modules. `require('./module')` loads a local file; `require('express')` loads an npm package.

**Q7. What is `module.exports`?**
The object that a module exposes when `require()`d. Anything assigned to `module.exports` becomes available to the importing file.

**Q8. What is the difference between `require` and `import`?**
`require` is CommonJS (synchronous, available in all `.js` files). `import` is ES Module syntax (asynchronous, requires `.mjs` or `"type":"module"` in package.json).

**Q9. What is `nodemon`?**
A development tool that watches for file changes and automatically restarts the Node.js server. Install as a devDependency and run with `npx nodemon server.js`.

**Q10. How do you read environment variables?**
Via `process.env.VARIABLE_NAME`. Use the `dotenv` package to load them from a `.env` file.

### Node.js — Intermediate

**Q11. What is the Event Loop?**
The mechanism that enables Node.js's non-blocking behavior. It continuously checks the call stack and callback/microtask queues, moving callbacks to the stack when it's empty.

**Q12. What is the order of execution: `setTimeout`, `Promise.then`, `process.nextTick`?**
`process.nextTick` → `Promise.then` (microtasks) → `setTimeout` (timers phase).

**Q13. What is `process.nextTick()`?**
A way to schedule a callback to run at the end of the current operation, before the event loop continues. It has higher priority than Promises.

**Q14. What is `setImmediate()`?**
Schedules a callback to run in the check phase of the event loop — after I/O events but before timers. Use it instead of `setTimeout(fn, 0)` when you need to run after I/O.

**Q15. What is libuv?**
A C library used by Node.js to handle asynchronous I/O. It provides the event loop, a thread pool (for expensive operations like file system), and cross-platform OS abstractions.

**Q16. What is the thread pool in Node.js?**
libuv's internal pool of 4 threads (configurable with `UV_THREADPOOL_SIZE`) used for operations that can't be done non-blockingly: file system, crypto, DNS, zlib.

**Q17. What is the difference between `fs.readFile` and `fs.readFileSync`?**
`readFile` is asynchronous (callback-based, non-blocking). `readFileSync` is synchronous (blocking). Never use sync methods in request handlers — they block the event loop.

**Q18. What are Streams in Node.js?**
Streams process data piece by piece rather than loading it all into memory. Types: Readable, Writable, Duplex, Transform. Use `pipe()` to connect them.

**Q19. What is an EventEmitter?**
A class in Node.js (`events` module) that implements the observer pattern. Objects can `emit()` named events and other objects can listen with `on()`.

**Q20. What is a Buffer?**
A fixed-size chunk of memory for storing raw binary data. Used when working with binary files, encryption, or network data where encoding/decoding is needed.

**Q21. What is CORS and how do you handle it in Express?**
Cross-Origin Resource Sharing — a browser security policy restricting cross-domain requests. Handle with the `cors` npm package: `app.use(cors({ origin: 'https://yourfrontend.com' }))`.

**Q22. What is middleware in Express?**
A function with `(req, res, next)` that runs between request and response. Used for logging, authentication, CORS, body parsing, error handling.

**Q23. What is the difference between `app.use()` and `app.get()`?**
`app.use()` matches all HTTP methods and can match partial paths. `app.get()` matches only GET requests on exact path. `app.use()` is for middleware; `app.get/post/etc` are for routes.

**Q24. How do you handle errors in async Express routes?**
Wrap the handler in an `asyncHandler` wrapper that calls `.catch(next)`. Or use a try/catch block and call `next(err)`. The global error handler (4-param middleware) then processes the error.

**Q25. What is `express.json()`?**
Built-in middleware that parses incoming JSON request bodies. Without it, `req.body` is `undefined`.

### Node.js — Advanced

**Q26. What is semantic versioning?**
`MAJOR.MINOR.PATCH`. PATCH = bug fix, MINOR = new backward-compatible feature, MAJOR = breaking change. `^` in package.json allows MINOR+PATCH updates; `~` allows only PATCH.

**Q27. What is the purpose of `package-lock.json`?**
Records the exact installed version of every package and its dependencies. Ensures consistent installs across machines and CI environments.

**Q28. How would you prevent a Node.js server from crashing on unhandled errors?**
Use `process.on('uncaughtException', handler)` and `process.on('unhandledRejection', handler)`. However, the best practice is to handle errors in every async function and use a global error middleware.

**Q29. What is the Cluster module in Node.js?**
Allows creating multiple child processes (workers) that share the same server port. Enables Node.js to utilize multiple CPU cores. PM2 is the production tool for this.

**Q30. What is the difference between `==` and `===` in JavaScript (Node.js context)?**
`==` performs type coercion (`'5' == 5` → true). `===` checks value AND type (`'5' === 5` → false). Always use `===` in server code to avoid subtle bugs.

### Express.js Questions

**Q31. How do you structure a large Express application?**
Use the MVC-like pattern: `routes/` (URL mapping), `controllers/` (HTTP handling), `services/` (business logic), `models/` (database), `middleware/` (reusable logic).

**Q32. What is `express.Router()`?**
A mini Express app for defining modular routes. Mounted on the main app with `app.use('/prefix', router)`.

**Q33. How do you serve static files in Express?**
`app.use(express.static('public'))` — serves files from the `public` directory.

**Q34. What is the difference between `res.send()` and `res.json()`?**
`res.json()` serializes an object to JSON and sets `Content-Type: application/json`. `res.send()` infers the type — sends string as text/html, Buffer as binary.

**Q35. How do you implement rate limiting in Express?**
Use the `express-rate-limit` package: `app.use(rateLimit({ windowMs: 15 * 60 * 1000, max: 100 }))`.

**Q36. What does `next()` do in middleware?**
Passes control to the next middleware or route handler in the stack. If not called and no response is sent, the request hangs.

**Q37. What does `next(err)` do?**
Skips all remaining non-error middleware and jumps directly to the error-handling middleware (the one with 4 parameters).

**Q38. How do you protect routes with authentication middleware?**
Define an auth middleware that verifies the JWT and sets `req.user`. Apply it per route: `app.get('/protected', authMiddleware, handler)` or per router: `router.use(authMiddleware)`.

**Q39. What is Helmet.js?**
An Express middleware that sets various security-related HTTP headers: `X-Frame-Options`, `X-Content-Type-Options`, `Strict-Transport-Security`, etc. Use `app.use(helmet())`.

**Q40. How do you implement pagination in Express?**
Read `page` and `limit` from query params, compute `offset = (page-1) * limit`, slice/query accordingly, return `meta: { total, page, limit, totalPages }` in the response.

**Q41. What is `dotenv` and why is it used?**
A package that loads environment variables from a `.env` file into `process.env`. Keeps secrets out of source code.

**Q42. How do you implement logging in Express?**
For HTTP request logs: use `morgan` middleware. For application logs: use `winston` or `pino`. Log method, path, status code, and response time for every request.

**Q43. What is the asyncHandler wrapper pattern?**
```javascript
const asyncHandler = fn => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);
```
Wraps an async route handler so thrown errors automatically go to `next(err)`.

**Q44. What is the Repository pattern?**
An abstraction layer between business logic (services) and the database. Services call repository methods (`userRepo.findById()`); the repository handles the actual query. Makes it easy to swap databases without changing business logic.

**Q45. How do you gracefully shut down an Express server?**
```javascript
const server = app.listen(PORT);
process.on('SIGTERM', () => {
  server.close(() => {
    db.disconnect();
    process.exit(0);
  });
});
```

### Scenario Questions

**Q46. A user reports an API is slow. How do you diagnose it?**
1. Check request logs for high latency endpoints. 2. Use APM (Datadog/New Relic) for traces. 3. Profile database queries (add indexes, fix N+1). 4. Check for missing `await` causing unhandled Promises. 5. Monitor CPU/memory for resource exhaustion.

**Q47. How would you prevent SQL injection in Node.js?**
Always use parameterized queries: `db.query('SELECT * FROM users WHERE id = $1', [id])`. Never concatenate user input into SQL strings. Use an ORM (Prisma, Sequelize) which handles this automatically.

**Q48. Your Node.js server crashes with "FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed". What is it?**
Out of memory — JavaScript heap exhausted. Common causes: memory leak (event listeners not removed), loading huge files without streams, infinite recursion. Fix: use streams, find and patch the leak, increase heap with `--max-old-space-size`.

**Q49. What is the difference between 401 and 403? Give an Express example.**
401 = not authenticated (no/invalid token). 403 = authenticated but lacks permission (wrong role). In middleware: check for token presence → 401; check for role → 403.

**Q50. How do you run multiple async database queries in parallel?**
`const [users, products] = await Promise.all([db.getUsers(), db.getProducts()])` — both queries execute simultaneously, dramatically reducing response time compared to sequential awaits.


# CHAPTER 26: Best Practices

### Code Organization
1. Separate `app.js` (Express setup) from `server.js` (listening) — makes testing easier.
2. Use `express.Router()` — keep routes out of `app.js`.
3. Keep controllers thin — delegate business logic to services.
4. Use a `config/` module for all environment variables — never scatter `process.env` access.
5. Name files consistently: `users.controller.js`, `users.service.js`, `users.routes.js`.

### Error Handling
6. Create a custom `AppError` class — distinguish operational from programmer errors.
7. Use the `asyncHandler` wrapper — avoid repetitive try/catch blocks.
8. Always have a global error middleware as the last `app.use()`.
9. Never expose stack traces in production responses.
10. Log every error with timestamp, method, path, and stack.

### Security
11. Use `helmet()` — automatic security headers.
12. Use CORS with an explicit origin whitelist.
13. Rate-limit all public endpoints.
14. Store all secrets in `.env` — never hardcode.
15. Add `.env` to `.gitignore` immediately.
16. Use parameterized queries — never interpolate user input into SQL.
17. Validate every incoming field — never trust client data.
18. Hash passwords with bcrypt (work factor ≥ 12) — never store plaintext.

### Performance
19. Never use `*Sync` methods in request handlers — they block the event loop.
20. Use `Promise.all()` for independent async operations.
21. Enable gzip compression: `app.use(require('compression')())`.
22. Use streams for large file responses — don't load everything into memory.
23. Index database columns used in WHERE/ORDER BY clauses.
24. Cache frequently read, rarely written data in Redis.

### Reliability
25. Set `NODE_ENV=production` in deployment — Express enables optimizations.
26. Use `process.on('unhandledRejection')` as a last-resort safety net.
27. Use `nodemon` for development, PM2 for production process management.
28. Expose `GET /health` that checks DB connectivity and returns `{ status: 'ok' }`.
29. Use semantic versioning for your API — bump MAJOR for breaking changes.
30. Write integration tests for every endpoint — at minimum happy path + 4xx cases.

# CHAPTER 27: Common Mistakes

| # | Mistake | Problem | Fix |
|:--|:--------|:--------|:----|
| 1 | `fs.readFileSync` in a route handler | Blocks the event loop for all concurrent requests | Use `fs.readFile` or `fsp.readFile` |
| 2 | Missing `return` before `res.json()` | "Cannot set headers after they are sent" error | `return res.json(...)` |
| 3 | Not calling `next()` in middleware | Request hangs indefinitely | Always call `next()` or send a response |
| 4 | Error middleware with 3 params | Express won't recognize it as error middleware | Must have exactly 4 params: `(err, req, res, next)` |
| 5 | Placing error middleware before routes | Errors from routes won't reach it | Put error middleware last |
| 6 | `req.body` is undefined | Missing `express.json()` | Add `app.use(express.json())` before routes |
| 7 | Committing `.env` to git | Credentials exposed in git history | Add `.env` to `.gitignore` immediately |
| 8 | Committing `node_modules` | Huge repo, CI breaks | Add `node_modules/` to `.gitignore` |
| 9 | Using `parseInt` without validation | `NaN` propagates silently | Check `isNaN(id)` after parsing |
| 10 | Forgetting `async` before a function using `await` | `SyntaxError` or unexpected behavior | Always pair `async` with `await` |
| 11 | `require()` inside a loop | Re-requires on every iteration (cached, but bad practice) | Move `require` to top of file |
| 12 | Not handling Promise rejections | `UnhandledPromiseRejectionWarning`, server may crash | Always `.catch()` or use `asyncHandler` |
| 13 | Returning 200 for errors | Client can't distinguish success from failure | Use correct 4xx/5xx status codes |
| 14 | Hardcoding port `3000` | Won't work in deployment environments | `process.env.PORT || 3000` |
| 15 | No input validation | Data corruption, injection attacks | Validate every field in every POST/PUT/PATCH |
| 16 | Deep callback nesting | Unreadable, hard to debug | Use `async/await` |
| 17 | `console.log` for production logging | No timestamps, levels, or structure | Use `winston` or `pino` |
| 18 | Storing JWT in localStorage | XSS can steal the token | Use `HttpOnly` cookie or in-memory storage |
| 19 | Not setting `Content-Type` header | Client may misparse the response | `res.json()` sets it automatically |
| 20 | No graceful shutdown | In-flight requests dropped on deploy | Handle `SIGTERM` signal |

---

# CHAPTER 28: Cheat Sheet

### Node.js Core Commands

```bash
node -v                    # Node version
npm -v                     # npm version
node script.js             # Run a script
node -e "console.log(1+1)" # Execute inline code
node --inspect server.js   # Debug mode
```

### npm Commands

```bash
npm init -y                  # Create package.json
npm install express          # Install package (production)
npm install nodemon -D       # Install devDependency
npm install                  # Install all from package.json
npm uninstall lodash         # Remove package
npm update                   # Update packages
npm outdated                 # Check for updates
npm audit                    # Security check
npm audit fix                # Auto-fix vulnerabilities
npm run dev                  # Run "dev" script
npm list --depth=0           # List top-level packages
npx nodemon server.js        # Run without global install
```

### Express Quick Reference

```javascript
const express = require('express');
const app = express();

// Middleware
app.use(express.json());
app.use(express.static('public'));
app.use(cors());
app.use(helmet());

// Routes
app.get('/path',    (req, res) => res.status(200).json({}));
app.post('/path',   (req, res) => res.status(201).json({}));
app.put('/path/:id',(req, res) => res.status(200).json({}));
app.patch('/path/:id',(req,res)=> res.status(200).json({}));
app.delete('/path/:id',(req,res)=>res.status(204).send());

// Request properties
req.body          // JSON body (requires express.json())
req.params.id     // :id route parameter
req.query.page    // ?page=1 query param
req.headers       // All headers
req.method        // 'GET', 'POST', etc.
req.path          // '/api/v1/users'
req.ip            // Client IP

// Response methods
res.status(200).json({ success: true })
res.status(204).send()
res.redirect(301, '/new-url')
res.set('Header', 'value')

// Error middleware (must be last, must have 4 params)
app.use((err, req, res, next) => {
  res.status(err.statusCode || 500).json({ error: err.message });
});

app.listen(3000, () => console.log('Running on 3000'));
```

### Async Patterns

```javascript
// Callback
fs.readFile('f.txt', 'utf8', (err, data) => { ... });

// Promise
fsp.readFile('f.txt', 'utf8').then(data => ...).catch(err => ...);

// async/await (preferred)
const data = await fsp.readFile('f.txt', 'utf8');

// Parallel
const [a, b] = await Promise.all([fetch(url1), fetch(url2)]);
```

### File System Quick Reference

```javascript
const { promises: fsp } = require('fs');

await fsp.readFile('file.txt', 'utf8');
await fsp.writeFile('file.txt', 'content');
await fsp.appendFile('file.txt', 'more');
await fsp.rename('old.txt', 'new.txt');
await fsp.unlink('file.txt');
await fsp.mkdir('dir', { recursive: true });
await fsp.readdir('./src');
await fsp.stat('file.txt');
```

### Project Folder Structure

```
project/
├── src/
│   ├── routes/          # URL → controller mapping
│   ├── controllers/     # HTTP layer
│   ├── services/        # Business logic
│   ├── models/          # Database schemas
│   ├── middleware/      # Auth, validate, error
│   ├── config/          # Env vars, DB config
│   ├── utils/           # AppError, asyncHandler
│   └── app.js           # Express setup
├── server.js            # app.listen()
├── .env                 # Secrets (DO NOT COMMIT)
├── .env.example         # Template (commit this)
├── .gitignore           # node_modules, .env
└── package.json
```

### Status Code Quick Reference

```
200 OK              → GET/PUT/PATCH success
201 Created         → POST created resource
204 No Content      → DELETE success
400 Bad Request     → Malformed/missing data
401 Unauthorized    → Not authenticated
403 Forbidden       → No permission
404 Not Found       → Resource missing
409 Conflict        → Duplicate resource
422 Unprocessable   → Invalid data (validation)
429 Too Many Req    → Rate limit exceeded
500 Server Error    → Unhandled exception
503 Unavailable     → Server down/busy
```


## Summary

| Chapter | Key Takeaways |
|:--------|:-------------|
| **1 – Node.js Intro** | JavaScript runtime on V8; non-blocking, event-driven, single-threaded |
| **2 – Installation** | Use nvm; `node -v`, `npm -v` to verify |
| **3 – Internals** | Event Loop, Call Stack, libuv Thread Pool, Microtask vs Macrotask queue |
| **4 – Modules** | CommonJS (`require`/`module.exports`) vs ESM (`import`/`export`) |
| **5 – npm** | `package.json`, `package-lock.json`, semver, `dependencies` vs `devDependencies` |
| **6 – Express Intro** | Minimal framework wrapping Node's `http`; defines routes and middleware |
| **7 – Structure** | routes → controllers → services → models; `app.js` vs `server.js` |
| **8 – Routing** | `app.get/post/put/patch/delete`; Router(); dynamic `:id` params |
| **9 – req/res** | `req.body`, `req.params`, `req.query`, `req.headers`; `res.json()`, `res.status()` |
| **10 – Middleware** | (req, res, next); order matters; error middleware has 4 params |
| **11 – Static Files** | `express.static('public')` serves HTML/CSS/JS/images |
| **12 – Templates** | EJS/Pug/Handlebars for server-rendered HTML; REST APIs use JSON |
| **13 – Env Vars** | `dotenv`; `.env` file; access via `process.env`; never commit secrets |
| **14 – Error Handling** | `AppError` class; global error middleware; `asyncHandler` wrapper |
| **15 – Async** | callbacks → Promises → async/await; `Promise.all` for parallelism |
| **16 – File System** | `fsp.readFile/writeFile/mkdir`; never use sync methods in handlers |
| **17 – Path** | `path.join(__dirname, ...)`, `basename`, `dirname`, `extname` |
| **18 – HTTP Module** | Node's raw http server; what Express wraps |
| **19 – Events** | `EventEmitter`; `on()`, `emit()`; observer pattern |
| **20 – Streams** | Readable/Writable/Transform; `pipe()`; memory-efficient large data |
| **21 – Buffers** | Raw binary data; `Buffer.from()`, `.toString()`; used in file/network I/O |
| **22 – Auth** | Sessions vs JWT; cookies vs Bearer tokens; `jsonwebtoken` package |
| **23 – Best Practices** | Naming, status codes, pagination, validation, security quick reference |
| **24 – Project** | Full Student Management API: routes, controllers, services, middleware |
| **25 – Interviews** | 50 Q&As: Node internals, Express, async, security, scenarios |
| **26 – Best Practices** | 30 rules: structure, security, performance, reliability |
| **27 – Mistakes** | 20 common errors: sync in handlers, missing return, no validation |
| **28 – Cheat Sheet** | npm commands, Express syntax, status codes, folder structure |


## References

- [Node.js Official Documentation](https://nodejs.org/en/docs/)
- [Express.js Official Documentation](https://expressjs.com/)
- [npm Documentation](https://docs.npmjs.com/)
- [libuv Documentation](https://libuv.org/)
- [Node.js Event Loop Explained — nodejs.org](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick)
- [JWT.io](https://jwt.io/)
- [Helmet.js](https://helmetjs.github.io/)
- [MDN — HTTP Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)


> **Next Module →** [Databases & SQL](../08-databases/notes.md)
> **Previous Module ←** [API Design](../06-api-design/notes.md)
