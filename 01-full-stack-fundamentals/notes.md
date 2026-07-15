# Full Stack Fundamentals

> **Module 01** · Prerequisites: None · Next: [HTML →](../02-html/notes.md)


## Table of Contents

1. [What is Full Stack Development?](#1-what-is-full-stack-development)
2. [How the Internet Works](#2-how-the-internet-works)
3. [The Browser](#3-the-browser)
4. [Client-Server Architecture](#4-client-server-architecture)
5. [Web Architecture](#5-web-architecture)
6. [HTTP Overview](#6-http-overview)
7. [Tech Stacks](#7-tech-stacks)
8. [Databases](#8-databases)
9. [Version Control](#9-version-control)
10. [Developer Tooling & Workflow](#10-developer-tooling--workflow)
11. [Deployment Overview](#11-deployment-overview)
12. [Full Stack Roadmap](#12-full-stack-roadmap)


## 1. What is Full Stack Development?

A **full stack developer** designs and builds every layer of a web application — the user interface, the server-side logic, and the database. The term "full stack" refers to the complete set of software layers required to deliver a working application to a user.

| Layer | Responsibility | Examples |
|:------|:---------------|:---------|
| **Frontend** | The interface rendered in a user's browser | HTML, CSS, React, Next.js |
| **Backend** | Server-side logic, data processing, APIs | Node.js, Django, Spring Boot |
| **Database** | Persistent data storage and retrieval | PostgreSQL, MongoDB, Redis |


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

> **Warning:** All production web applications must use HTTPS. HTTP transmits data in plaintext.


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

> **Note:** The DOM and JavaScript interaction are covered in depth in [Module 04 — JavaScript](../04-javascript/notes.md#16-dom-manipulation).


## 4. Client-Server Architecture

Every web application is built on a **client-server** model. The client initiates requests; the server processes them and sends responses.

```
+------------------+        HTTP Request        +------------------+
|                  | ─────────────────────────→ |                  |
|   Client         |                            |   Server         |
|   (Browser /     |                            |   (Application + |
|   Mobile App)    | ←───────────────────────── |   Database)      |
|                  |        HTTP Response        |                  |
+------------------+                            +------------------+
```

**Key characteristics:**

- The client is responsible for rendering the UI and capturing user input.
- The server is responsible for business logic, data validation, and database access.
- Communication between them follows a defined contract — the API.
- The server is stateless by default: each request must carry enough information to be processed independently.


## 5. Web Architecture

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

| Layer | Primary Responsibility |
|:------|:----------------------|
| **Frontend** | Render UI, capture input, call the API |
| **API** | Define the contract between frontend and backend |
| **Backend** | Validate, process, apply business rules |
| **Database** | Persist and retrieve data reliably |


## 6. HTTP Overview

**HTTP (Hypertext Transfer Protocol)** is the application-layer protocol that defines how clients and servers communicate. Every interaction between a browser and a server is an HTTP transaction consisting of a **request** and a **response**.

HTTP is stateless — each request is independent. The server does not retain information about previous requests unless a persistence mechanism (cookies, tokens) is used.

> **Note:** HTTP is covered in full detail in [Module 05 — HTTP, JSON & Fetch](../05-http-json-fetch/notes.md). This section provides only the conceptual introduction required for understanding web architecture.


## 7. Tech Stacks

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


## 8. Databases

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
{
  "_id": "648f...",
  "name": "Tushar",
  "email": "thetushardev0@gmail.com",
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


## 9. Version Control

**Git** is the industry-standard version control system. It tracks changes to files over time, enables collaboration, and supports rollback to any previous state.

### Core Concepts

| Concept | Description |
|:--------|:------------|
| **Repository** | A project directory tracked by Git |
| **Commit** | A snapshot of the project at a point in time |
| **Branch** | An independent line of development |
| **Merge** | Combining changes from one branch into another |
| **Pull Request** | A review process before merging code |
| **Remote** | A hosted copy of the repository (GitHub, GitLab) |

### Git Workflow (Feature Branch)

```
main ─────────────────────────────────────────────────────▶ production
         │                              │
         └─ feature/login-page ─────────┘
              commit → commit → PR → review → merge
```

### Essential Commands

```bash
git init                    # Initialise a new repository
git clone <url>             # Clone a remote repository
git add .                   # Stage all changes
git commit -m "message"     # Commit staged changes
git push origin main        # Push commits to remote
git pull origin main        # Pull latest changes
git checkout -b feature/x   # Create and switch to a new branch
git merge feature/x         # Merge a branch into the current branch
git log --oneline -10       # View last 10 commits
```


## 10. Developer Tooling & Workflow

| Tool | Purpose |
|:-----|:--------|
| **VS Code** | Code editor with extension ecosystem |
| **Git** | Version control — track changes, collaborate, roll back |
| **GitHub / GitLab** | Remote repository hosting, pull requests, CI/CD |
| **Node.js / npm** | JavaScript runtime; package manager |
| **Postman / Insomnia** | API testing and exploration |
| **Docker** | Containerise the application for consistent environments |
| **Browser DevTools** | Inspect DOM, debug JS, profile network requests |


## 11. Deployment Overview

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

> **Warning:** Environment variables (secrets, database URLs, API keys) must never be committed to version control. Use `.env` files locally and platform-provided secret management in production.


## 12. Full Stack Roadmap

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
