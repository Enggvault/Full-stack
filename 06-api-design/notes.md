title: "API Design & HTTP Status Codes: Complete Beginner to Advanced"
subtitle: "From First Principles to Production-Grade Architecture"
author: "Principal Backend Engineer — 15+ Years Industry Experience"
version: "2.0"
date: "2025"

# API Design & HTTP Status Codes
## Complete Beginner to Advanced Engineering Handbook

> A production-grade, book-quality reference covering REST API design principles, HTTP status codes, validation, authentication, and production implementation. Written for engineers at all levels — from beginners to FAANG system designers.

> **Prerequisites:** [05 — HTTP, JSON & Fetch ←](../05-http-json-fetch/notes.md) · **Next:** [07 — Node.js & Express →](../07-nodejs-express/notes.md)


## Table of Contents

### Part I: API Foundations
- [Chapter 1: Introduction to APIs](#chapter-1-introduction-to-apis)
- [Chapter 2: Client-Server Architecture](#chapter-2-client-server-architecture)
- [Chapter 3: HTTP Basics](#chapter-3-http-basics)
- [Chapter 4: HTTP Request Lifecycle](#chapter-4-http-request-lifecycle)
- [Chapter 5: HTTP Methods](#chapter-5-http-methods)

### Part II: REST API Design
- [Chapter 6: REST API Design Principles](#chapter-6-rest-api-design-principles)
- [Chapter 7: Designing Good Endpoints](#chapter-7-designing-good-endpoints)
- [Chapter 8: Route Parameters](#chapter-8-route-parameters)
- [Chapter 9: Query Parameters](#chapter-9-query-parameters)
- [Chapter 10: Request Body](#chapter-10-request-body)
- [Chapter 11: Response Body](#chapter-11-response-body)
- [Chapter 12: CRUD Operations](#chapter-12-crud-operations)

### Part III: Status Codes & Error Handling
- [Chapter 13: HTTP Status Codes](#chapter-13-http-status-codes)
- [Chapter 14: Validation](#chapter-14-validation)
- [Chapter 15: Error Handling](#chapter-15-error-handling)

### Part IV: Security & Documentation
- [Chapter 16: Authentication Overview](#chapter-16-authentication-overview)
- [Chapter 17: Security Best Practices](#chapter-17-security-best-practices)
- [Chapter 18: API Documentation](#chapter-18-api-documentation)

### Part V: Implementation & Reference
- [Chapter 19: Complete Express CRUD Example](#chapter-19-complete-express-crud-example)
- [Chapter 20: Best Practices](#chapter-20-best-practices)
- [Chapter 21: Common Mistakes](#chapter-21-common-mistakes)
- [Chapter 22: Interview Questions](#chapter-22-interview-questions)
- [Chapter 23: Practice Projects](#chapter-23-practice-projects)
- [Chapter 24: Cheat Sheet](#chapter-24-cheat-sheet)
- [Chapter 25: Summary](#chapter-25-summary)


# CHAPTER 1: Introduction to APIs

### What is an API?

**API** stands for **Application Programming Interface**.

An API is a **set of rules and definitions** that allows one software application to talk to another. It acts as a **contract** — defining what requests can be made, how to make them, and what responses to expect.

> **Think of it like this:** An API is a **menu at a restaurant**.
> - The **menu** tells you what dishes are available (what requests you can make).
> - You, the **customer**, are the client (your app or browser).
> - The **waiter** is the API (the middleman that carries your request).
> - The **kitchen** is the server (where the actual work happens).
> - The **chef** prepares the food (the server processes your request).
> - The waiter brings the food back to you (the server returns a response).
>
> You don't need to know *how* the kitchen works. You just use the menu (API).

### Why Do APIs Exist?

Before APIs, if you wanted to share data between applications, you would have to directly access the database of the other application — which is a huge security and maintenance problem.

APIs solve this by providing a **controlled, standardised interface**:

| Problem Without APIs | How APIs Solve It |
|:---------------------|:------------------|
| Direct database access is insecure | API acts as a gatekeeper, exposing only what is needed |
| Every team uses different formats | APIs enforce a standard format (usually JSON) |
| Front-end and back-end are tightly coupled | APIs decouple them — each can be developed independently |
| Mobile apps, web apps need the same data | One API serves all clients |
| Third parties can't integrate easily | APIs provide a public contract for integration |

### Real-World API Examples

| Company | What Their API Does |
|:--------|:--------------------|
| **Google Maps** | Returns location data, directions, geocoding |
| **Stripe** | Processes payments, creates invoices |
| **Twilio** | Sends SMS messages and makes calls |
| **OpenWeatherMap** | Returns weather data for any city |
| **GitHub** | Manages repositories, issues, pull requests |
| **Twitter/X** | Posts tweets, fetches timelines |

### History of APIs

| Era | Technology | Description |
|:----|:-----------|:------------|
| **1970s–1990s** | RPC (Remote Procedure Call) | Programs called procedures on remote machines |
| **2000s** | SOAP (Simple Object Access Protocol) | XML-based messaging over HTTP; very strict |
| **2000** | REST introduced | Roy Fielding's PhD thesis introduced REST principles |
| **2010s** | REST dominates | REST + JSON becomes the standard for web APIs |
| **2015** | GraphQL by Facebook | Query language for APIs; client controls the shape of data |
| **2016** | gRPC by Google | High-performance API protocol using Protocol Buffers |
| **2020s** | REST + GraphQL + gRPC | All three coexist for different use cases |


# CHAPTER 2: Client-Server Architecture

### The Three-Tier Model

Modern web applications follow a **three-tier architecture**:

```
┌─────────────────────────────────────────────────┐
│                   CLIENT TIER                   │
│   Browser / Mobile App / Desktop App / CLI      │
│   (Displays UI, sends HTTP requests)            │
└──────────────────────┬──────────────────────────┘
                       │  HTTP Request
                       ▼
┌─────────────────────────────────────────────────┐
│                  SERVER TIER                    │
│   Node.js + Express / Django / Spring Boot      │
│   (Business Logic, Validation, Auth, Routing)   │
└──────────────────────┬──────────────────────────┘
                       │  SQL / NoSQL Query
                       ▼
┌─────────────────────────────────────────────────┐
│                DATABASE TIER                    │
│   PostgreSQL / MongoDB / MySQL / Redis           │
│   (Stores and retrieves persistent data)        │
└─────────────────────────────────────────────────┘
```

### Communication Flow — Step by Step

```
User clicks "Get All Users" button in the browser
          │
          ▼
  Browser builds HTTP Request
  GET https://api.example.com/v1/users
          │
          ▼
  Request travels over the Internet (TCP/IP)
          │
          ▼
  Server receives the request
  Express.js matches the route: GET /users
          │
          ▼
  Business Logic runs:
  - Validate authentication token
  - Check user permissions
  - Sanitise query parameters
          │
          ▼
  Database query executes:
  SELECT * FROM users LIMIT 20;
          │
          ▼
  Database returns rows to the server
          │
          ▼
  Server builds a JSON response:
  { "success": true, "data": [...] }
          │
          ▼
  HTTP Response sent back to the browser
  Status: 200 OK
          │
          ▼
  Browser receives the response
  JavaScript parses JSON and renders the UI
```

### Client

The **client** is any application that **sends a request** to an API. It does not care how the server works internally — it only cares about the API contract.

Examples: Chrome browser, React app, Android app, Postman, cURL, another server.

### Server

The **server** is the application that **receives requests**, processes them, and **sends back responses**. It contains all the business logic.

### Database

The **database** is where data is permanently stored. The server queries it based on what the client needs. The client never talks directly to the database — the server is the only one that can.

> **Interview Tip:** "Why does the client not access the database directly?"
> Because it would expose your credentials, allow data tampering, bypass business logic, and be a massive security vulnerability.


# CHAPTER 3: HTTP Basics

### What is HTTP?

**HTTP** stands for **HyperText Transfer Protocol**. It is the foundation of data communication on the web. Every time you open a webpage or call an API, you are using HTTP.

HTTP defines:
- How requests are formatted and sent
- How responses are formatted and returned
- The meaning of status codes, methods, and headers

### HTTPS

**HTTPS** = HTTP + **TLS encryption** (Transport Layer Security).

All data sent over HTTPS is encrypted. This means:
- Passwords cannot be intercepted
- Tokens cannot be stolen in transit
- API responses cannot be read by a third party

> **Best Practice:** Always use HTTPS in production. Never expose APIs over plain HTTP.

### HTTP is Stateless

HTTP is a **stateless protocol** — this means:

- Each HTTP request is **completely independent**.
- The server does **not remember** any previous request.
- If a user logs in and then makes another request, the server has no idea who they are unless the second request also carries proof of identity (a token or cookie).

This is why authentication tokens exist — to carry identity on every request.

### Anatomy of an HTTP Request

```
Method   URL                                  HTTP Version
  │       │                                       │
  ▼       ▼                                       ▼
POST https://api.example.com/v1/users   HTTP/1.1

Headers:
Content-Type: application/json
Authorization: Bearer eyJhbGci...
Accept: application/json

Body (payload):
{
  "name": "Tushar",
  "email": "tushar@example.com"
}
```

**Parts of an HTTP Request:**

| Part | Description | Example |
|:-----|:------------|:--------|
| **Method** | The action to perform | `GET`, `POST`, `PUT`, `DELETE` |
| **URL** | The address of the resource | `https://api.example.com/v1/users` |
| **Headers** | Metadata about the request | `Content-Type: application/json` |
| **Body** | The data being sent (not used in GET) | `{ "name": "Tushar" }` |

### Anatomy of an HTTP Response

```
HTTP Version   Status Code   Status Text
     │              │             │
     ▼              ▼             ▼
  HTTP/1.1       200          OK

Headers:
Content-Type: application/json
X-RateLimit-Remaining: 99

Body:
{
  "success": true,
  "data": { "id": 1, "name": "Tushar" }
}
```

**Parts of an HTTP Response:**

| Part | Description | Example |
|:-----|:------------|:--------|
| **Status Code** | Numeric result of the request | `200`, `404`, `500` |
| **Headers** | Metadata about the response | `Content-Type: application/json` |
| **Body** | The returned data | `{ "success": true, "data": {...} }` |

### Key Terms

| Term | Definition | Example |
|:-----|:-----------|:--------|
| **URL** | Uniform Resource Locator — the full address | `https://api.example.com/v1/users?page=2` |
| **Endpoint** | A specific URL where an API receives requests | `GET /api/v1/users` |
| **Port** | A number that identifies a specific service on a server | `:3000`, `:443`, `:80` |
| **Protocol** | The set of rules for communication | `HTTP`, `HTTPS`, `WebSocket` |
| **Header** | Key-value metadata in a request or response | `Authorization: Bearer token` |
| **Body** | The data payload in a request or response | `{ "email": "test@test.com" }` |
| **Base URL** | The common prefix for all API endpoints | `https://api.example.com/v1` |
| **Path** | The route part of the URL | `/users/123/posts` |

### Common HTTP Headers

| Header | Direction | Purpose | Example |
|:-------|:----------|:--------|:--------|
| `Content-Type` | Request & Response | Format of the body | `application/json` |
| `Authorization` | Request | Authentication credential | `Bearer eyJhbGci...` |
| `Accept` | Request | Format client expects | `application/json` |
| `Cache-Control` | Response | Caching instructions | `no-cache, no-store` |
| `X-Request-ID` | Both | Unique request identifier for tracing | `550e8400-e29b-41d4` |


# CHAPTER 4: HTTP Request Lifecycle

When you make an API call, many steps happen in milliseconds. Understanding this lifecycle helps you debug issues.

```
┌──────────────────────────────────────────────────────────┐
│  Step 1: User Action                                     │
│  User clicks a button or your code calls fetch()         │
└──────────────────────────────┬───────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────┐
│  Step 2: DNS Resolution                                  │
│  "api.example.com" → resolves to IP address 203.0.113.1 │
└──────────────────────────────┬───────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────┐
│  Step 3: TCP Connection                                  │
│  Browser opens a TCP connection to the server IP        │
│  HTTPS → TLS handshake to establish encrypted channel   │
└──────────────────────────────┬───────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────┐
│  Step 4: HTTP Request Sent                               │
│  GET /v1/users HTTP/1.1                                  │
│  Host: api.example.com                                   │
│  Authorization: Bearer <token>                           │
└──────────────────────────────┬───────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────┐
│  Step 5: Middleware Pipeline (Express)                   │
│  → Logging middleware logs the request                   │
│  → CORS middleware checks allowed origins               │
│  → Auth middleware verifies the JWT token               │
│  → Rate limiter checks request count                    │
└──────────────────────────────┬───────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────┐
│  Step 6: Route Handler                                   │
│  Express matches: GET /v1/users → usersController.list  │
└──────────────────────────────┬───────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────┐
│  Step 7: Business Logic & Database                       │
│  → Validate query parameters                            │
│  → Execute: SELECT * FROM users LIMIT 20                │
│  → Transform database rows into JSON                    │
└──────────────────────────────┬───────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────┐
│  Step 8: HTTP Response                                   │
│  HTTP/1.1 200 OK                                         │
│  Content-Type: application/json                          │
│  { "success": true, "data": [...] }                     │
└──────────────────────────────┬───────────────────────────┘
                               │
                               ▼
┌──────────────────────────────────────────────────────────┐
│  Step 9: Client Receives Response                        │
│  Browser parses JSON, updates the DOM                   │
└──────────────────────────────────────────────────────────┘
```


# CHAPTER 5: HTTP Methods

HTTP methods tell the server **what action** to perform on the resource. Choosing the correct method is fundamental to good API design.

### Overview Table

| Method | Purpose | Has Body? | Safe? | Idempotent? |
|:-------|:--------|:---------:|:-----:|:-----------:|
| **GET** | Retrieve resource(s) | No | ✅ | ✅ |
| **POST** | Create a new resource | Yes | ❌ | ❌ |
| **PUT** | Replace entire resource | Yes | ❌ | ✅ |
| **PATCH** | Partially update resource | Yes | ❌ | ✅ |
| **DELETE** | Remove a resource | No | ❌ | ✅ |
| **HEAD** | Like GET, but returns headers only | No | ✅ | ✅ |
| **OPTIONS** | Returns supported methods | No | ✅ | ✅ |

> **Safe** = No side effects; the server state does not change.
> **Idempotent** = Calling it multiple times produces the same result as calling it once.


### GET

**Purpose:** Retrieve data from the server. Never modifies anything.

```
GET /api/v1/users          → Get all users
GET /api/v1/users/123      → Get user with ID 123
GET /api/v1/users?role=admin → Get all admin users
```

**Express Example:**
```javascript
const express = require('express');
const app = express();

const users = [
  { id: 1, name: 'Alice', role: 'admin' },
  { id: 2, name: 'Bob',   role: 'user'  },
];

// GET all users
app.get('/api/v1/users', (req, res) => {
  res.status(200).json({
    success: true,
    data: users,
    message: 'Users retrieved successfully.'
  });
});

// GET single user by ID
app.get('/api/v1/users/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const user = users.find(u => u.id === id);

  if (!user) {
    return res.status(404).json({
      success: false,
      error: { code: 'NOT_FOUND', message: `User ${id} not found.` }
    });
  }

  res.status(200).json({ success: true, data: user });
});
```

**Real-World Use Cases:**
- `GET /products` — List all products in an e-commerce store
- `GET /users/me` — Get the current logged-in user's profile
- `GET /posts/slug-here` — Fetch a blog post by its slug

> **Rule:** GET requests must **never** change server state. They should be safe to call repeatedly.


### POST

**Purpose:** Create a new resource on the server. The server determines the new resource's ID.

```
POST /api/v1/users          → Create a new user
POST /api/v1/posts          → Create a new blog post
POST /api/v1/auth/login     → Authenticate (special case: not creating a resource, but "creating" a session)
```

**Express Example:**
```javascript
app.use(express.json()); // Required to parse JSON body

app.post('/api/v1/users', (req, res) => {
  const { name, email } = req.body;

  // Validate required fields
  if (!name || !email) {
    return res.status(400).json({
      success: false,
      error: {
        code: 'VALIDATION_ERROR',
        message: 'Name and email are required.'
      }
    });
  }

  // Create new user (in real apps, save to database)
  const newUser = {
    id: users.length + 1,
    name,
    email
  };
  users.push(newUser);

  // Return 201 Created with the new resource
  res.status(201).json({
    success: true,
    data: newUser,
    message: 'User created successfully.'
  });
});
```

**Key Rule:** Always return **201 Created** (not 200) when a resource is successfully created.


### PUT

**Purpose:** **Replace** an entire resource. The client sends the complete updated version of the resource.

```
PUT /api/v1/users/123
Body: { "name": "Alice Updated", "email": "alice@new.com", "role": "admin" }
```

> If the user had 10 fields and you PUT with only 3 fields, the other 7 fields get **deleted/reset**. PUT replaces the whole thing.

**Express Example:**
```javascript
app.put('/api/v1/users/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = users.findIndex(u => u.id === id);

  if (index === -1) {
    return res.status(404).json({
      success: false,
      error: { code: 'NOT_FOUND', message: `User ${id} not found.` }
    });
  }

  const { name, email, role } = req.body;

  // Validate all required fields (PUT requires the full object)
  if (!name || !email || !role) {
    return res.status(400).json({
      success: false,
      error: { code: 'VALIDATION_ERROR', message: 'name, email, and role are all required for PUT.' }
    });
  }

  // Replace entire resource
  users[index] = { id, name, email, role };

  res.status(200).json({ success: true, data: users[index] });
});
```


### PATCH

**Purpose:** **Partially update** a resource. Only the fields sent in the body are changed; everything else stays the same.

```
PATCH /api/v1/users/123
Body: { "email": "newemail@example.com" }
→ Only updates email; name, role etc. remain unchanged.
```

**Express Example:**
```javascript
app.patch('/api/v1/users/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = users.findIndex(u => u.id === id);

  if (index === -1) {
    return res.status(404).json({
      success: false,
      error: { code: 'NOT_FOUND', message: `User ${id} not found.` }
    });
  }

  // Merge existing data with provided updates
  users[index] = { ...users[index], ...req.body };

  res.status(200).json({ success: true, data: users[index] });
});
```

### PUT vs PATCH — Key Differences

| Aspect | PUT | PATCH |
|:-------|:----|:------|
| **What it does** | Replaces the entire resource | Updates only the fields provided |
| **Body required?** | Yes — full resource | Yes — only changed fields |
| **Missing fields** | Get deleted / reset to null | Remain unchanged |
| **Idempotent?** | Yes | Yes (if deterministic) |
| **Use case** | Replacing an entire document | Changing a single field |

```
Existing user: { id: 1, name: "Alice", email: "old@email.com", role: "user" }

PUT  body: { name: "Alice", role: "admin" }
Result:    { id: 1, name: "Alice", email: null, role: "admin" }  ← email is lost!

PATCH body: { role: "admin" }
Result:    { id: 1, name: "Alice", email: "old@email.com", role: "admin" }  ← safe!
```


### DELETE

**Purpose:** Remove a resource from the server.

```
DELETE /api/v1/users/123    → Delete user with ID 123
```

**Express Example:**
```javascript
app.delete('/api/v1/users/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = users.findIndex(u => u.id === id);

  if (index === -1) {
    return res.status(404).json({
      success: false,
      error: { code: 'NOT_FOUND', message: `User ${id} not found.` }
    });
  }

  users.splice(index, 1);

  // 204 No Content — success, but nothing to return
  res.status(204).send();
});
```

> **Best Practice:** Return **204 No Content** on successful delete (no body). Some APIs return 200 with a confirmation message — both are acceptable.


### HEAD

**Purpose:** Identical to GET, but the server returns **only the headers** — no body. Useful for checking if a resource exists or getting its size without downloading it.

```
HEAD /api/v1/files/video.mp4
→ Returns Content-Length, Content-Type etc., but not the file itself.
```

---

### OPTIONS

**Purpose:** Returns the HTTP methods that the server supports for a specific endpoint. Used by browsers in **CORS preflight requests**.

```
OPTIONS /api/v1/users
→ Response: Allow: GET, POST, PUT, DELETE, OPTIONS
```


# CHAPTER 6: REST API Design Principles

### What is REST?

**REST** = **Representational State Transfer**

REST is an **architectural style** (a set of rules) for designing APIs over HTTP. It was defined by Roy Fielding in his doctoral dissertation in 2000.

An API that follows these rules is called a **RESTful API**.

REST is not a protocol or a library — it is a philosophy about how to design networked systems.

### The 6 REST Constraints

#### 1. Client-Server Separation

The client (frontend) and server (backend) are completely separate systems. They communicate only through the API. This means:
- The frontend can be rewritten without touching the backend
- The backend can be scaled independently
- Mobile apps, web apps, and CLI tools can all use the same API

#### 2. Statelessness

> **This is the most important REST principle.**

Every HTTP request must carry **all the information** the server needs to process it. The server stores **no session state** between requests.

```
❌ Stateful (NOT REST):
  Request 1: POST /login  → server stores your session
  Request 2: GET /profile → server remembers who you are from session

✅ Stateless (REST):
  Request 1: POST /login  → server returns a JWT token
  Request 2: GET /profile + Authorization: Bearer <token> → server reads identity from the token
```

#### 3. Cacheability

Responses should indicate whether they can be cached. Caching improves performance by letting clients or CDNs reuse responses.

```
Cache-Control: public, max-age=3600   ← Can be cached for 1 hour
Cache-Control: no-store               ← Must never be cached (sensitive data)
```

#### 4. Uniform Interface

All resources follow the same consistent rules:
- Resources are identified by URIs (`/users`, `/products/5`)
- Resources are manipulated using standard HTTP methods
- Responses use a standard format (JSON)

#### 5. Layered System

The client doesn't know (and doesn't care) how many layers exist between it and the database. The request might go through:
```
Client → Load Balancer → CDN → API Gateway → Express Server → Database
```
This enables horizontal scaling, caching, and security without the client changing.

#### 6. Code on Demand *(Optional)*

The server may send executable code to the client (e.g., JavaScript). This is rarely used in REST APIs.

### Richardson Maturity Model

A scale for measuring how "RESTful" an API is:

```
Level 0 ──── Level 1 ──── Level 2 ──── Level 3
 POX            Resources   HTTP Verbs  HATEOAS
 (RPC-like)     /users      GET/POST    + Links
```

| Level | Name | Example | Notes |
|:------|:-----|:--------|:------|
| **0** | Swamp of POX | `POST /api?action=getUser` | Not REST at all |
| **1** | Resources | `GET /users`, `GET /products` | URIs per resource |
| **2** | HTTP Verbs | `GET /users`, `POST /users`, `DELETE /users/1` | Industry standard |
| **3** | HATEOAS | Response includes `"links": { "self": "...", "delete": "..." }` | Rare in practice |

> **Most production APIs operate at Level 2.** Aim for Level 2.


# CHAPTER 7: Designing Good Endpoints

### The Golden Rule

> **URIs identify resources (nouns). HTTP methods define actions (verbs).**

The URL should answer: **"What thing am I acting on?"**
The HTTP method should answer: **"What am I doing to it?"**

### Bad vs Good Endpoints

| ❌ Bad (verb in URL) | ✅ Good (noun in URL) | Why? |
|:---------------------|:----------------------|:-----|
| `POST /createUser` | `POST /users` | Method already says "create" |
| `GET /getUsers` | `GET /users` | Method already says "get" |
| `POST /users/123/delete` | `DELETE /users/123` | Use the DELETE method |
| `GET /getUserById?id=123` | `GET /users/123` | Use path parameter |
| `POST /updateUserEmail` | `PATCH /users/123` | Use PATCH with path param |

### Naming Rules

```
✅ Use plural nouns:      /users        /products      /orders
✅ Use lowercase:         /blog-posts   /user-profiles
✅ Use hyphens:           /company-reports              (not underscores)
✅ Be consistent:         /users/:id/posts              (not /user/:id/Post)
❌ Never use verbs:       /getUsers     /deletePost     /createOrder
❌ No file extensions:    /users.json   /data.xml
❌ No trailing slashes:   /users/       (can cause routing issues)
```

### Nested Resources

Use nesting when there is a clear parent-child relationship:

```
GET  /posts/5/comments         → All comments on post 5
POST /posts/5/comments         → Create a comment on post 5
GET  /posts/5/comments/42      → Specific comment on post 5
```

> **Rule:** Limit nesting to **2 levels maximum**. Deeper nesting is hard to read and maintain.

```
❌ Too deep: GET /users/1/posts/5/comments/42/likes/9
✅ Better:   GET /comments/42/likes/9
```

### API Versioning in the URL

Always version your API from day one:

```
https://api.example.com/v1/users
https://api.example.com/v2/users    ← Breaking changes go here
```

This lets you make breaking changes in `v2` without breaking `v1` clients.


# CHAPTER 8: Route Parameters

Route parameters are **variable segments** in a URL path. They are used to identify a **specific resource**.

### Syntax

In Express.js, route parameters are defined with a colon prefix (`:`):

```
/users/:id           ← :id is the parameter
/posts/:postId/comments/:commentId
```

### Accessing Parameters

```javascript
app.get('/api/v1/users/:id', (req, res) => {
  // req.params contains all route parameters
  console.log(req.params);       // { id: '123' }
  console.log(req.params.id);    // '123'  (always a string!)

  const id = parseInt(req.params.id); // Convert to number

  if (isNaN(id)) {
    return res.status(400).json({
      success: false,
      error: { code: 'INVALID_ID', message: 'ID must be a number.' }
    });
  }

  const user = users.find(u => u.id === id);

  if (!user) {
    return res.status(404).json({
      success: false,
      error: { code: 'NOT_FOUND', message: `User ${id} not found.` }
    });
  }

  res.status(200).json({ success: true, data: user });
});
```

### Multiple Route Parameters

```javascript
// GET /posts/5/comments/42
app.get('/api/v1/posts/:postId/comments/:commentId', (req, res) => {
  const { postId, commentId } = req.params;
  // postId = '5', commentId = '42'

  res.status(200).json({ success: true, data: { postId, commentId } });
});
```

### Route Parameters vs Query Parameters

| Feature | Route Parameter | Query Parameter |
|:--------|:----------------|:----------------|
| **Position** | Inside the path | After `?` in the URL |
| **Purpose** | Identify a specific resource | Filter, sort, search a collection |
| **Required?** | Always required | Usually optional |
| **Example** | `/users/123` | `/users?role=admin` |
| **Express access** | `req.params.id` | `req.query.role` |


# CHAPTER 9: Query Parameters

Query parameters are **key-value pairs** appended to a URL after `?`. Multiple parameters are separated by `&`.

```
/users?role=admin&status=active&page=2&limit=10
        │              │          │       │
        └─ key=value   └─ key=value └─ key └─ value
```

### Accessing Query Parameters in Express

```javascript
app.get('/api/v1/users', (req, res) => {
  console.log(req.query);
  // { role: 'admin', status: 'active', page: '2', limit: '10' }

  const { role, status, page = 1, limit = 10 } = req.query;

  // Note: query params are always strings — convert types as needed
  const pageNum  = parseInt(page);
  const limitNum = parseInt(limit);

  // Filter in-memory (in real apps, pass to DB query)
  let result = users;
  if (role)   result = result.filter(u => u.role === role);
  if (status) result = result.filter(u => u.status === status);

  // Pagination (simple slice)
  const start     = (pageNum - 1) * limitNum;
  const paginated = result.slice(start, start + limitNum);

  res.status(200).json({
    success: true,
    data: paginated,
    meta: {
      total: result.length,
      page: pageNum,
      limit: limitNum,
      totalPages: Math.ceil(result.length / limitNum)
    }
  });
});
```

### Common Query Parameter Patterns

#### Filtering

```
GET /products?category=electronics&brand=apple&inStock=true
```

#### Sorting

```
GET /users?sort=name         → Sort by name ascending
GET /users?sort=-createdAt   → Sort by createdAt descending (- prefix = descending)
```

#### Pagination

```
GET /posts?page=3&limit=20
```

#### Searching

```
GET /users?search=tushar      → Search users by name/email
GET /products?q=wireless+headphones
```


# CHAPTER 10: Request Body

The **request body** contains data that the client sends to the server to create or update a resource. It is used with POST, PUT, and PATCH requests.

### Why JSON?

JSON (JavaScript Object Notation) is the universal data format for APIs because:
- It is human-readable
- It is lightweight
- All programming languages can parse it
- It maps naturally to JavaScript objects

### Setting Up Express to Read JSON Bodies

```javascript
const express = require('express');
const app = express();

// This middleware parses incoming JSON bodies
// Without this, req.body will be undefined
app.use(express.json());
```

### Example: Creating a User

**Request:**
```
POST /api/v1/users
Content-Type: application/json

{
  "name": "Tushar",
  "email": "tushar@example.com",
  "password": "SecurePass123!",
  "role": "user"
}
```

**Express Handler:**
```javascript
app.post('/api/v1/users', (req, res) => {
  // req.body is the parsed JSON object
  const { name, email, password, role } = req.body;

  console.log(req.body);
  // { name: 'Tushar', email: 'tushar@example.com', ... }

  // Process and respond
  res.status(201).json({ success: true, data: { id: 99, name, email } });
});
```

### Nested JSON Bodies

```json
{
  "name": "Tushar",
  "address": {
    "street": "123 Main St",
    "city": "Bengaluru",
    "pincode": "560001"
  },
  "skills": ["Node.js", "React", "SQL"]
}
```

```javascript
app.post('/api/v1/users', (req, res) => {
  const { name, address, skills } = req.body;
  console.log(address.city);   // 'Bengaluru'
  console.log(skills[0]);      // 'Node.js'
});
```


# CHAPTER 11: Response Body

Every API response should follow a **consistent structure**. This makes it predictable for the client — it always knows where to find the data, errors, and metadata.

### Standard Success Response

```json
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Tushar",
    "email": "tushar@example.com"
  },
  "message": "User retrieved successfully."
}
```

### Standard Error Response

```json
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "User with ID 999 does not exist."
  }
}
```

### Paginated Collection Response

```json
{
  "success": true,
  "data": [
    { "id": 1, "name": "Alice" },
    { "id": 2, "name": "Bob" }
  ],
  "meta": {
    "total": 150,
    "page": 2,
    "limit": 20,
    "totalPages": 8
  },
  "links": {
    "self": "/api/v1/users?page=2",
    "next": "/api/v1/users?page=3",
    "prev": "/api/v1/users?page=1"
  }
}
```

### Express Response Methods

```javascript
// Send JSON with status code
res.status(200).json({ success: true, data: user });

// Send no body (for DELETE)
res.status(204).send();

// Shorthand (defaults to 200)
res.json({ success: true });
```


# CHAPTER 12: CRUD Operations

**CRUD** stands for the four basic operations every data-driven application performs:

| Letter | Operation | HTTP Method | SQL Equivalent |
|:-------|:----------|:------------|:---------------|
| **C** | Create | `POST` | `INSERT` |
| **R** | Read | `GET` | `SELECT` |
| **U** | Update | `PUT` / `PATCH` | `UPDATE` |
| **D** | Delete | `DELETE` | `DELETE` |

### CRUD Blueprint for a `products` Resource

| Action | Method | Endpoint | Request Body | Success Response |
|:-------|:-------|:---------|:-------------|:----------------|
| Create a product | POST | `/products` | `{ name, price, category }` | `201 Created` |
| Get all products | GET | `/products` | — | `200 OK` + array |
| Get one product | GET | `/products/:id` | — | `200 OK` + object |
| Replace product | PUT | `/products/:id` | `{ name, price, category }` | `200 OK` |
| Update one field | PATCH | `/products/:id` | `{ price: 999 }` | `200 OK` |
| Delete product | DELETE | `/products/:id` | — | `204 No Content` |


# CHAPTER 13: HTTP Status Codes

### Why Status Codes Matter

HTTP status codes are **three-digit numbers** the server returns to tell the client **what happened**. They are critical for:
- Debugging API issues
- Client-side error handling
- Communicating intent
- Interview questions!

### Groups Overview

```
1xx  →  Informational    (request received, continue processing)
2xx  →  Success          (request was successfully received and processed)
3xx  →  Redirection      (further action needed to complete the request)
4xx  →  Client Error     (client made a bad request)
5xx  →  Server Error     (server failed to handle a valid request)
```


### 1xx — Informational

Rarely used in REST APIs directly. The server uses them to communicate intermediate steps.

| Code | Name | Meaning |
|:-----|:-----|:--------|
| **100** | Continue | Server received headers; client should proceed to send body |
| **101** | Switching Protocols | Server agrees to switch to WebSocket protocol |
| **103** | Early Hints | Server sends some headers before the full response |

> **101 Switching Protocols** is what makes WebSockets work. The browser sends `Upgrade: websocket` header and the server responds with 101.


### 2xx — Success

#### 200 OK

| | |
|:--|:--|
| **Meaning** | The request was successful |
| **Use when** | GET, PUT, PATCH requests succeed |
| **Don't use when** | A new resource was created (use 201) |

```javascript
// GET — return resource
app.get('/api/v1/users/:id', (req, res) => {
  res.status(200).json({
    success: true,
    data: { id: 1, name: 'Alice' },
    message: 'User retrieved successfully.'
  });
});
```

> **Interview Tip:** "What's the difference between 200 and 201?" — 200 is for successful reads/updates; 201 is for successful **creation** of a new resource.


#### 201 Created

| | |
|:--|:--|
| **Meaning** | A new resource was successfully created |
| **Use when** | POST requests that create a new resource succeed |
| **Best Practice** | Include the newly created resource in the response body and optionally a `Location` header pointing to the new resource |

```javascript
app.post('/api/v1/users', (req, res) => {
  const newUser = { id: 99, name: req.body.name };
  users.push(newUser);

  res.status(201)
     .set('Location', `/api/v1/users/${newUser.id}`)
     .json({
       success: true,
       data: newUser,
       message: 'User created successfully.'
     });
});
```

> **Common Mistake:** Returning `200 OK` instead of `201 Created` when a resource is created. This is technically wrong and makes the API less informative.


#### 202 Accepted

| | |
|:--|:--|
| **Meaning** | The request was accepted for **async processing** but not yet completed |
| **Use when** | Long-running jobs (video encoding, email campaigns, batch imports) |

```javascript
app.post('/api/v1/reports/generate', (req, res) => {
  // Start async job (e.g., push to message queue)
  const jobId = startAsyncJob(req.body);

  res.status(202).json({
    success: true,
    message: 'Report generation started.',
    data: {
      jobId,
      statusUrl: `/api/v1/jobs/${jobId}`
    }
  });
});
```


#### 204 No Content

| | |
|:--|:--|
| **Meaning** | Success, but there is **no body to return** |
| **Use when** | DELETE requests, or PUT/PATCH when you don't need to return the updated resource |
| **Important** | The response must have **no body** |

```javascript
app.delete('/api/v1/users/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = users.findIndex(u => u.id === id);

  if (index === -1) {
    return res.status(404).json({ success: false, error: { message: 'User not found.' } });
  }

  users.splice(index, 1);
  res.status(204).send(); // No body!
});
```



### 3xx — Redirection

#### 301 Moved Permanently

| | |
|:--|:--|
| **Meaning** | The resource has **permanently moved** to a new URL |
| **Use when** | API endpoint URL has changed permanently (e.g., renaming `/api/users` to `/api/v1/users`) |
| **Effect** | Browsers and clients should update bookmarks/links to the new URL |

```javascript
app.get('/api/users', (req, res) => {
  res.status(301).redirect('/api/v1/users');
});
```


#### 302 Found (Temporary Redirect)

| | |
|:--|:--|
| **Meaning** | The resource is **temporarily** at another URL |
| **Use when** | Temporarily redirecting (maintenance page, A/B testing) |
| **Effect** | Client should continue using the original URL for future requests |


#### 304 Not Modified

| | |
|:--|:--|
| **Meaning** | The resource has not changed since the client's last request |
| **Use when** | Client sends `If-None-Match` or `If-Modified-Since` headers and the resource hasn't changed |
| **Effect** | No body is returned — the client uses its cached version, saving bandwidth |

```javascript
app.get('/api/v1/products', (req, res) => {
  const etag = 'abc123'; // Hash of current data

  if (req.headers['if-none-match'] === etag) {
    return res.status(304).send(); // Data unchanged, use cache
  }

  res.set('ETag', etag).status(200).json({ data: products });
});
```

> **Interview Tip:** 304 is a caching optimization. It saves bandwidth by not re-sending data the client already has.


#### 307 Temporary Redirect

Like 302, but the client **must** use the **same HTTP method** for the redirected request (302 may change POST to GET).

#### 308 Permanent Redirect

Like 301, but the client **must** use the **same HTTP method** for the redirected request.


### 4xx — Client Errors

These mean **the client made a mistake**. The server understood the request but can't fulfill it.

#### 400 Bad Request

| | |
|:--|:--|
| **Meaning** | The request has invalid syntax or missing required data |
| **Use when** | JSON parsing fails, required fields are missing, data types are wrong |
| **Common mistake** | Using 400 for authorization errors (use 401/403 for those) |

```javascript
app.post('/api/v1/users', (req, res) => {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({
      success: false,
      error: {
        code: 'MISSING_FIELDS',
        message: 'Name and email are required.',
        details: [
          !name  && { field: 'name',  message: 'Name is required.' },
          !email && { field: 'email', message: 'Email is required.' }
        ].filter(Boolean)
      }
    });
  }
});
```


#### 401 Unauthorized

| | |
|:--|:--|
| **Meaning** | Authentication is required but missing or invalid |
| **Use when** | No token provided, token is expired, token signature is invalid |
| **Common mistake** | Using 403 when the user is not authenticated (should be 401) |

> **Memory Trick:** 401 = "Who are you?" (not authenticated). 403 = "I know who you are, but you can't do this" (not authorized).

```javascript
app.get('/api/v1/admin', (req, res) => {
  const token = req.headers.authorization?.split(' ')[1];

  if (!token) {
    return res.status(401).json({
      success: false,
      error: {
        code: 'MISSING_TOKEN',
        message: 'Authentication token is required. Please login first.'
      }
    });
  }

  // verify token...
});
```


#### 403 Forbidden

| | |
|:--|:--|
| **Meaning** | The client is authenticated but **lacks permission** for this resource |
| **Use when** | User is logged in but doesn't have the right role/permission |
| **Common mistake** | Returning 401 when user is logged in but not allowed |

```javascript
app.delete('/api/v1/users/:id', (req, res) => {
  // Assume req.user is set by auth middleware
  if (req.user.role !== 'admin') {
    return res.status(403).json({
      success: false,
      error: {
        code: 'FORBIDDEN',
        message: 'Only administrators can delete users.'
      }
    });
  }
});
```


#### 404 Not Found

| | |
|:--|:--|
| **Meaning** | The requested resource does not exist |
| **Use when** | `GET /users/999` and user 999 doesn't exist; route doesn't match |
| **Best Practice** | Be careful not to reveal sensitive information (don't say "user exists but you can't see it" — just say not found) |

```javascript
app.get('/api/v1/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));

  if (!user) {
    return res.status(404).json({
      success: false,
      error: {
        code: 'NOT_FOUND',
        message: `User with ID ${req.params.id} was not found.`
      }
    });
  }

  res.status(200).json({ success: true, data: user });
});

// Catch-all 404 for unknown routes
app.use((req, res) => {
  res.status(404).json({
    success: false,
    error: { code: 'ROUTE_NOT_FOUND', message: `Route ${req.method} ${req.path} does not exist.` }
  });
});
```


#### 405 Method Not Allowed

| | |
|:--|:--|
| **Meaning** | The HTTP method is not supported for this endpoint |
| **Use when** | Client sends DELETE to an endpoint that only supports GET/POST |
| **Best Practice** | Include an `Allow` header listing the permitted methods |

```javascript
// If someone sends PATCH to /auth/login, return 405
res.status(405)
   .set('Allow', 'GET, POST')
   .json({
     success: false,
     error: {
       code: 'METHOD_NOT_ALLOWED',
       message: 'PATCH is not allowed on /auth/login. Use POST.'
     }
   });
```


#### 409 Conflict

| | |
|:--|:--|
| **Meaning** | The request conflicts with the current state of the resource |
| **Use when** | Duplicate email registration, username already taken, version conflict in optimistic locking |

```javascript
app.post('/api/v1/users', async (req, res) => {
  const existing = users.find(u => u.email === req.body.email);

  if (existing) {
    return res.status(409).json({
      success: false,
      error: {
        code: 'DUPLICATE_EMAIL',
        message: 'An account with this email address already exists.'
      }
    });
  }
});
```


#### 410 Gone

| | |
|:--|:--|
| **Meaning** | The resource existed but has been **permanently deleted** |
| **Use when** | A resource was intentionally removed and will never return |
| **Difference from 404** | 404 = never existed or unknown; 410 = did exist, now gone |


#### 415 Unsupported Media Type

| | |
|:--|:--|
| **Meaning** | The server cannot process the request body's format |
| **Use when** | Client sends XML when only JSON is accepted, or sends text/plain when JSON is required |

```javascript
app.post('/api/v1/users', (req, res) => {
  const contentType = req.headers['content-type'];

  if (!contentType || !contentType.includes('application/json')) {
    return res.status(415).json({
      success: false,
      error: {
        code: 'UNSUPPORTED_MEDIA_TYPE',
        message: 'Content-Type must be application/json.'
      }
    });
  }
});
```


#### 422 Unprocessable Entity

| | |
|:--|:--|
| **Meaning** | The request body is valid JSON (not a 400), but the data fails **semantic validation** |
| **Use when** | `age: -5`, `email: "notanemail"`, `startDate > endDate` |
| **vs 400** | 400 = malformed/missing; 422 = valid format but logically invalid |

```javascript
app.post('/api/v1/events', (req, res) => {
  const { startDate, endDate } = req.body;

  if (new Date(startDate) >= new Date(endDate)) {
    return res.status(422).json({
      success: false,
      error: {
        code: 'VALIDATION_ERROR',
        message: 'startDate must be before endDate.',
        details: [{ field: 'endDate', message: 'Must be after startDate.' }]
      }
    });
  }
});
```


#### 429 Too Many Requests

| | |
|:--|:--|
| **Meaning** | The client has exceeded the rate limit |
| **Use when** | Rate limiting is triggered |
| **Best Practice** | Include `Retry-After` header telling the client when to retry |

```javascript
app.use((req, res) => {
  res.status(429)
     .set('Retry-After', '60')
     .json({
       success: false,
       error: {
         code: 'RATE_LIMIT_EXCEEDED',
         message: 'Too many requests. Please try again in 60 seconds.'
       }
     });
});
```


### 5xx — Server Errors

These mean the server made a mistake. The **request was valid** but the server failed to handle it.


#### 500 Internal Server Error

| | |
|:--|:--|
| **Meaning** | An unexpected error occurred on the server |
| **Use when** | Unhandled exceptions, uncaught errors, bugs |
| **CRITICAL** | Never expose stack traces or error details to the client in production |

```javascript
// Global error handler middleware (must have 4 params)
app.use((err, req, res, next) => {
  // Log full error internally
  console.error('[ERROR]', err.stack);

  // Return generic message to client
  res.status(500).json({
    success: false,
    error: {
      code: 'INTERNAL_SERVER_ERROR',
      message: 'An unexpected error occurred. Please try again later.'
    }
  });
});
```

> **Warning:** Never do `res.json({ error: err.stack })` in production. Exposing stack traces leaks your file paths, library versions, and server structure to attackers.


#### 501 Not Implemented

| | |
|:--|:--|
| **Meaning** | The server does not support the functionality required to fulfill the request |
| **Use when** | A feature is planned but not yet built |


#### 502 Bad Gateway

| | |
|:--|:--|
| **Meaning** | The server (acting as a gateway) received an invalid response from an upstream server |
| **Use when** | Your API calls a third-party service and that service returns garbage |
| **Real example** | Nginx returns 502 when the Node.js process is down |


#### 503 Service Unavailable

| | |
|:--|:--|
| **Meaning** | The server is temporarily unable to handle the request |
| **Use when** | Server is overloaded, under maintenance, or rate-limited by a dependency |
| **Best Practice** | Include `Retry-After` header |

```javascript
app.use('/api', (req, res, next) => {
  if (serverUnderMaintenance) {
    return res.status(503)
              .set('Retry-After', '3600')
              .json({
                success: false,
                error: {
                  code: 'SERVICE_UNAVAILABLE',
                  message: 'Scheduled maintenance. Back online at 02:00 UTC.'
                }
              });
  }
  next();
});
```


#### 504 Gateway Timeout

| | |
|:--|:--|
| **Meaning** | The gateway (proxy/load balancer) did not receive a timely response from the upstream server |
| **Use when** | A third-party API call or database query timed out |
| **Real example** | Your API calls another microservice and it doesn't respond within 30 seconds |

### Complete Status Code Reference Table

| Code | Name | Category | Common Use |
|:-----|:-----|:---------|:-----------|
| 200 | OK | Success | GET, PUT, PATCH success |
| 201 | Created | Success | POST creates resource |
| 202 | Accepted | Success | Async operation queued |
| 204 | No Content | Success | DELETE success, no body |
| 301 | Moved Permanently | Redirect | Permanent URL change |
| 302 | Found | Redirect | Temporary redirect |
| 304 | Not Modified | Redirect | Cache is valid |
| 400 | Bad Request | Client Error | Missing/malformed fields |
| 401 | Unauthorized | Client Error | No/invalid authentication |
| 403 | Forbidden | Client Error | Authenticated, no permission |
| 404 | Not Found | Client Error | Resource doesn't exist |
| 405 | Method Not Allowed | Client Error | Wrong HTTP method |
| 409 | Conflict | Client Error | Duplicate, state conflict |
| 410 | Gone | Client Error | Permanently deleted |
| 415 | Unsupported Media Type | Client Error | Wrong Content-Type |
| 422 | Unprocessable Entity | Client Error | Validation failed |
| 429 | Too Many Requests | Client Error | Rate limit exceeded |
| 500 | Internal Server Error | Server Error | Unhandled exception |
| 501 | Not Implemented | Server Error | Feature not built yet |
| 502 | Bad Gateway | Server Error | Upstream service failure |
| 503 | Service Unavailable | Server Error | Server overloaded/maintenance |
| 504 | Gateway Timeout | Server Error | Upstream timeout |



# CHAPTER 14: Validation

**Validation** is the process of checking that data sent by the client is correct, complete, and safe before processing it. Never trust client input.

> **Rule:** Validate everything. Assume every request is malicious until proven otherwise.

### Types of Validation

| Type | What to Check | Example |
|:-----|:-------------|:--------|
| **Required** | Field must be present | `name` is required |
| **Type** | Must be the correct data type | `age` must be a number |
| **Length** | String length within bounds | `password` must be 8–128 chars |
| **Format** | Must match a pattern | `email` must be valid email format |
| **Range** | Number within bounds | `price` must be between 0 and 99999 |
| **Enum** | Value must be from a list | `role` must be `'admin'` or `'user'` |
| **Business** | Domain-specific rules | `startDate` must be in the future |

### Manual Validation Example

```javascript
app.post('/api/v1/users', (req, res) => {
  const { name, email, age, role } = req.body;
  const errors = [];

  // Required checks
  if (!name || typeof name !== 'string') {
    errors.push({ field: 'name', message: 'Name is required and must be a string.' });
  } else if (name.trim().length < 2 || name.trim().length > 100) {
    errors.push({ field: 'name', message: 'Name must be between 2 and 100 characters.' });
  }

  // Email format
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!email || !emailRegex.test(email)) {
    errors.push({ field: 'email', message: 'A valid email address is required.' });
  }

  // Age range
  if (age !== undefined && (typeof age !== 'number' || age < 0 || age > 150)) {
    errors.push({ field: 'age', message: 'Age must be a number between 0 and 150.' });
  }

  // Enum validation
  const validRoles = ['admin', 'editor', 'user'];
  if (role && !validRoles.includes(role)) {
    errors.push({ field: 'role', message: `Role must be one of: ${validRoles.join(', ')}.` });
  }

  if (errors.length > 0) {
    return res.status(422).json({
      success: false,
      error: {
        code: 'VALIDATION_ERROR',
        message: 'Input validation failed.',
        details: errors
      }
    });
  }

  // All good — process the request
  res.status(201).json({ success: true, data: { name, email } });
});
```

### Using a Validation Library (Joi)

In production, use a dedicated validation library instead of writing it all manually:

```javascript
const Joi = require('joi');

const userSchema = Joi.object({
  name:     Joi.string().min(2).max(100).required(),
  email:    Joi.string().email().required(),
  age:      Joi.number().integer().min(0).max(150).optional(),
  password: Joi.string().min(8).max(128).required(),
  role:     Joi.string().valid('admin', 'editor', 'user').default('user')
});

app.post('/api/v1/users', (req, res) => {
  const { error, value } = userSchema.validate(req.body, { abortEarly: false });

  if (error) {
    const details = error.details.map(d => ({
      field: d.path.join('.'),
      message: d.message
    }));

    return res.status(422).json({
      success: false,
      error: { code: 'VALIDATION_ERROR', message: 'Validation failed.', details }
    });
  }

  // value is the validated and sanitised data
  res.status(201).json({ success: true, data: value });
});
```


# CHAPTER 15: Error Handling

A well-designed API returns **consistent, actionable error responses**. Poor error handling is one of the most common API design mistakes.

### Standard Error Response Structure

```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "User with ID 123 was not found.",
    "details": []
  }
}
```

| Field | Purpose |
|:------|:--------|
| `success` | Always `false` for errors |
| `error.code` | A machine-readable string for programmatic handling |
| `error.message` | A human-readable description |
| `error.details` | Array of field-level errors (useful for validation) |

### Custom Error Class

```javascript
class AppError extends Error {
  constructor(message, statusCode, code) {
    super(message);
    this.statusCode = statusCode;
    this.code = code;
    this.isOperational = true; // Expected errors vs programming bugs
  }
}

// Usage
throw new AppError('User not found.', 404, 'NOT_FOUND');
```

### Global Error Handler (Express)

```javascript
// Place this AFTER all route definitions
app.use((err, req, res, next) => {
  // Log full error for debugging
  console.error(`[${new Date().toISOString()}] ERROR:`, {
    message: err.message,
    stack:   err.stack,
    path:    req.path,
    method:  req.method
  });

  // Operational errors: safe to send to client
  if (err.isOperational) {
    return res.status(err.statusCode).json({
      success: false,
      error: {
        code: err.code,
        message: err.message
      }
    });
  }

  // Programming errors: generic message to client
  res.status(500).json({
    success: false,
    error: {
      code: 'INTERNAL_SERVER_ERROR',
      message: 'An unexpected error occurred. Please try again later.'
    }
  });
});
```

### Async Error Handling

Errors inside `async` functions must be caught and passed to `next()`:

```javascript
// Wrapper to catch async errors automatically
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Clean route handler — no try/catch needed
app.get('/api/v1/users', asyncHandler(async (req, res) => {
  const users = await db.query('SELECT * FROM users'); // Could throw
  res.status(200).json({ success: true, data: users });
}));
```


# CHAPTER 16: Authentication Overview

**Authentication** answers the question: **"Who are you?"**

> Note: This chapter gives an overview. Deep implementation (hashing passwords, JWT signing, OAuth flows) is covered in Module 07 — Security & Authentication.

### 1. API Keys

A static secret string passed in a header or query parameter.

```
GET /api/v1/weather?city=London
X-API-Key: sk_live_abc123xyz456
```

| Pros | Cons |
|:-----|:-----|
| Simple to implement | Hard to revoke one key without affecting others |
| Works for server-to-server | If leaked, attacker has full access |
| No expiry management | Not suitable for user authentication |

### 2. JWT (JSON Web Tokens)

A stateless token containing encoded claims about the user.

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**JWT Structure:**
```
header.payload.signature

{
  "alg": "HS256",    ← Algorithm
  "typ": "JWT"
}.{
  "userId": 123,      ← Claims
  "role": "admin",
  "iat": 1700000000,  ← Issued at
  "exp": 1700003600   ← Expires in 1 hour
}.SIGNATURE
```

```
┌──────────────────────────────────────┐
│          JWT Authentication Flow      │
│                                      │
│ 1. POST /auth/login                  │
│    Body: { email, password }         │
│                 │                    │
│                 ▼                    │
│ 2. Server verifies credentials       │
│    Server creates JWT token          │
│                 │                    │
│                 ▼                    │
│ 3. Server returns: { token: "..." }  │
│                 │                    │
│                 ▼                    │
│ 4. Client stores token (memory/LS)   │
│                 │                    │
│                 ▼                    │
│ 5. Client sends token on every req   │
│    Authorization: Bearer <token>     │
│                 │                    │
│                 ▼                    │
│ 6. Server verifies token signature   │
│    Extracts userId, role from payload│
│    Proceeds with request             │
└──────────────────────────────────────┘
```

> **Warning:** Never store sensitive data (passwords, credit card numbers) in JWT payload. The payload is Base64-encoded — not encrypted. Anyone can decode it.

### 3. Session + Cookies

Traditional server-side authentication:
- User logs in → server creates a session in the database/Redis
- Server sends a `Set-Cookie: sessionId=abc123` header
- Browser automatically sends the cookie on every subsequent request
- Server looks up session data to identify the user

| Pros | Cons |
|:-----|:-----|
| Easy to revoke (delete session) | Requires server-side session storage |
| Secure with HttpOnly cookies | Harder to scale horizontally |
| Native browser support | Cross-origin (CORS) complications |

### 4. OAuth 2.0

A framework for **delegated authorization** — "Login with Google/GitHub/Facebook".

```
1. Your app redirects user to Google's login page
2. User logs in with Google credentials
3. Google redirects back to your app with an authorization code
4. Your app exchanges the code for an access token
5. Your app uses the access token to get user info from Google
```

| OAuth Flow | Use Case |
|:-----------|:---------|
| Authorization Code | Web apps (most secure) |
| Authorization Code + PKCE | SPAs and mobile apps |
| Client Credentials | Server-to-server (no user) |
| Device Code | Smart TVs, CLI tools |

### 5. Bearer Token

A general pattern where an opaque or structured token is passed in the `Authorization` header:

```
Authorization: Bearer <token>
```

JWT tokens are the most common type of Bearer token.


# CHAPTER 17: Security Best Practices

### 1. Always Use HTTPS

```javascript
// Redirect HTTP to HTTPS (in production)
app.use((req, res, next) => {
  if (req.protocol !== 'https' && process.env.NODE_ENV === 'production') {
    return res.redirect(301, `https://${req.headers.host}${req.url}`);
  }
  next();
});
```

### 2. Use Helmet for Security Headers

```javascript
const helmet = require('helmet');
app.use(helmet()); // Sets X-Frame-Options, X-Content-Type-Options, HSTS, etc.
```

### 3. Configure CORS Properly

```javascript
const cors = require('cors');

app.use(cors({
  origin: ['https://yourfrontend.com'],  // ← Never use '*' in production
  methods: ['GET', 'POST', 'PUT', 'PATCH', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization']
}));
```

### 4. Rate Limiting

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100,                  // Max 100 requests per window
  message: {
    success: false,
    error: { code: 'RATE_LIMIT_EXCEEDED', message: 'Too many requests.' }
  }
});

app.use('/api/', limiter);
```

### 5. Prevent SQL Injection

```javascript
// ❌ NEVER do this — vulnerable to SQL injection
const query = `SELECT * FROM users WHERE email = '${req.body.email}'`;

// ✅ Always use parameterized queries
const query = 'SELECT * FROM users WHERE email = $1';
const result = await db.query(query, [req.body.email]);
```

### 6. Keep Secrets in Environment Variables

```javascript
// .env file (never commit to git)
JWT_SECRET=super_long_random_secret_here
DB_URL=postgresql://user:password@localhost/mydb

// Access in code
const secret = process.env.JWT_SECRET;
```

### Security Checklist

| Check | Status |
|:------|:-------|
| HTTPS enforced in production | ✅ |
| Helmet middleware enabled | ✅ |
| CORS origin whitelist (no `*`) | ✅ |
| Rate limiting on all routes | ✅ |
| Parameterized database queries | ✅ |
| Input validation on all endpoints | ✅ |
| Secrets in environment variables | ✅ |
| No stack traces in responses | ✅ |
| Auth on all protected routes | ✅ |
| Logs stored securely | ✅ |


# CHAPTER 18: API Documentation

> A great API with poor documentation is a bad API.

### Why Documentation Matters

- Developers can't use your API without knowing what it expects
- Good docs reduce support questions by 80%
- Auto-generated docs stay in sync with the code

### OpenAPI / Swagger

The industry standard for documenting REST APIs. Written in JSON or YAML.

```yaml
openapi: 3.0.0
info:
  title: User API
  version: 1.0.0

paths:
  /users:
    get:
      summary: Get all users
      parameters:
        - name: role
          in: query
          required: false
          schema:
            type: string
      responses:
        '200':
          description: List of users
          content:
            application/json:
              example:
                success: true
                data:
                  - id: 1
                    name: Alice
    post:
      summary: Create a user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required: [name, email]
              properties:
                name:
                  type: string
                email:
                  type: string
                  format: email
      responses:
        '201':
          description: User created
        '422':
          description: Validation error
```

### Swagger UI in Express

```javascript
const swaggerUi = require('swagger-ui-express');
const swaggerDocument = require('./swagger.json');

// Interactive docs at http://localhost:3000/api-docs
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerDocument));
```

### What Good API Docs Include

| Section | What to Include |
|:--------|:----------------|
| **Getting Started** | Base URL, auth method, rate limits |
| **Endpoints** | Method, URL, description |
| **Parameters** | Name, type, required/optional, example |
| **Request Body** | Schema + example |
| **Responses** | All status codes + example responses |
| **Errors** | Error codes and their meanings |
| **Code Examples** | curl, JavaScript, Python |


# CHAPTER 19: Complete Express CRUD Example

Let's build a full **User API** from scratch with all CRUD operations, validation, and error handling.

```javascript
// server.js — Complete User API

const express = require('express');
const app = express();

// ─── Middleware ───────────────────────────────────────────────────────────────
app.use(express.json()); // Parse JSON request bodies

// Request logger
app.use((req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.path}`);
  next();
});

// ─── In-memory data store (replace with DB in production) ────────────────────
let users = [
  { id: 1, name: 'Alice',   email: 'alice@example.com',   role: 'admin' },
  { id: 2, name: 'Bob',     email: 'bob@example.com',     role: 'user'  },
  { id: 3, name: 'Charlie', email: 'charlie@example.com', role: 'user'  },
];
let nextId = 4;

// ─── Helper: find user or 404 ─────────────────────────────────────────────────
const findUser = (id, res) => {
  const user = users.find(u => u.id === id);
  if (!user) {
    res.status(404).json({
      success: false,
      error: { code: 'NOT_FOUND', message: `User ${id} not found.` }
    });
    return null;
  }
  return user;
};

// ─── Routes ───────────────────────────────────────────────────────────────────

// GET /api/v1/users — List all users (with optional filtering)
app.get('/api/v1/users', (req, res) => {
  const { role, search, page = '1', limit = '10' } = req.query;

  let result = [...users];

  // Filter by role
  if (role) {
    result = result.filter(u => u.role === role);
  }

  // Search by name or email
  if (search) {
    const q = search.toLowerCase();
    result = result.filter(
      u => u.name.toLowerCase().includes(q) || u.email.toLowerCase().includes(q)
    );
  }

  // Pagination
  const pageNum  = parseInt(page);
  const limitNum = parseInt(limit);
  const start    = (pageNum - 1) * limitNum;
  const paginated = result.slice(start, start + limitNum);

  res.status(200).json({
    success: true,
    data: paginated,
    meta: {
      total:      result.length,
      page:       pageNum,
      limit:      limitNum,
      totalPages: Math.ceil(result.length / limitNum)
    }
  });
});

// GET /api/v1/users/:id — Get single user
app.get('/api/v1/users/:id', (req, res) => {
  const id   = parseInt(req.params.id);
  const user = findUser(id, res);
  if (!user) return;

  res.status(200).json({ success: true, data: user });
});

// POST /api/v1/users — Create a user
app.post('/api/v1/users', (req, res) => {
  const { name, email, role = 'user' } = req.body;
  const errors = [];

  // Validate
  if (!name || typeof name !== 'string' || name.trim().length < 2) {
    errors.push({ field: 'name', message: 'Name must be at least 2 characters.' });
  }
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!email || !emailRegex.test(email)) {
    errors.push({ field: 'email', message: 'A valid email is required.' });
  }
  if (!['admin', 'editor', 'user'].includes(role)) {
    errors.push({ field: 'role', message: 'Role must be admin, editor, or user.' });
  }

  if (errors.length) {
    return res.status(422).json({
      success: false,
      error: { code: 'VALIDATION_ERROR', message: 'Validation failed.', details: errors }
    });
  }

  // Check for duplicate email
  if (users.some(u => u.email === email)) {
    return res.status(409).json({
      success: false,
      error: { code: 'DUPLICATE_EMAIL', message: 'This email is already registered.' }
    });
  }

  const newUser = { id: nextId++, name: name.trim(), email, role };
  users.push(newUser);

  res.status(201)
     .set('Location', `/api/v1/users/${newUser.id}`)
     .json({ success: true, data: newUser, message: 'User created successfully.' });
});

// PUT /api/v1/users/:id — Replace entire user
app.put('/api/v1/users/:id', (req, res) => {
  const id    = parseInt(req.params.id);
  const index = users.findIndex(u => u.id === id);

  if (index === -1) {
    return res.status(404).json({
      success: false,
      error: { code: 'NOT_FOUND', message: `User ${id} not found.` }
    });
  }

  const { name, email, role } = req.body;

  // PUT requires ALL fields
  if (!name || !email || !role) {
    return res.status(400).json({
      success: false,
      error: {
        code: 'MISSING_FIELDS',
        message: 'PUT requires name, email, and role.'
      }
    });
  }

  users[index] = { id, name, email, role };
  res.status(200).json({ success: true, data: users[index] });
});

// PATCH /api/v1/users/:id — Partial update
app.patch('/api/v1/users/:id', (req, res) => {
  const id   = parseInt(req.params.id);
  const user = findUser(id, res);
  if (!user) return;

  // Only allow specific fields to be updated
  const allowedFields = ['name', 'email', 'role'];
  const updates = {};

  allowedFields.forEach(field => {
    if (req.body[field] !== undefined) {
      updates[field] = req.body[field];
    }
  });

  if (Object.keys(updates).length === 0) {
    return res.status(400).json({
      success: false,
      error: {
        code: 'NO_VALID_FIELDS',
        message: `Provide at least one of: ${allowedFields.join(', ')}.`
      }
    });
  }

  // Merge updates into existing user
  const index = users.findIndex(u => u.id === id);
  users[index] = { ...users[index], ...updates };

  res.status(200).json({ success: true, data: users[index] });
});

// DELETE /api/v1/users/:id — Delete a user
app.delete('/api/v1/users/:id', (req, res) => {
  const id    = parseInt(req.params.id);
  const index = users.findIndex(u => u.id === id);

  if (index === -1) {
    return res.status(404).json({
      success: false,
      error: { code: 'NOT_FOUND', message: `User ${id} not found.` }
    });
  }

  users.splice(index, 1);
  res.status(204).send(); // No Content
});

// ─── 404 Catch-All ────────────────────────────────────────────────────────────
app.use((req, res) => {
  res.status(404).json({
    success: false,
    error: {
      code: 'ROUTE_NOT_FOUND',
      message: `${req.method} ${req.path} is not a valid endpoint.`
    }
  });
});

// ─── Global Error Handler ─────────────────────────────────────────────────────
app.use((err, req, res, next) => {
  console.error('[UNHANDLED ERROR]', err.stack);
  res.status(500).json({
    success: false,
    error: {
      code: 'INTERNAL_SERVER_ERROR',
      message: 'Something went wrong on the server. Please try again later.'
    }
  });
});

// ─── Start Server ─────────────────────────────────────────────────────────────
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`User API running at http://localhost:${PORT}`);
  console.log('Endpoints:');
  console.log('  GET    /api/v1/users');
  console.log('  GET    /api/v1/users/:id');
  console.log('  POST   /api/v1/users');
  console.log('  PUT    /api/v1/users/:id');
  console.log('  PATCH  /api/v1/users/:id');
  console.log('  DELETE /api/v1/users/:id');
});
```

### Testing with cURL

```bash
# Get all users
curl http://localhost:3000/api/v1/users

# Get user 1
curl http://localhost:3000/api/v1/users/1

# Create a user
curl -X POST http://localhost:3000/api/v1/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Tushar","email":"tushar@example.com","role":"user"}'

# Update name only (PATCH)
curl -X PATCH http://localhost:3000/api/v1/users/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice Smith"}'

# Delete user 3
curl -X DELETE http://localhost:3000/api/v1/users/3
```



# CHAPTER 20: Best Practices

1. **Use correct HTTP methods** — GET for reading, POST for creating, PUT/PATCH for updating, DELETE for deleting.
2. **Use plural nouns in URIs** — `/users` not `/user`.
3. **Never put verbs in URLs** — `/users` not `/getUsers`.
4. **Always version your API** — `/api/v1/users` from day one.
5. **Return consistent response envelopes** — always `{ success, data, error }`.
6. **Use the right status codes** — 201 for created, 204 for delete, 422 for validation.
7. **Never expose stack traces** in production responses.
8. **Validate all input** — never trust the client.
9. **Paginate all list endpoints** — never return unlimited records.
10. **Use HTTPS everywhere** — no exceptions in production.
11. **Implement rate limiting** — on every public endpoint.
12. **Configure CORS properly** — never use `*` in production.
13. **Use environment variables** for secrets — never hardcode.
14. **Use a global error handler** in Express — centralize all error responses.
15. **Return field-level validation errors** — not just "validation failed".
16. **Use UUIDs instead of auto-increment IDs** for public-facing resources.
17. **Use ISO 8601 dates** — `2025-07-21T08:00:00Z` not timestamps or local formats.
18. **Return `[]` for empty collections** — never `null` for a list field.
19. **Return the created resource** in the POST response body.
20. **Include a `Location` header** in 201 responses pointing to the new resource.
21. **Use idempotency keys** for payment and mutation endpoints.
22. **Log every request** — method, path, status, latency.
23. **Expose a health check endpoint** — `GET /health`.
24. **Use helmet** for security headers.
25. **Sanitise output** — strip password hashes and internal fields before sending.
26. **Keep endpoints stateless** — no server-side session dependency.
27. **Use descriptive error codes** — `DUPLICATE_EMAIL` not just `ERROR`.
28. **Document every endpoint** — use Swagger/OpenAPI.
29. **Use compression** — gzip/Brotli for large responses.
30. **Test every endpoint** — happy path, validation, auth, and 404 cases.



# CHAPTER 21: Common Mistakes

| # | Mistake | Why It's Wrong | Fix |
|:--|:--------|:---------------|:----|
| 1 | Returning `200 OK` for errors | Clients can't distinguish success from failure | Use correct 4xx/5xx codes |
| 2 | Verbs in URLs (`/getUsers`) | Violates REST — method already implies action | Use nouns: `/users` |
| 3 | No API versioning | Breaking changes break all clients immediately | Add `/v1/` from the start |
| 4 | Exposing stack traces | Leaks file paths, library versions to attackers | Log internally; return generic message |
| 5 | No input validation | SQL injection, data corruption, crashes | Validate every field |
| 6 | Returning `null` for empty lists | Forces client to handle both `null` and `[]` | Always return `[]` |
| 7 | Using `GET` to modify data | GET should be safe and idempotent | Use POST/PUT/PATCH/DELETE |
| 8 | Inconsistent response format | Clients need different parsing logic per endpoint | Standardize the envelope |
| 9 | No pagination | Server OOM, slow responses for large datasets | Always paginate collections |
| 10 | Storing secrets in code | Credentials leak in git history | Use `.env` + `.gitignore` |
| 11 | Using `403` for unauthenticated | Wrong semantic — 403 means known user, no permission | Use `401` for unauthenticated |
| 12 | Deeply nested URLs | Hard to read and maintain | Max 2 levels of nesting |
| 13 | No error codes (just messages) | Clients can't handle errors programmatically | Add machine-readable `code` field |
| 14 | Returning passwords/hashes | Massive security vulnerability | Strip sensitive fields before response |
| 15 | No rate limiting | API is vulnerable to brute force and DoS | Implement per-IP rate limiting |
| 16 | Cors `origin: '*'` in production | Any website can call your API | Whitelist specific origins |
| 17 | Mutable IDs in URLs | Auto-increment IDs are guessable | Use UUIDs |
| 18 | No `Content-Type` header | Server can't parse the body | Always send `Content-Type: application/json` |
| 19 | Conflating 400 and 422 | Different semantics | 400 = malformed; 422 = logically invalid |
| 20 | No health endpoint | Can't monitor service status | Expose `GET /health` |
| 21 | Swallowing errors silently | Bugs become invisible | Always log caught errors |
| 22 | Using `PUT` when `PATCH` is needed | Accidentally deletes fields | Use PATCH for partial updates |
| 23 | Not returning the created resource | Client must make another GET | Include new object in POST 201 response |
| 24 | String IDs where numbers expected | Type mismatch bugs | Parse `req.params.id` with `parseInt()` |
| 25 | No logging | Impossible to debug production issues | Log every request with method, path, status |


# CHAPTER 22: Interview Questions

### Beginner Level

**Q1. What is an API?**
An API (Application Programming Interface) is a set of rules that defines how two software systems communicate. In web development, it's a server that exposes endpoints over HTTP for clients to interact with data.

**Q2. What is REST?**
REST (Representational State Transfer) is an architectural style for designing networked APIs. It defines six constraints: Client-Server, Statelessness, Cacheability, Uniform Interface, Layered System, and Code on Demand.

**Q3. What is the difference between GET and POST?**
GET retrieves data and should not change server state (safe + idempotent). POST creates a new resource or triggers an action and modifies server state (not safe, not idempotent).

**Q4. What does stateless mean in REST?**
Each HTTP request must contain all information needed to process it. The server stores no session state between requests. Authentication is typically achieved with tokens (JWT) sent on every request.

**Q5. What is a REST endpoint?**
A specific URL that maps to a resource and HTTP method combination. Example: `GET /api/v1/users` is an endpoint that retrieves a list of users.

**Q6. What HTTP status code is returned when a resource is successfully created?**
`201 Created`.

**Q7. What HTTP status code is returned for a successful GET request?**
`200 OK`.

**Q8. What HTTP status code is returned when a resource is not found?**
`404 Not Found`.

**Q9. What is the difference between 401 and 403?**
- `401 Unauthorized`: The request lacks valid authentication credentials. "Who are you?"
- `403 Forbidden`: The client is authenticated but lacks permission. "I know who you are, but you can't do this."

**Q10. What is JSON? Why is it used in APIs?**
JSON (JavaScript Object Notation) is a lightweight, human-readable data format. It's used in APIs because it's language-agnostic, easy to parse, and maps naturally to objects in most languages.

**Q11. What is the difference between PUT and PATCH?**
PUT replaces the entire resource — missing fields are deleted. PATCH partially updates a resource — only the provided fields are changed.

**Q12. What status code does DELETE typically return?**
`204 No Content` — success with no response body.

**Q13. What is a path parameter vs a query parameter?**
- Path parameter identifies a specific resource: `/users/123`
- Query parameter filters or modifies a collection: `/users?role=admin`

**Q14. What does `req.body`, `req.params`, and `req.query` contain in Express?**
- `req.body`: Parsed JSON request body (POST/PUT/PATCH)
- `req.params`: Route parameters (`:id`)
- `req.query`: Query string parameters (`?page=1`)

**Q15. What is CORS?**
Cross-Origin Resource Sharing — a browser security mechanism that restricts web pages from making API calls to a different domain than the one that served the page. Servers must explicitly allow cross-origin requests.


### Intermediate Level

**Q16. Explain the Richardson Maturity Model.**
A scale measuring REST compliance:
- Level 0: Single endpoint, all actions via POST
- Level 1: Separate URIs per resource
- Level 2: HTTP verbs used correctly with status codes
- Level 3: HATEOAS — responses include hypermedia links. Most APIs target Level 2.

**Q17. When should you use 422 instead of 400?**
400 = malformed or unparseable request (missing JSON braces, wrong Content-Type). 422 = the request is valid JSON but the data is semantically invalid (e.g., email format is wrong, start date is after end date).

**Q18. What is idempotency? Which HTTP methods are idempotent?**
An operation is idempotent if calling it multiple times produces the same result as calling it once. GET, PUT, DELETE, HEAD, OPTIONS are idempotent. POST is not (calling POST twice creates two resources).

**Q19. What is an API key? How is it different from a JWT?**
An API key is a static opaque secret — no expiry, no claims. A JWT is a signed token containing claims (user ID, role, expiry) — stateless and self-contained. JWTs expire; API keys typically don't unless rotated.

**Q20. What is the purpose of the `Authorization` header?**
It carries authentication credentials for the request. Common formats:
- `Authorization: Bearer <jwt_token>`
- `Authorization: ApiKey <api_key>`
- `Authorization: Basic <base64(username:password)>`

**Q21. Why should APIs be versioned? What is the best strategy?**
Versioning allows making breaking changes without disrupting existing clients. URI versioning (`/v1/`, `/v2/`) is the most common and easiest to route, test, and document.

**Q22. What is rate limiting? How is 429 used?**
Rate limiting restricts the number of requests a client can make in a time window. When exceeded, the server returns `429 Too Many Requests` with a `Retry-After` header indicating when the client may retry.

**Q23. What is the difference between authentication and authorization?**
- Authentication: Verifying *who* you are (login, JWT verification)
- Authorization: Verifying *what* you're allowed to do (role checks, permission checks)

**Q24. What is a JWT? What are its three parts?**
JSON Web Token — a compact, self-contained token for securely transmitting claims.
- Header: Algorithm and token type
- Payload: Claims (userId, role, iat, exp)
- Signature: HMAC/RSA signature to verify integrity

**Q25. What is the difference between offset and cursor pagination?**
Offset: `page=3&limit=20` — uses SQL OFFSET; simple but slow on large tables. Cursor: `cursor=lastId` — uses `WHERE id > lastId`; consistent performance regardless of dataset size.

**Q26. How does HTTPS protect API traffic?**
HTTPS uses TLS to encrypt data in transit. Without it, tokens, passwords, and API responses can be intercepted (man-in-the-middle attack). The server proves its identity with a certificate.

**Q27. What is the purpose of middleware in Express?**
Middleware is a function that runs between the request and the route handler. Common uses: logging, authentication, CORS, rate limiting, body parsing, error handling.

**Q28. What is a global error handler in Express? How do you define one?**
A middleware with 4 parameters `(err, req, res, next)` placed after all routes. Express calls it automatically when `next(err)` is called or an error is thrown in a handler.

**Q29. Why should you not expose stack traces in production?**
Stack traces reveal file paths, function names, library versions, and code structure — valuable information for attackers to exploit vulnerabilities.

**Q30. What is the `Content-Type` header used for?**
It tells the server (on request) or the client (on response) what format the body is in. For JSON APIs: `Content-Type: application/json`.


### Advanced Level

**Q31. What is HATEOAS?**
Hypermedia as the Engine of Application State — Level 3 REST. Responses include links to related actions, allowing clients to discover the API dynamically without hardcoding URLs.

**Q32. What is the difference between REST and GraphQL?**

| | REST | GraphQL |
|:-|:-----|:--------|
| Endpoints | Multiple | Single (`/graphql`) |
| Data shape | Server-defined | Client-defined |
| Over-fetching | Common | Eliminated |
| Caching | Native HTTP | Requires specialized tools |

**Q33. What is OAuth 2.0? When would you use it?**
A delegated authorization framework. Use it when users need to grant your app permission to access their data from another service (Google, GitHub). The Authorization Code + PKCE flow is most secure for web/mobile apps.

**Q34. What is the idempotency key pattern?**
A unique key (UUID) sent by the client in a header (e.g., `Idempotency-Key: uuid-here`). The server caches the result of the first request. If the same key is sent again (retry), the server returns the cached result instead of processing again. Critical for payment APIs.

**Q35. What is an API Gateway?**
A single entry point for all API clients. It handles routing to microservices, authentication, rate limiting, logging, and SSL termination. Examples: AWS API Gateway, Kong, nginx.

**Q36. Explain the N+1 query problem.**
When fetching a list of N items and then making one query per item for related data (N additional queries). Solution: use JOINs, eager loading (Sequelize `include`), or DataLoaders (GraphQL).

**Q37. What are the security implications of using `*` for CORS origin?**
It allows any website to call your API from a browser, enabling CSRF-like attacks, unauthorized data access, and API abuse. Always whitelist specific origins in production.

**Q38. What is the difference between 502 and 503?**
- 502 Bad Gateway: The server received an invalid response from an upstream service.
- 503 Service Unavailable: The server is overloaded or under maintenance and can't handle the request.

**Q39. How would you design a file upload endpoint?**
Use `multipart/form-data` (not JSON). Validate MIME type (not just extension). Store files in object storage (S3). Save the file URL in the database. Use a resource-specific path: `POST /users/:id/avatar`.

**Q40. What is the difference between horizontal and vertical scaling, and how does statelessness help?**
- Vertical scaling: Adding more resources to one server (bigger machine)
- Horizontal scaling: Adding more servers (load balancing)
Stateless APIs can be horizontally scaled easily because any server can handle any request — no session affinity required.

**Q41. What is the ETag header? How does it reduce bandwidth?**
An ETag is a hash of the current resource state. The client caches it. On subsequent requests, the client sends `If-None-Match: <etag>`. If the resource hasn't changed, the server returns 304 (no body) — saving bandwidth.

**Q42. How do you handle breaking vs non-breaking changes in an API?**
Non-breaking (additive): Adding new optional fields, new endpoints — no version bump needed. Breaking: Removing/renaming fields, changing behavior — requires a new version (`/v2/`). Always deprecate old versions with a migration timeline.

**Q43. What is the purpose of the `Retry-After` header?**
Tells the client how long to wait before retrying. Used with 429 (rate limit) and 503 (service unavailable). Value is seconds or an HTTP date.

**Q44. What are some strategies to prevent SQL injection in a Node.js API?**
1. Use parameterized queries / prepared statements (never string concatenation)
2. Use an ORM (Prisma, Sequelize) which handles parameterization automatically
3. Validate and sanitize all user input
4. Use least-privilege database credentials

**Q45. How would you implement soft delete in a REST API?**
Add a `deletedAt` timestamp field. Instead of `DELETE FROM users WHERE id = ?`, do `UPDATE users SET deletedAt = NOW() WHERE id = ?`. Return 204 to the client. Filter out soft-deleted records from GET queries. This preserves data integrity and audit history.


# CHAPTER 23: Practice Projects

| # | Project | Difficulty | Skills | Est. Time |
|:--|:--------|:----------:|:-------|:---------:|
| 1 | User Registration & Login API | ⭐ | POST, validation, 201/409 | 2 hrs |
| 2 | Todo List API (full CRUD) | ⭐ | All HTTP methods, 204 | 2 hrs |
| 3 | Blog Posts API | ⭐ | CRUD, nested resources (`/posts/:id/comments`) | 3 hrs |
| 4 | Product Catalog API | ⭐⭐ | Filtering, sorting, pagination | 3 hrs |
| 5 | Bookmarks API | ⭐ | CRUD, search by query param | 2 hrs |
| 6 | Notes App API | ⭐ | Tags, filtering, soft delete | 3 hrs |
| 7 | Weather Proxy API | ⭐⭐ | Third-party API integration, 502 handling | 2 hrs |
| 8 | URL Shortener API | ⭐⭐ | 301 redirects, unique code generation | 3 hrs |
| 9 | Movie Review API | ⭐⭐ | Nested resources, avg rating calculation | 4 hrs |
| 10 | E-Commerce Cart API | ⭐⭐⭐ | Cart state, quantity updates, total | 5 hrs |
| 11 | File Upload API | ⭐⭐⭐ | Multipart, S3 (or local), 415 | 4 hrs |
| 12 | JWT Auth API | ⭐⭐⭐ | Login, refresh tokens, protected routes | 5 hrs |
| 13 | Rate-Limited Public API | ⭐⭐⭐ | Rate limiting, 429, headers | 3 hrs |
| 14 | Admin Dashboard API | ⭐⭐⭐ | RBAC, 403, role middleware | 5 hrs |
| 15 | Inventory Management API | ⭐⭐⭐ | Stock tracking, 409 conflict | 5 hrs |
| 16 | Expense Tracker API | ⭐⭐ | Categories, date filtering, totals | 4 hrs |
| 17 | Event Booking API | ⭐⭐⭐ | Seat availability, 409 overbooking | 5 hrs |
| 18 | Chat History API | ⭐⭐⭐ | Cursor pagination, real-time readiness | 4 hrs |
| 19 | Social Feed API | ⭐⭐⭐⭐ | Follow/unfollow, feed generation | 8 hrs |
| 20 | Multi-tenant SaaS API | ⭐⭐⭐⭐ | Org isolation, ABAC, API keys | 10 hrs |


# CHAPTER 24: Cheat Sheet

### HTTP Methods

| Method | Action | Body | Idempotent | Success Code |
|:-------|:-------|:----:|:----------:|:------------:|
| GET | Read | No | Yes | 200 |
| POST | Create | Yes | No | 201 |
| PUT | Replace | Yes | Yes | 200 |
| PATCH | Partial Update | Yes | Yes | 200 |
| DELETE | Delete | No | Yes | 204 |

### Status Codes Quick Reference

```
200 OK              → GET/PUT/PATCH success
201 Created         → POST created a resource
204 No Content      → DELETE success (no body)
400 Bad Request     → Malformed/missing data
401 Unauthorized    → Not authenticated
403 Forbidden       → Authenticated but no permission
404 Not Found       → Resource doesn't exist
409 Conflict        → Duplicate / state conflict
422 Unprocessable   → Valid JSON, invalid data
429 Too Many Req    → Rate limit exceeded
500 Server Error    → Unhandled server bug
503 Unavailable     → Server down/maintenance
```

### REST Naming Rules

```
✅ GET    /users              → list all
✅ GET    /users/:id          → get one
✅ POST   /users              → create
✅ PUT    /users/:id          → replace
✅ PATCH  /users/:id          → update fields
✅ DELETE /users/:id          → delete

✅ Plural nouns:  /users  /posts  /products
✅ Lowercase:     /blog-posts  /user-settings
✅ Versioned:     /api/v1/users
❌ No verbs:      /getUsers  /deletePost
❌ No nesting > 2 levels
```

### Standard Response Envelope

```json
// Success
{
  "success": true,
  "data": { ... },
  "message": "Optional message."
}

// Error
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human readable message.",
    "details": []
  }
}

// Paginated
{
  "success": true,
  "data": [...],
  "meta": { "total": 100, "page": 2, "limit": 10, "totalPages": 10 }
}
```

### CRUD → HTTP → SQL

```
Create → POST   → INSERT
Read   → GET    → SELECT
Update → PATCH  → UPDATE
Delete → DELETE → DELETE
```

### REST Principles (6 Constraints)

```
1. Client-Server      → Frontend & backend are independent
2. Stateless          → No session on server; token per request
3. Cacheable          → Responses declare cacheability
4. Uniform Interface  → Standard URIs + HTTP methods
5. Layered System     → Load balancers, CDNs are transparent
6. Code on Demand     → Optional: server sends executable code
```

### Express Quick Reference

```javascript
const express = require('express');
const app = express();
app.use(express.json());

app.get('/path',    (req, res) => { /* req.params, req.query */ });
app.post('/path',   (req, res) => { /* req.body */ });
app.put('/path/:id',(req, res) => { /* req.params.id */ });
app.patch('/path/:id',(req,res)=> { /* req.params.id, req.body */ });
app.delete('/path/:id',(req,res)=>{ /* req.params.id */ });

res.status(200).json({ ... });   // with body
res.status(204).send();          // no body

// Global error handler (4 params)
app.use((err, req, res, next) => {
  res.status(500).json({ success: false, error: { message: 'Server error.' } });
});

app.listen(3000);
```


# CHAPTER 25: Summary

| Chapter | Key Takeaways |
|:--------|:-------------|
| **1 – APIs** | APIs are contracts between systems. They enable integration, abstraction, and reusability. |
| **2 – Client-Server** | 3-tier architecture: Client → Server (API) → Database. Client never accesses DB directly. |
| **3 – HTTP Basics** | HTTP is stateless. Every request has method, URL, headers, and optional body. |
| **4 – Request Lifecycle** | Request travels through DNS → TCP → Middleware → Route Handler → DB → Response. |
| **5 – HTTP Methods** | GET=read, POST=create, PUT=replace, PATCH=update, DELETE=delete. Methods have safety/idempotency semantics. |
| **6 – REST** | 6 constraints: Stateless, Client-Server, Cacheable, Uniform Interface, Layered, Code on Demand. |
| **7 – Endpoints** | Use plural nouns, no verbs in URLs, lowercase, hyphens, max 2 nesting levels, versioned. |
| **8 – Route Params** | `:id` in path identifies a resource. Always string — parse with `parseInt()`. |
| **9 – Query Params** | After `?` — used for filtering, sorting, searching, pagination. |
| **10 – Request Body** | JSON payload for POST/PUT/PATCH. Requires `express.json()` middleware. |
| **11 – Response Body** | Consistent envelope: `{ success, data, error, meta }`. |
| **12 – CRUD** | Create→POST/201, Read→GET/200, Update→PATCH/200, Delete→DELETE/204. |
| **13 – Status Codes** | 2xx=success, 4xx=client error, 5xx=server error. Use the specific code, not just 200/500. |
| **14 – Validation** | Validate type, required, length, format, range. Return 422 with field-level errors. |
| **15 – Error Handling** | Global error handler, custom AppError class, never expose stack traces. |
| **16 – Authentication** | JWT=stateless token, Sessions=server-state, OAuth=delegated auth, API Keys=simple server-to-server. |
| **17 – Security** | HTTPS, Helmet, CORS whitelist, rate limiting, parameterized queries, env vars. |
| **18 – Documentation** | OpenAPI/Swagger is the industry standard. Document every endpoint, parameter, and response. |
| **19 – CRUD Example** | Full Express User API with GET, POST, PUT, PATCH, DELETE, validation, 404, global error handler. |
| **20 – Best Practices** | 30 rules covering methods, naming, responses, security, and monitoring. |
| **21 – Common Mistakes** | 200 for errors, verbs in URLs, no versioning, exposed stack traces, no pagination. |
| **22 – Interview Q&A** | 45 questions covering beginner → advanced: REST, HTTP, JWT, OAuth, rate limiting, scaling. |
| **23 – Projects** | 20 projects from Todo API to Multi-tenant SaaS API. |
| **24 – Cheat Sheet** | One-page reference: methods, status codes, REST rules, response format, Express syntax. |


## References

- [RFC 7231 — HTTP/1.1 Semantics](https://datatracker.ietf.org/doc/html/rfc7231)
- [RFC 7235 — HTTP/1.1 Authentication](https://datatracker.ietf.org/doc/html/rfc7235)
- [Roy Fielding's REST Dissertation (2000)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
- [OpenAPI Specification 3.0](https://spec.openapis.org/oas/v3.0.3)
- [Express.js Official Documentation](https://expressjs.com/)
- [MDN — HTTP Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- [JWT.io — JSON Web Tokens](https://jwt.io/)
- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)


> **Next Module →** [Node.js & Express Deep Dive](../07-nodejs-express/notes.md)
> **Previous Module ←** [HTTP, JSON & Fetch](../05-http-json-fetch/notes.md)
