# Full Stack Development — Fundamentals

> **Day 1 | Full Stack Development**
> Clean, structured notes for learning, revision, and interview preparation.

---

## Table of Contents

1. [What Happens When a User Interacts With a Website?](#1-what-happens-when-a-user-interacts-with-a-website)
2. [Full Stack Architecture Overview](#2-full-stack-architecture-overview)
3. [Components of Full Stack Architecture](#3-components-of-full-stack-architecture)
4. [Login Flow — Step by Step](#4-login-flow--step-by-step)
5. [What is a Server?](#5-what-is-a-server)
6. [Rate Limiting](#6-rate-limiting)
7. [Cache — The Speed Layer](#7-cache--the-speed-layer)
8. [Cookies](#8-cookies)
9. [Sessions](#9-sessions)
10. [Session-Based Authentication](#10-session-based-authentication)
11. [Complete Full Stack Request Flow](#11-complete-full-stack-request-flow)
12. [Real-World Example — Instagram Login](#12-real-world-example--instagram-login)
13. [Key Concepts Summary](#13-key-concepts-summary)
14. [Glossary](#14-glossary)
15. [Quick Revision](#15-quick-revision)

---

## 1. What Happens When a User Interacts With a Website?

When a user performs any action on a website — clicking a button, submitting a form, or searching for a product — a chain of events is triggered across multiple systems working in coordination. This coordinated system is called **full stack architecture**.

### Real-World Analogy — A Restaurant

Think of a restaurant to understand how the layers of a web app communicate:

```
+----------------------------+        +----------------------------+
|          YOU               |        |        FULL STACK          |
|     (The Customer)         |        |         EQUIVALENT         |
+----------------------------+        +----------------------------+
| You sit down               |  --->  | User opens browser         |
| You read the menu          |  --->  | Frontend renders the page  |
| Waiter takes your order    |  --->  | API receives the request   |
| Kitchen prepares the food  |  --->  | Backend processes logic    |
| Kitchen fetches ingredients|  --->  | Database returns data      |
| Waiter brings food to you  |  --->  | Response shown on screen   |
+----------------------------+        +----------------------------+
```

### Request Flow (User to System)

```
+-------------------+
|       User        |  <-- Types a URL or clicks a button
+-------------------+
          |
          v
+-------------------+
|  Browser (Client) |  <-- Interprets the action
+-------------------+
          |
          v
+-------------------+
| Frontend (UI)     |  <-- Builds and sends the API request
+-------------------+
          |
          v
+-------------------+
|    API Layer      |  <-- Routes the request to the right backend handler
+-------------------+
          |
          v
+-------------------+
| Backend (Logic)   |  <-- Validates, processes, and applies rules
+-------------------+
          |
          v
+-------------------+
|   Database        |  <-- Stores or retrieves the requested data
+-------------------+
```

### Response Flow (System back to User)

After the database responds, the data travels back up through every layer:

```
Database  -->  Backend  -->  API Layer  -->  Frontend  -->  Browser  -->  User sees result
```

---

## 2. Full Stack Architecture Overview

A full stack application is built in distinct layers, each with a well-defined responsibility. No layer does the job of another — they communicate through defined interfaces.

```
+--------------------------------------------------+
|                     User                         |
|         Person interacting with the app          |
+--------------------------------------------------+
                         |
                         v
+--------------------------------------------------+
|             Browser / Client                     |
|       Chrome, Firefox, Safari, Mobile App        |
+--------------------------------------------------+
                         |
                         v
+--------------------------------------------------+
|         Frontend  (Presentation Layer)           |
|            HTML, CSS, JavaScript, React          |
+--------------------------------------------------+
                         |
                         v
+--------------------------------------------------+
|            API Layer  (Communication)            |
|        REST API, GraphQL, HTTP Endpoints         |
+--------------------------------------------------+
                         |
                         v
+--------------------------------------------------+
|         Backend  (Application Logic Layer)       |
|         Node.js, Express, Django, FastAPI        |
+--------------------------------------------------+
                         |
                         v
+--------------------------------------------------+
|          Database  (Persistent Data Layer)       |
|         MongoDB, PostgreSQL, MySQL, Redis        |
+--------------------------------------------------+
```

### Layer Responsibilities at a Glance

| Layer        | Primary Responsibility                     | Real-Life Analogy        |
|--------------|--------------------------------------------|--------------------------|
| User         | Initiates all interactions                 | Customer at a restaurant |
| Browser      | Displays the app, runs JavaScript          | Menu and dining table    |
| Frontend     | Renders UI, captures user input            | Waiter                   |
| API          | Routes communication between layers        | Order slip               |
| Backend      | Executes business logic and rules          | Kitchen staff            |
| Database     | Persists and retrieves all application data| Storage room             |

---

## 3. Components of Full Stack Architecture

### 3.1  User

The user is the person who interacts with the application. Every component in the stack exists to serve the user efficiently and securely.

**Typical user actions include:**
- Registering an account or logging in
- Browsing and searching for content
- Submitting forms and placing orders
- Viewing personalised data (dashboards, feeds)

---

### 3.2  Browser / Client Layer

The browser is the software through which the user accesses the application. It is responsible for rendering the visual interface and executing JavaScript.

**Common browsers:**  Chrome, Mozilla Firefox, Safari, Microsoft Edge

**Browser responsibilities:**

| Responsibility                | Description                                      |
|-------------------------------|--------------------------------------------------|
| Render the page               | Parses HTML and CSS to display the UI            |
| Execute JavaScript            | Runs client-side logic (button clicks, etc.)     |
| Send HTTP requests to server  | Forwards login credentials, form data, etc.      |
| Store cookies                 | Persists session identifiers between requests    |
| Manage local storage          | Caches data for offline or faster access         |

---

### 3.3  Frontend (Presentation Layer)

The frontend is everything visible to the user — text, images, buttons, forms, animations. It communicates with the backend exclusively through the API layer.

**Core responsibilities:**
- Render content received from the backend
- Handle user input events (clicks, keypresses, form submissions)
- Send API requests when the user takes an action
- Update the UI dynamically based on server responses

**Technology overview:**

```
HTML        -->  Defines the page structure (headings, paragraphs, forms)
CSS         -->  Controls visual appearance (colors, layout, typography)
JavaScript  -->  Adds interactivity (event handlers, DOM manipulation)
React.js    -->  Component-based library for building complex UIs
Next.js     -->  React framework with server-side rendering (SSR)
Tailwind    -->  Utility-first CSS framework for rapid styling
```

**Example:** When a user clicks "Add to Cart", the frontend captures the event and sends a `POST /cart/add` request to the backend API.

---

### 3.4  API Layer (Communication Layer)

The API (Application Programming Interface) is the contract between the frontend and backend. It defines what requests can be made, what format data must follow, and what responses to expect.

```
Frontend                    API                    Backend
   |                         |                        |
   |  POST /login            |                        |
   |  { email, password }    |                        |
   |-----------------------> |                        |
   |                         |  Forward validated     |
   |                         |  request               |
   |                         |----------------------> |
   |                         |                        |
   |                         |  { success, token }    |
   |                         | <--------------------- |
   |  { success: true }      |                        |
   | <---------------------- |                        |
```

**Common REST API endpoint examples:**

```
POST   /login            -->  Authenticate a user with credentials
GET    /products         -->  Fetch all available products
POST   /cart/add         -->  Add an item to a user's shopping cart
DELETE /cart/item/:id    -->  Remove a specific item from the cart
GET    /user/profile     -->  Retrieve the logged-in user's profile
```

---

### 3.5  Backend (Application Logic Layer)

The backend is the central processing unit of the application. It receives API requests, validates data, applies business rules, coordinates with the database, and returns structured responses.

**Core responsibilities:**

| Task                       | Description                                          |
|----------------------------|------------------------------------------------------|
| Parse and validate requests| Ensure incoming data meets required format and rules |
| Authenticate users         | Verify identity using sessions, tokens, or cookies   |
| Authorize actions          | Check whether the user has permission to proceed     |
| Apply business logic       | E.g., "A user cannot order more than 10 of one item" |
| Query the database         | Read or write data as needed                         |
| Format and return response | Send structured data (usually JSON) back to frontend |

**Common backend technologies:**

```
Node.js + Express.js  -->  Most widely used JavaScript backend stack
Django                -->  Python framework; comes with built-in admin, auth, ORM
FastAPI               -->  Modern, fast Python API framework
Spring Boot           -->  Java-based; used heavily in enterprise systems
NestJS                -->  TypeScript backend with Angular-style architecture
```

---

### 3.6  Database (Persistent Data Layer)

The database is the permanent storage layer. All data that must survive beyond a single session — user accounts, orders, messages — is stored here.

**What a typical database stores:**
- User records (username, email, hashed password)
- Orders and transaction history
- Product catalogue (name, description, price, inventory)
- Application content (blog posts, comments, media metadata)

**Database options by type:**

| Database     | Type     | Best Suited For                                  |
|--------------|----------|--------------------------------------------------|
| MongoDB      | NoSQL    | Flexible, document-based data (JSON format)      |
| PostgreSQL   | SQL      | Complex relational data; high reliability        |
| MySQL        | SQL      | Web applications; very widely supported          |
| SQLite       | SQL      | Small or embedded apps; local development        |
| Redis        | NoSQL    | Caching, sessions, real-time data (in-memory)    |
| Firebase     | NoSQL    | Real-time apps; mobile-first backends            |

---

## 4. Login Flow — Step by Step

This section traces the complete journey of a login request from the moment a user clicks the login button to the moment they are granted access.

### High-Level Flow

```
+---------------------------------------------+
|  User types email + password, clicks Login  |
+---------------------------------------------+
                      |
                      v
+---------------------------------------------+
|  Frontend sends:  POST /login               |
|  Body: { email: "...", password: "..." }    |
+---------------------------------------------+
                      |
                      v
+---------------------------------------------+
|  Backend receives and validates the request |
|  - Is email format valid?                   |
|  - Are required fields present?             |
+---------------------------------------------+
                      |
                      v
+---------------------------------------------+
|  Backend queries Database                   |
|  - Does this email exist?                   |
|  - Does the password hash match?            |
+---------------------------------------------+
                      |
           +----------+----------+
           |                     |
     Credentials            Credentials
      INVALID                  VALID
           |                     |
           v                     v
+------------------+   +-------------------------+
| Return Error 401 |   | Create session on server|
| "Invalid login"  |   | Generate session ID     |
+------------------+   +-------------------------+
                                 |
                                 v
                    +----------------------------+
                    | Send Cookie to Browser     |
                    | Set-Cookie: sessionId=xyz  |
                    +----------------------------+
                                 |
                                 v
                    +----------------------------+
                    | Browser stores the cookie  |
                    | User is now logged in      |
                    +----------------------------+
```

### Step-by-Step Breakdown

| Step | Actor         | Action Taken                                               |
|------|---------------|------------------------------------------------------------|
| 1    | User          | Enters email and password, clicks the Login button         |
| 2    | Frontend      | Sends `POST /login` with credentials in the request body   |
| 3    | Backend       | Validates the format and presence of the fields            |
| 4    | Backend + DB  | Queries the database to verify credentials                 |
| 5    | Backend       | If valid, creates a session and stores user data server-side|
| 6    | Backend       | Sends a `Set-Cookie` header with the session ID            |
| 7    | Browser       | Stores the cookie locally                                  |
| 8    | Browser       | Automatically includes the cookie in all future requests   |
| 9    | Backend       | Reads the cookie, looks up the session, and grants access  |

---

## 5. What is a Server?

A server is a computer (physical machine or virtual instance) that continuously listens for incoming requests from clients (browsers, mobile apps) and sends back appropriate responses.

### Client-Server Communication

```
+-------------------+         HTTP Request          +-------------------+
|                   | ----------------------------> |                   |
|  Client (Browser) |                               |      Server       |
|                   | <---------------------------- |                   |
+-------------------+        HTTP Response          +-------------------+
```

**What a server handles:**

| Responsibility               | Example                                          |
|------------------------------|--------------------------------------------------|
| Listen for incoming requests | Waits for any client to send `POST /login`       |
| Run backend application code | Executes Node.js or Python logic                 |
| Communicate with the database| Runs a SQL query or MongoDB find operation       |
| Send a structured response   | Returns `{ success: true, user: { ... } }`       |

> A server is not a single machine — it can be a cluster of machines, a cloud function, or a containerized process. The concept remains the same: it receives requests and sends responses.

---

## 6. Rate Limiting

Rate limiting is a technique used to restrict the number of requests a client (user, IP address, or API key) can make within a defined time window.

### Purpose

Without rate limiting, a malicious actor or automated bot could send thousands of requests per second to a server, causing it to become unresponsive for legitimate users.

### Where Rate Limiting Is Applied

```
+-------------------+
|  Client (Browser) |
+-------------------+
          |
          | HTTP Request
          v
+-------------------+
|   API Gateway     |
+-------------------+
          |
          v
+-----------------------------------+
|  Rate Limiter                     |
|  Checks: Has this IP exceeded     |
|  100 requests per minute?         |
+-----------------------------------+
          |
     +----+----+
     |         |
  WITHIN     EXCEEDED
   LIMIT      LIMIT
     |         |
     v         v
+--------+  +-------------------------------+
|Backend |  | Return HTTP 429               |
|handles |  | "Too Many Requests"           |
|request |  | "Retry after 60 seconds"      |
+--------+  +-------------------------------+
```

### What Rate Limiting Prevents

| Threat           | Description                                               |
|------------------|-----------------------------------------------------------|
| DDoS Attacks     | Flooding a server with millions of requests to crash it   |
| Brute Force      | Trying thousands of password combinations automatically   |
| Bot Abuse        | Automated scripts scraping data or submitting spam forms  |
| API Exploitation | Excessive API calls consuming server resources unfairly   |

### Example Response When Limit Is Exceeded

```
HTTP/1.1 429 Too Many Requests
Retry-After: 60
Content-Type: application/json

{
  "error": "Rate limit exceeded. Try again in 60 seconds."
}
```

---

## 7. Cache — The Speed Layer

A cache is a temporary storage layer that keeps copies of frequently requested data in memory, so the server does not need to fetch it from the database every time.

### The Problem Without Cache

Every user request hits the database. Database queries are comparatively slow — especially when multiple users are making the same request simultaneously.

```
Without Cache:
User --> Server --> Database --> Server --> User
         (Query executed every single time)
```

### The Solution With Cache

Frequently accessed data is stored in fast in-memory storage (like Redis). Subsequent requests are served from the cache without touching the database.

```
With Cache:
User --> Server --> Cache (data found) --> User     [Fast path]
User --> Server --> Cache (data absent) --> Database --> Cache --> User  [First fetch only]
```

### Cache Hit vs Cache Miss

| Scenario        | What Happens                                              | Performance  |
|-----------------|-----------------------------------------------------------|--------------|
| Cache Hit       | Requested data is found in cache; returned immediately    | Very fast    |
| Cache Miss      | Data not in cache; fetched from database, then cached     | Slower once, fast on retry |

### Cache Hit Workflow

```
Incoming Request
      |
      v
+-------------------+
|   Cache (Redis)   |
+-------------------+
      |
   Data Found?
      |
   YES --> Return data immediately to user
   NO  --> Fetch from Database
              |
              v
        Store in Cache
              |
              v
        Return data to user
```

### Benefits of Caching

- Dramatically reduces average response times
- Reduces load on the database, extending its capacity
- Allows the application to serve more users simultaneously
- Improves the user experience with near-instant responses

> **Common caching tool:** Redis — an in-memory key-value store designed for speed.

---

## 8. Cookies

A cookie is a small piece of data (a string of text) that a server sends to the browser. The browser stores it and automatically sends it back with every subsequent request to the same server.

### Cookie Structure (Example)

```
Set-Cookie: sessionId=abc123xyz; HttpOnly; Secure; Max-Age=86400
```

| Attribute   | Description                                             |
|-------------|---------------------------------------------------------|
| sessionId   | The name of the cookie (key-value pair)                 |
| HttpOnly    | Prevents JavaScript from reading the cookie (security)  |
| Secure      | Cookie only sent over HTTPS connections                 |
| Max-Age     | How long (in seconds) the cookie persists               |

### How Cookies Work

```
STEP 1 — Server sends a cookie:
   Server  -->  "Set-Cookie: sessionId=abc123"  -->  Browser

STEP 2 — Browser stores the cookie automatically.

STEP 3 — On every future request, the browser includes the cookie:
   Browser  -->  "Cookie: sessionId=abc123"  -->  Server

STEP 4 — Server reads the cookie to identify the user.
```

### Common Uses of Cookies

| Use Case              | Example                                         |
|-----------------------|-------------------------------------------------|
| Session management    | Stores the session ID to keep users logged in   |
| User preferences      | Remembers dark mode, language, or region        |
| Shopping cart state   | Tracks which items are in a guest user's cart   |
| Analytics tracking    | Identifies returning visitors                   |

---

## 9. Sessions

A session is a server-side record that stores information about an authenticated user. Unlike cookies, which live in the browser, sessions live on the server — making them more secure.

### What a Session Looks Like (Stored on Server)

```
Session ID:  abc123xyz

Session Data:
{
  userId:     45,
  username:   "tushar",
  role:       "admin",
  loggedInAt: "2026-06-23T14:00:00Z",
  expiresAt:  "2026-06-24T14:00:00Z"
}
```

The browser only knows the **session ID** — the actual data never leaves the server.

### Cookie vs Session — Comparison

| Feature         | Cookie                              | Session                           |
|-----------------|-------------------------------------|-----------------------------------|
| Stored in       | Browser (client-side)               | Server (server-side)              |
| Contains        | Session ID (just a reference key)   | Full user data                    |
| Visible to user | Yes (can be inspected in browser)   | No                                |
| Security        | Lower — can be stolen if not secured| Higher — data stays on server     |
| Size limit      | ~4 KB                               | No fixed limit                    |
| Typical content | `sessionId=abc123`                  | `{ userId, role, email, ... }`    |

---

## 10. Session-Based Authentication

Session-based authentication is the mechanism by which a server tracks and verifies authenticated users across multiple requests using sessions and cookies.

### Phase 1 — Login (First Request)

```
Browser sends:  POST /login  { email, password }
                       |
                       v
         +----------------------------+
         | Backend validates the      |
         | credentials against DB     |
         +----------------------------+
                       |
              Valid? --+-- Invalid?
                |               |
                v               v
   +------------------+    +-----------------+
   | Create Session   |    | Return 401      |
   | Store in server  |    | "Unauthorized"  |
   +------------------+    +-----------------+
                |
                v
   +----------------------------------+
   | Send response with:             |
   | Set-Cookie: sessionId=abc123    |
   +----------------------------------+
                |
                v
   +----------------------------------+
   | Browser stores cookie           |
   | User is authenticated           |
   +----------------------------------+
```

### Phase 2 — Subsequent Requests

```
Browser sends:  GET /dashboard
                Header: Cookie: sessionId=abc123
                       |
                       v
         +----------------------------------+
         | Backend reads sessionId cookie  |
         | Looks up session on server      |
         +----------------------------------+
                       |
            Found? ---+--- Not Found / Expired?
                |                     |
                v                     v
   +--------------------+    +-------------------+
   | Identify user:     |    | Return 401        |
   | { userId, role }   |    | "Session expired" |
   | Grant access       |    +-------------------+
   +--------------------+
```

---

## 11. Complete Full Stack Request Flow

The following diagram shows how all components of a full stack application work together to serve a single user request.

```
+----------------------------------------------+
|              User Action                     |
|  (clicks button, submits form, etc.)         |
+----------------------------------------------+
                      |
                      v
+----------------------------------------------+
|              Browser / Client                |
|  Captures user event, initiates HTTP request |
+----------------------------------------------+
                      |
                      v
+----------------------------------------------+
|              Frontend (React / HTML)         |
|  Builds the request body, calls API endpoint |
+----------------------------------------------+
                      |  HTTP Request (e.g. POST /login)
                      v
+----------------------------------------------+
|              API Layer (Express Route)       |
|  Receives request, routes to handler         |
+----------------------------------------------+
                      |
                      v
+----------------------------------------------+
|              Rate Limiter                    |
|  Is request count within allowed limits?     |
+----------------------------------------------+
         |                        |
      Within Limit          Limit Exceeded
         |                        |
         v                        v
+------------------+   +----------------------+
| Backend Logic    |   | Return HTTP 429      |
| (Node.js logic)  |   | "Too Many Requests"  |
+------------------+   +----------------------+
         |
         v
+----------------------------------------------+
|              Cache (Redis)                   |
|  Is the requested data already cached?       |
+----------------------------------------------+
         |                        |
     Cache Hit               Cache Miss
         |                        |
         v                        v
+------------------+   +----------------------+
| Return cached    |   | Query Database       |
| response (fast)  |   | (MongoDB/PostgreSQL) |
+------------------+   +----------------------+
                                 |
                                 v
                        +-------------------+
                        | Store in cache    |
                        | Prepare response  |
                        +-------------------+
                                 |
                                 v
+----------------------------------------------+
|              API sends JSON Response         |
+----------------------------------------------+
                      |
                      v
+----------------------------------------------+
|              Frontend updates the UI         |
+----------------------------------------------+
                      |
                      v
+----------------------------------------------+
|              User sees the result            |
+----------------------------------------------+
```

---

## 12. Real-World Example — Instagram Login

The following table traces every step that occurs when you log into Instagram:

| Step | User Action / System Action                               | Layer Involved      |
|------|-----------------------------------------------------------|---------------------|
| 1    | User types username and password                          | User                |
| 2    | Instagram app captures input, builds login request        | Frontend            |
| 3    | Request sent: `POST /accounts/login/`                     | API Layer           |
| 4    | Rate limiter checks: has this IP made too many attempts?  | Security / Gateway  |
| 5    | Backend validates credentials against user database       | Backend + Database  |
| 6    | If valid, a session is created and stored server-side     | Backend             |
| 7    | A session cookie is sent back to the browser              | Backend + Browser   |
| 8    | Browser stores the cookie                                 | Browser             |
| 9    | User is redirected to their home feed                     | Frontend            |
| 10   | Every subsequent request includes the cookie              | Browser             |
| 11   | Server reads cookie, looks up session, shows user's data  | Backend             |

---

## 13. Key Concepts Summary

| Concept          | Layer                | Role in the System                                    |
|------------------|----------------------|-------------------------------------------------------|
| Frontend         | Presentation Layer   | Renders UI; captures user input; calls the API        |
| Backend          | Application Layer    | Validates, processes, and applies business rules      |
| API              | Communication Layer  | Defines how frontend and backend talk to each other   |
| Database         | Data Layer           | Persists all application data permanently             |
| Cache            | Performance Layer    | Stores frequently accessed data in memory for speed   |
| Rate Limiting    | Security Layer       | Prevents abuse by capping request frequency           |
| Cookies          | Client Storage       | Small browser-side data stores used to track sessions |
| Sessions         | Server Storage       | Server-side user data mapped to a cookie session ID   |

---

## 14. Glossary

| Term             | Definition                                                              |
|------------------|-------------------------------------------------------------------------|
| User             | The person interacting with the application                             |
| Browser          | Software used to access the web (Chrome, Firefox, Safari)              |
| Frontend         | The visual, client-side layer of the application                        |
| Backend          | The server-side layer responsible for logic and processing              |
| API              | The interface (set of endpoints) connecting frontend and backend        |
| Database         | A structured system for storing and retrieving persistent data          |
| Cache            | Temporary, fast storage used to reduce database load                    |
| Rate Limiting    | A control mechanism that limits the number of requests per time window  |
| Cookie           | A small key-value string stored in the browser by the server            |
| Session          | A server-stored record of an authenticated user's data                  |
| Server           | A machine or process that listens for requests and sends responses      |
| Client           | The browser or application that sends requests to the server            |
| HTTP             | HyperText Transfer Protocol — the communication standard of the web    |
| Request          | A message sent from a client to a server asking for data or an action  |
| Response         | The server's reply to a client's request                                |
| JSON             | JavaScript Object Notation — the standard data format for APIs         |
| Authentication   | The process of verifying who a user is                                  |
| Authorization    | The process of verifying what a user is allowed to do                   |

---

## 15. Quick Revision

### One-Line Definitions

```
Frontend      = What the user sees (HTML, CSS, React)
Backend       = Logic and processing layer (Node.js, Express, Django)
API           = The defined interface connecting frontend and backend
Database      = Persistent storage for all application data
Cache         = Temporary fast storage to reduce database hits (Redis)
Rate Limiting = Throttles excessive requests to protect the server
Cookie        = A small browser-stored string (e.g. sessionId)
Session       = Server-stored user data keyed by a session ID
Server        = A machine that receives requests and sends responses
```

### True / False Check

| Statement                                       | Answer  |
|-------------------------------------------------|---------|
| Frontend is what the user sees                  | True    |
| Backend permanently stores all data             | False — that is the Database |
| The API connects frontend and backend           | True    |
| Caching makes applications slower               | False — it makes them faster |
| Cookies are stored on the server                | False — they are stored in the browser |
| Sessions are stored on the server               | True    |
| Rate limiting helps prevent DDoS attacks        | True    |
| Redis is a common caching tool                  | True    |

### HTTP Status Codes to Know

| Code | Category    | Meaning                                 |
|------|-------------|-----------------------------------------|
| 200  | Success     | Request completed successfully          |
| 201  | Success     | Resource created successfully           |
| 400  | Client Error| Bad Request — malformed or missing data |
| 401  | Client Error| Unauthorized — not logged in            |
| 403  | Client Error| Forbidden — logged in but no permission |
| 404  | Client Error| Not Found — resource does not exist     |
| 429  | Client Error| Too Many Requests — rate limit exceeded |
| 500  | Server Error| Internal Server Error — backend fault   |

### Complete Architecture at a Glance

```
User
 |
 v
Browser
 |
 v
Frontend  (builds and sends HTTP request)
 |
 v
API Layer  (routes to correct handler)
 |
 v
Rate Limiter  --[exceeded]--> Return 429
 |
 v
Backend Logic  (validates, authenticates, applies rules)
 |
 v
Cache  --[hit]--> Return cached data immediately
 |  [miss]
 v
Database  (read or write data)
 |
 v
Cache  (store new result)
 |
 v
Response built by Backend
 |
 v
Frontend updates the UI
 |
 v
User sees the result
```


