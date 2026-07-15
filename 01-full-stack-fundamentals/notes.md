# Full Stack Fundamentals

> **Module 01** · Prerequisites: None · Next: [HTML →](../02-html/notes.md)

---

## Table of Contents

1. [What is Full Stack Development?](#1-what-is-full-stack-development)
2. [How the Internet Works](#2-how-the-internet-works)
3. [The Browser](#3-the-browser)
4. [Client-Server Architecture](#4-client-server-architecture)
5. [Full Stack Architecture](#5-full-stack-architecture)
6. [The Request-Response Cycle](#6-the-request-response-cycle)
7. [HTTP Overview](#7-http-overview)
8. [State: Cookies, Sessions & Storage](#8-state-cookies-sessions--storage)
9. [Caching](#9-caching)
10. [Rate Limiting](#10-rate-limiting)
11. [Tech Stacks](#11-tech-stacks)
12. [Databases](#12-databases)
13. [Developer Tooling & Workflow](#13-developer-tooling--workflow)
14. [Deployment Overview](#14-deployment-overview)
15. [Full Stack Roadmap](#15-full-stack-roadmap)

---

## 1. What is Full Stack Development?

A **full stack developer** designs and builds every layer of a web application — the user interface, the server-side logic, and the database. The term "full stack" refers to the complete set of software layers required to deliver a working application to a user.

**The three primary stacks:**

| Stack | Responsibility | Examples |
|:------|:---------------|:---------|
| **Frontend** | The interface rendered in a user's browser | HTML, CSS, React, Next.js |
| **Backend** | Server-side logic, data processing, APIs | Node.js, Django, Spring Boot |
| **Database** | Persistent data storage and retrieval | PostgreSQL, MongoDB, Redis |

---

## 2. How the Internet Works

### DNS Resolution

When a user enters `https://example.com` into a browser, the following sequence occurs:

```
Browser → DNS Resolver → Root Nameserver → TLD Nameserver → Authoritative Nameserver
                                                                        ↓
                                              IP Address returned: 93.184.216.34
```

1. The browser checks its local DNS cache for the domain's IP address.
2. If not cached, it queries the operating system's DNS resolver.
3. The resolver contacts a series of nameservers to find the authoritative record.
4. The IP address is returned and cached for future requests.

### TCP/IP

All web communication travels over **TCP/IP** (Transmission Control Protocol / Internet Protocol). TCP ensures packets arrive in order and without loss. IP addresses identify machines on the network.

### TLS/SSL

**HTTPS** adds a TLS handshake before any HTTP data is transmitted. TLS:
- Authenticates the server via a certificate issued by a trusted Certificate Authority.
- Establishes an encrypted channel so data cannot be read in transit.

> All production web applications must use HTTPS. HTTP transmits data in plaintext.

---

## 3. The Browser

A web browser is a runtime environment that:

- Fetches resources (HTML, CSS, JavaScript, images) over HTTP.
- Parses HTML and constructs the **DOM** (Document Object Model) — a tree of nodes representing the page structure.
- Applies CSS rules to compute the visual layout.
- Executes JavaScript, which can read and modify the DOM and make additional network requests.

**Major browser engines:**

| Engine | Used By |
|:-------|:--------|
| V8 | Chrome, Edge, Node.js |
| SpiderMonkey | Firefox |
| JavaScriptCore | Safari |

> The DOM and JavaScript interaction are covered in depth in [04 — JavaScript](../04-javascript/notes.md).

---

## 4. Client-Server Architecture

Every web application is built on a **client-server** model. The client initiates requests; the server processes them and sends responses.

```
+------------------+        HTTP Request        +------------------+
|                  | ─────────────────────────> |                  |
|   Client         |                            |   Server         |
|   (Browser /     |                            |   (Application + |
|   Mobile App)    | <───────────────────────── |   Database)      |
|                  |        HTTP Response        |                  |
+------------------+                            +------------------+
```

**Key characteristics:**

- The client is responsible for rendering the UI and capturing user input.
- The server is responsible for business logic, data validation, and database access.
- Communication between them follows a defined contract — the API.
- The server is stateless by default: each request must carry enough information to be processed independently.

---

## 5. Full Stack Architecture

A production web application is divided into distinct layers, each with a clearly bounded responsibility.

```
+----------------------------------------------------------+
|  USER                                                    |
|  Person interacting with the application                 |
+----------------------------------------------------------+
                          │
                          ▼
+----------------------------------------------------------+
|  BROWSER / CLIENT                                        |
|  Chrome, Firefox, Safari, Mobile App                    |
|  Renders the UI; sends HTTP requests                     |
+----------------------------------------------------------+
                          │
                          ▼
+----------------------------------------------------------+
|  FRONTEND  (Presentation Layer)                          |
|  HTML, CSS, JavaScript, React, Next.js                  |
|  Constructs the user interface; handles user events      |
+----------------------------------------------------------+
                          │  HTTP Request
                          ▼
+----------------------------------------------------------+
|  API LAYER  (Communication Layer)                        |
|  REST Endpoints, GraphQL, gRPC                          |
|  Routes requests to the correct handler                  |
+----------------------------------------------------------+
                          │
                          ▼
+----------------------------------------------------------+
|  BACKEND  (Application Logic Layer)                      |
|  Node.js/Express, Django, FastAPI, Spring Boot          |
|  Validates input; applies business rules; queries DB     |
+----------------------------------------------------------+
                          │
                          ▼
+----------------------------------------------------------+
|  DATABASE  (Persistence Layer)                           |
|  PostgreSQL, MongoDB, MySQL, Redis                      |
|  Stores and retrieves all persistent data                |
+----------------------------------------------------------+
```

**Layer responsibilities at a glance:**

| Layer | Primary Responsibility |
|:------|:----------------------|
| **Frontend** | Render UI, capture input, call the API |
| **API** | Define the contract between frontend and backend |
| **Backend** | Validate, process, apply business rules |
| **Database** | Persist and retrieve data reliably |

---

## 6. The Request-Response Cycle

Every user action that requires data follows this cycle:

```
User clicks "Submit"
        │
        ▼
Frontend captures the event
        │
        ▼
Frontend builds an HTTP request (method + URL + headers + body)
        │  POST /api/orders  { productId: 42, qty: 1 }
        ▼
API Layer receives and routes the request
        │
        ▼
Rate Limiter checks request frequency
        │  429 Too Many Requests ──→ Client
        ▼
Backend handler executes:
  1. Authenticate the caller (is the token valid?)
  2. Authorize the action (does this user have permission?)
  3. Validate the input (is qty a positive integer?)
  4. Apply business logic (is the product in stock?)
  5. Query the database (INSERT order record)
        │
        ▼
Database returns a result
        │
        ▼
Backend formats a JSON response { "orderId": 9981 }
        │  HTTP 201 Created
        ▼
Frontend receives the response and updates the UI
        │
        ▼
User sees the confirmation
```

---

## 7. HTTP Overview

**HTTP (Hypertext Transfer Protocol)** is the application-layer protocol that defines how clients and servers communicate.

> HTTP is covered in full detail in [05 — HTTP, JSON & Fetch](../05-http-json-fetch/notes.md). This section provides the conceptual orientation required for understanding the architecture.

### HTTP Methods

| Method | Semantic Meaning | Safe | Idempotent |
|:-------|:-----------------|:----:|:----------:|
| `GET` | Read a resource | ✓ | ✓ |
| `POST` | Create a resource | ✗ | ✗ |
| `PUT` | Replace a resource entirely | ✗ | ✓ |
| `PATCH` | Partially update a resource | ✗ | Usually |
| `DELETE` | Remove a resource | ✗ | ✓ |

### HTTP Status Code Families

| Range | Meaning |
|:------|:--------|
| `1xx` | Informational |
| `2xx` | Success |
| `3xx` | Redirection |
| `4xx` | Client Error |
| `5xx` | Server Error |

---

## 8. State: Cookies, Sessions & Storage

HTTP is stateless. Each request is independent. Applications use several mechanisms to maintain state across requests.

### Cookies

A **cookie** is a small string the server sends to the browser via the `Set-Cookie` response header. The browser stores it and sends it back automatically with every subsequent request to the same origin.

```
Server → Set-Cookie: sessionId=a1b2c3; HttpOnly; Secure; SameSite=Strict; Max-Age=86400
Browser stores the cookie.
Browser → Cookie: sessionId=a1b2c3  (sent with every future request)
```

| Cookie Attribute | Effect |
|:-----------------|:-------|
| `HttpOnly` | Prevents JavaScript from reading the cookie (mitigates XSS) |
| `Secure` | Transmits only over HTTPS |
| `SameSite=Strict` | Prevents the cookie from being sent with cross-site requests (mitigates CSRF) |
| `Max-Age` | Expiry in seconds; omitting it creates a session cookie (cleared on browser close) |

### Sessions

A **session** is a server-side record of an authenticated user's state. The browser holds only the session ID (in a cookie). All meaningful data — user ID, role, permissions — is stored on the server, typically in Redis.

```json
Session ID: a1b2c3 (stored in browser cookie)

Session record (stored on server / Redis):
{
  "userId": 45,
  "role": "admin",
  "createdAt": "2026-07-15T12:00:00Z",
  "expiresAt": "2026-07-16T12:00:00Z"
}
```

| | Cookie | Session |
|:--|:-------|:--------|
| **Stored in** | Browser | Server |
| **Contains** | Session ID only | Full user data |
| **Size limit** | ~4 KB | No hard limit |
| **Security** | Lower (client-side) | Higher (server-side) |

### localStorage and sessionStorage

Browser-native key-value stores for client-side persistence. Unlike cookies, these are never automatically sent to the server.

| | localStorage | sessionStorage | Cookie |
|:--|:-------------|:---------------|:-------|
| **Cleared** | Never (manual only) | On tab close | On expiry or manually |
| **Sent to server** | ✗ | ✗ | ✓ |
| **Size** | ~5 MB | ~5 MB | ~4 KB |

> `localStorage` and `sessionStorage` JavaScript APIs are documented in [04 — JavaScript](../04-javascript/notes.md#browser-storage-apis).

---

## 9. Caching

A **cache** is a temporary storage layer that holds copies of responses so that the same data does not need to be recomputed or re-fetched from the database for every request.

```
Without cache:
Request → Server → Database → Server → Client
                  (executed every time, potentially slow)

With cache (hit):
Request → Server → Cache → Client
                  (immediate, no database query)

With cache (miss):
Request → Server → Cache (empty) → Database → Cache (stored) → Client
                  (slower once, then fast for all subsequent requests)
```

**Cache types:**

| Type | Location | Common Tools |
|:-----|:---------|:-------------|
| **In-memory cache** | Application server | Redis, Memcached |
| **HTTP cache** | Browser / CDN | `Cache-Control`, `ETag`, `Last-Modified` headers |
| **CDN (Edge cache)** | Geographically distributed nodes | Cloudflare, AWS CloudFront |

> HTTP caching headers are documented in detail in [06 — API Design](../06-api-design/notes.md#caching).

---

## 10. Rate Limiting

**Rate limiting** restricts the number of requests a client (identified by IP address, API key, or user ID) can make within a defined time window.

```
Incoming Request
        │
        ▼
Rate Limiter: Has this client exceeded N requests per minute?
        │                           │
       NO                          YES
        │                           │
        ▼                           ▼
  Forward to handler         HTTP 429 Too Many Requests
                             Retry-After: 60
```

**What rate limiting prevents:**

| Threat | Description |
|:-------|:------------|
| **DDoS** | Flooding a server with millions of requests to exhaust its resources |
| **Brute force** | Automated repeated password attempts |
| **Scraping** | Excessive automated data extraction |
| **API abuse** | A single client consuming a disproportionate share of server capacity |

> Rate limiting implementation for API servers is covered in [06 — API Design](../06-api-design/notes.md#rate-limiting).

---

## 11. Tech Stacks

A **tech stack** is the combination of technologies used to build an application end-to-end.

### Popular Stacks

| Stack | Technologies | Primary Use Case |
|:------|:-------------|:-----------------|
| **MERN** | MongoDB, Express, React, Node.js | Full-stack JavaScript web apps |
| **PERN** | PostgreSQL, Express, React, Node.js | Full-stack JS with relational data |
| **MEAN** | MongoDB, Express, Angular, Node.js | Full-stack JS with Angular frontend |
| **T3** | TypeScript, tRPC, Tailwind, Prisma, Next.js | Type-safe modern full-stack |
| **Django** | Django, PostgreSQL, HTML/CSS/JS | Python-based full-stack |
| **LAMP** | Linux, Apache, MySQL, PHP | Traditional web apps, WordPress |

### MERN Architecture

```
React.js (Frontend)
        │
        │  GET /api/users  (HTTP)
        ▼
Express.js (API + Backend)
        │
        ▼
Node.js (Runtime)
        │
        ▼
MongoDB (Database)
        │  Returns JSON documents
        ▼
Express.js formats response
        │
        ▼
React.js receives data, updates state, re-renders
```

**Why MERN is popular:**
- A single language (JavaScript) across frontend, backend, and database layer.
- Large ecosystem of packages via npm.
- Fast initial development cycle suited to startups and prototypes.

---

## 12. Databases

### SQL (Relational Databases)

Stores data in **tables** with fixed schemas, rows, and columns. Relationships between tables are enforced by the database engine using foreign keys and JOIN operations.

```sql
-- Users table
| id | name  | email           |
|----|-------|-----------------|
|  1 | Alice | alice@email.com |

-- Orders table
| id | userId | product |
|----|--------|---------|
| 10 | 1      | Laptop  |

-- Retrieve with JOIN
SELECT users.name, orders.product
FROM orders JOIN users ON orders.userId = users.id;
```

**SQL databases:** PostgreSQL, MySQL, SQLite, Supabase

### NoSQL (Non-Relational Databases)

Stores data as flexible documents, key-value pairs, or graphs. The schema is not enforced by the database — it is the application's responsibility.

```json
// MongoDB document
{
  "_id": "648f...",
  "name": "Alice",
  "email": "alice@email.com",
  "orders": [
    { "product": "Laptop", "qty": 1 }
  ]
}
```

**NoSQL databases:** MongoDB, Redis, Firebase, Cassandra

### SQL vs NoSQL

| Feature | SQL | NoSQL |
|:--------|:----|:------|
| **Schema** | Fixed, defined upfront | Flexible, per-document |
| **Relationships** | Native (JOIN, foreign keys) | Application-managed |
| **Consistency** | Strong (ACID) | Eventual (typically) |
| **Best for** | Financial data, complex relations | Rapid iteration, variable structure |
| **Scaling** | Vertical (primarily) | Horizontal |

---

## 13. Developer Tooling & Workflow

**Core tools every full stack developer uses:**

| Tool | Purpose |
|:-----|:--------|
| **VS Code** | Code editor with extension ecosystem |
| **Git** | Version control — track changes, collaborate, roll back |
| **GitHub / GitLab** | Remote repository hosting, pull requests, CI/CD |
| **Node.js / npm** | JavaScript runtime; package manager |
| **Postman / Insomnia** | API testing and exploration |
| **Docker** | Containerise the application for consistent environments |
| **Browser DevTools** | Inspect DOM, debug JS, profile network requests |

### Git Workflow (Feature Branch)

```
main ─────────────────────────────────────────────────────▶ production
         │                              │
         └─ feature/login-page ─────────┘
              commit → commit → PR → review → merge
```

---

## 14. Deployment Overview

**Deployment** is the process of making an application accessible to users over the internet.

| Component | Options |
|:----------|:--------|
| **Frontend hosting** | Vercel, Netlify, GitHub Pages, AWS S3 + CloudFront |
| **Backend hosting** | Railway, Render, AWS EC2, Google Cloud Run, Fly.io |
| **Database** | Supabase (PostgreSQL), MongoDB Atlas, PlanetScale |
| **CI/CD** | GitHub Actions, GitLab CI, CircleCI |

**Deployment stages:**

```
Local (dev) → Staging (testing) → Production (users)
```

Environment variables (secrets, database URLs, API keys) must never be committed to version control. Use `.env` files locally and platform-provided secret management in production.

---

## 15. Full Stack Roadmap

The recommended learning sequence for this repository:

```
01 - Full Stack Fundamentals  ← You are here
02 - HTML
03 - CSS
04 - JavaScript
05 - HTTP, JSON & Fetch
06 - API Design
─────────────────────────── (Upcoming)
07 - Node.js & Express
08 - Databases (PostgreSQL, MongoDB)
09 - Authentication (JWT, OAuth 2.0)
10 - React
11 - Next.js
12 - Deployment
```

---

> **Next:** [02 — HTML →](../02-html/notes.md)
