# 📚 Full Stack Basics
> **Day 1 | Full Stack Development**
> _Simple notes, easy language, quick to revise!_


## 📌 Table of Contents
1. [What Happens When a User Interacts With a Website?](#1-what-happens-when-a-user-interacts-with-a-website)
2. [Full Stack Architecture Overview](#2-full-stack-architecture-overview)
3. [Components of Full Stack Architecture](#3-components-of-full-stack-architecture)
4. [Login Flow — Step by Step](#4-login-flow--step-by-step)
5. [What is a Server?](#5-what-is-a-server)
6. [Rate Limiting](#6-rate-limiting)
7. [Cache (Speed Layer)](#7-cache-speed-layer)
8. [Cookies](#8-cookies)
9. [Sessions](#9-sessions)
10. [Session-Based Authentication](#10-session-based-authentication)
11. [Complete Full Stack Flow](#11-complete-full-stack-flow)
12. [Real-World Example — Instagram Login](#12-real-world-example--instagram-login)
13. [Key Concepts Summary](#13-key-concepts-summary)
14. [Glossary](#14-glossary)
15. [Quick Revision](#15-quick-revision)


## 1. What Happens When a User Interacts With a Website?

> When a user does something on a website (like clicking a button), a whole system wakes up and works behind the scenes.

This system is called **full stack architecture**.

**Simple analogy — a Restaurant:**

```
You (User)
 ↓
Waiter takes your order (Frontend + API)
 ↓
Kitchen prepares it (Backend)
 ↓
Storage room gets ingredients (Database)
 ↓
Waiter brings food back (Response)
 ↓
You eat! (User sees result)
```

### 📤 Request Flow (User → System):

```
User
 ↓
Browser (Client)
 ↓
Frontend (UI Layer)
 ↓
API Layer (Messenger)
 ↓
Backend (Logic Layer)
 ↓
Database (Data Layer)
```

### 📥 Response Flow (System → User):

```
Database
 ↓
Backend
 ↓
API Layer
 ↓
Frontend
 ↓
Browser
 ↓
User sees result ✅
```


## 2. Full Stack Architecture Overview

> A **full stack app** has multiple layers, each with a clear job.

```
┌──────────────────────────────────┐
│            User                  │  ← Person using the app
├──────────────────────────────────┤
│     Browser / Client             │  ← Chrome, Firefox, Safari
├──────────────────────────────────┤
│    Frontend (Presentation Layer) │  ← HTML, CSS, React
├──────────────────────────────────┤
│    API Layer (Communication)     │  ← REST API, endpoints
├──────────────────────────────────┤
│    Backend (Application Logic)   │  ← Node.js, Express, Django
├──────────────────────────────────┤
│    Database (Data Storage)       │  ← MongoDB, PostgreSQL, MySQL
└──────────────────────────────────┘
```

| Layer        | Job                              | Real-Life Role     |
|--------------|----------------------------------|--------------------|
| **User**     | Interacts with the app           | Customer           |
| **Browser**  | Shows the website                | Menu + Table       |
| **Frontend** | What users see and click         | Waiter             |
| **API**      | Connects frontend and backend    | Order ticket       |
| **Backend**  | Processes logic and rules        | Kitchen            |
| **Database** | Stores everything permanently    | Storage room       |


## 3. Components of Full Stack Architecture

### 👤 1. User
The user is the person using the app.

**What users do:**
- Click buttons
- Fill out forms
- Search for products
- Log in / sign up

> The entire system is built to serve the user!


### 🌐 2. Browser / Client Layer

The browser is what the user opens to access the website.

**Examples:**
- Google Chrome
- Mozilla Firefox
- Safari

**What the browser does:**

| Responsibility             | Example                              |
|----------------------------|--------------------------------------|
| Displays the website       | Shows text, images, buttons          |
| Runs JavaScript code       | Makes buttons clickable              |
| Sends requests to server   | Sends your login info to backend     |
| Stores cookies             | Remembers you are logged in          |


### 🖥️ 3. Frontend (Presentation Layer)

> The frontend is **everything you can see on the screen**.

**Responsibilities:**
- Show content (text, images, buttons)
- Handle user input (clicks, typing)
- Send requests to backend through API
- Update the screen based on responses

**Technologies:**

```
HTML       → Structure of the page
CSS        → Styling (colors, layout)
JavaScript → Interactivity (click events)
React      → Component-based UI library
Next.js    → React with server-side rendering
Tailwind   → Utility CSS framework
```

**Example:** When you click "Add to Cart" → the frontend captures that click and sends a request to the backend.


### 🔗 4. API Layer (Communication Layer)

> **API** = Application Programming Interface — the **messenger** between frontend and backend.

```
Frontend  →  API  →  Backend
                ↑
         (Like a waiter who
          takes your order
          to the kitchen)
```

**What the API does:**
- Receives requests from the frontend
- Passes them to the backend
- Returns the response back to the frontend

**Example API endpoints:**

```
POST /login        → Send username + password to log in
GET  /products     → Get all products from database
POST /cart/add     → Add an item to cart
DELETE /cart/item  → Remove an item from cart
```

---

### ⚙️ 5. Backend (Application Logic Layer)

> The backend is the **brain** of the application. It processes everything.

**Responsibilities:**

| Task                     | Example                                    |
|--------------------------|--------------------------------------------|
| Processing requests      | Handling a login form submission           |
| Validating input         | Checking if email format is correct        |
| Authentication           | Verifying your password                    |
| Talking to the database  | Fetching your profile info                 |
| Applying business rules  | "Only admins can delete users"             |

**Technologies:**

```
Node.js + Express.js   → Most popular JS backend
Django                 → Python, great for quick apps
FastAPI                → Fast Python API framework
Spring Boot            → Java, used in enterprises
NestJS                 → TypeScript backend framework
```

**Example:** User submits login → backend checks if username + password match the database.


### 🗄️ 6. Database (Data Layer)

> The database is where **all data is stored permanently**.

**What gets stored:**
- User accounts (username, password, email)
- Orders (what you bought, when)
- Products (name, price, stock)
- Messages (chat history)

**Technologies:**

| Database       | Type     | Best For                            |
|----------------|----------|-------------------------------------|
| MongoDB        | NoSQL    | Flexible data, JSON format          |
| PostgreSQL     | SQL      | Complex relations, reliable         |
| MySQL          | SQL      | Very popular, easy to use           |
| SQLite         | SQL      | Small apps, local storage           |
| Redis          | NoSQL    | Fast caching, session storage       |


## 4. Login Flow — Step by Step

> Let's trace what happens when you log in to a website.

### The Flow:

```
User types username + password
          |
          ▼
Frontend sends request: POST /login
          |
          ▼
Backend receives the request
          |
          ▼
Backend checks credentials in Database
          |
          ▼
        Match? ──── No ──→ Return error: "Wrong password"
          |
         Yes
          |
          ▼
Backend creates a Session
          |
          ▼
Cookie with Session ID is sent to Browser
          |
          ▼
Browser stores Cookie
          |
          ▼
User is now logged in ✅
```

### Detailed Steps:

1. User enters email and password → clicks **Login**
2. Frontend sends: `POST /login` with the credentials
3. Backend receives it → validates the format
4. Backend queries the **database** → checks if user exists
5. If correct → creates a **session** on the server
6. Sends a **cookie** (with session ID) to the browser
7. Browser stores the cookie
8. For future pages → browser sends the cookie automatically
9. Backend reads the cookie → knows who you are → grants access ✅


## 5. What is a Server?

> A **server** is a computer (or program) that receives requests and sends back responses.

```
Client (Browser)
      |
      | Sends Request
      ▼
   Server
      |
      | Processes it
      ▼
   Response
      |
      ▼
Client receives it ✅
```

**What a server does:**

| Responsibility                  | Example                           |
|---------------------------------|-----------------------------------|
| Receive API requests            | `POST /login` arrives at server   |
| Run backend logic               | Validate password                 |
| Talk to the database            | Query user record                 |
| Send response back to client    | Return `{ success: true }`        |

> 💡 Think of the server as the **kitchen** — it does all the work you don't see.


## 6. Rate Limiting

> **Rate Limiting** = controlling how many requests a user can send in a given time.

**Example:**
```
100 requests per minute per user
```

If a user sends more than 100 requests in 1 minute → they get blocked temporarily.

### Where Rate Limiting Sits:

```
Client
  |
  ▼
API Gateway
  |
  ▼
Rate Limiter ← checks: "Too many requests?"
  |
  ▼
Backend (only if limit is OK)
```

### Why is it Important?

| Problem it Prevents  | What it means                               |
|----------------------|---------------------------------------------|
| DDoS Attacks         | Flooding the server with millions of requests |
| Bot Abuse            | Bots making automated fake requests          |
| Spam                 | Someone submitting a form thousands of times |
| Server Overload      | System crashing due to too many requests     |

### Example Response When Limit is Exceeded:

```
POST /login
→ Error 429: Too Many Requests
→ "Try again after 60 seconds"
```

> 💡 Like a bouncer at a club — if you're too rowdy, you get turned away!


## 7. Cache (Speed Layer)

> **Cache** = temporary fast storage. Saves frequently used data so it loads faster next time.

### Without Cache (Slow 🐢):

```
User → Server → Database → Server → User
           (Every request hits the database — slow!)
```

### With Cache (Fast ⚡):

```
User → Server → Cache → User
     (Data found in cache — instant response!)
```

### Cache Hit vs Cache Miss:

| Situation      | What Happens                                          | Speed  |
|----------------|-------------------------------------------------------|--------|
| **Cache Hit**  | Data found in cache → returned immediately            | ⚡ Fast |
| **Cache Miss** | Data NOT in cache → fetched from DB → stored in cache | 🐢 Slower (first time only) |

### Cache Hit Flow:
```
Request → Cache → ✅ Found → Response (fast!)
```

### Cache Miss Flow:
```
Request → Cache → ❌ Not found → Database → Store in Cache → Response
```

### Benefits of Cache:
- ✅ Faster response times
- ✅ Less load on the database
- ✅ App can handle more users
- ✅ Better user experience

> 🛠️ **Tool used for caching:** Redis


## 8. Cookies

> **Cookies** = small pieces of data stored **in the browser** by the server.

**Example cookie:**
```
sessionId=abc123xyz
```

### What Cookies Are Used For:

| Use Case              | Example                              |
|-----------------------|--------------------------------------|
| Keep user logged in   | Browser remembers your session ID    |
| Track session         | Know which user is making a request  |
| Save preferences      | Remember dark mode is ON             |
| Shopping cart         | Remember items you added             |

**How it works:**

```
Server → sends Cookie → Browser stores it
Browser → sends Cookie automatically with every request
Server → reads Cookie → knows who you are
```

> 💡 Cookies are like a **wristband at an event** — it proves you already paid to get in, so you don't have to pay again!


## 9. Sessions

> **Sessions** = user data stored **on the server**.

The browser only stores the **session ID** (in a cookie). The actual user data lives on the server.

**Example session (stored on server):**
```
sessionId: abc123xyz
userId: 45
username: tushar
role: admin
loggedInAt: 2026-06-23
```

### Cookie vs Session:

| Feature           | Cookie                          | Session                        |
|-------------------|---------------------------------|--------------------------------|
| Stored in         | Browser (client-side)           | Server (server-side)           |
| Contains          | Session ID (a reference)        | Actual user data               |
| Security          | Can be seen by user             | Hidden from user               |
| Size limit        | ~4KB                            | No fixed limit                 |
| Example           | `sessionId=abc123`              | `{ userId: 45, role: admin }`  |


## 10. Session-Based Authentication

> How the server knows **who you are** after you log in.

### Step 1 — First Request (Login):

```
Browser → POST /login → Server

Server does:
  1. Validates your username + password
  2. Creates a session (stores data on server)
  3. Sends back a cookie: sessionId=abc123
```

### Step 2 — Subsequent Requests:

```
Browser → GET /dashboard → Server
(Automatically sends: Cookie: sessionId=abc123)

Server does:
  1. Reads the sessionId from cookie
  2. Finds session data → { userId: 45, role: admin }
  3. Confirms: "This is Tushar, an admin"
  4. Grants access ✅
```

### The Full Auth Flow:

```
User logs in
     |
     ▼
Backend validates credentials
     |
     ▼
Session created on server (stores userId, role, etc.)
     |
     ▼
Cookie with sessionId sent to browser
     |
     ▼
Browser stores cookie
     |
     ▼
Next request → browser sends cookie
     |
     ▼
Server reads cookie → finds session → grants access ✅
```


## 11. Complete Full Stack Flow

> Here's how **all layers work together** from start to finish.

```
User does something (clicks, types, submits)
             |
             ▼
       Browser (Client)
             |
             ▼
       Frontend (React / HTML)
             |  sends API request
             ▼
       API Layer (Express route)
             |
             ▼
       Rate Limiter ── too many? → 429 Error
             |
             ▼
       Backend (Node.js logic)
             |
             ▼
       Cache ─── data found? → return fast
             |
             ▼ (if cache miss)
       Database (MongoDB / PostgreSQL)
             |
             ▼
       Backend prepares response
             |
             ▼
       API sends response to Frontend
             |
             ▼
       Frontend updates the UI
             |
             ▼
       User sees the result ✅
```


## 12. Real-World Example — Instagram Login

Let's trace a real login on Instagram:

| Step | What Happens                                          | Layer        |
|------|-------------------------------------------------------|--------------|
| 1    | You type your username and password                   | User         |
| 2    | Instagram app sends `POST /login`                     | Frontend/API |
| 3    | Rate limiter checks: is this the 6th attempt?         | Security     |
| 4    | Backend verifies credentials against the database     | Backend + DB |
| 5    | If valid, a session is created on Instagram's server  | Backend      |
| 6    | A cookie (session ID) is saved in your browser/app   | Browser      |
| 7    | You're redirected to your home feed                   | Frontend     |
| 8    | Every future request sends the cookie automatically   | Browser      |
| 9    | Server reads cookie → confirms it's you → shows data  | Backend      |


## 13. Key Concepts Summary

| Concept          | Layer               | One-line Meaning                              |
|------------------|---------------------|-----------------------------------------------|
| **Frontend**     | Presentation Layer  | What the user sees and clicks                 |
| **Backend**      | Logic Layer         | Processes requests, applies rules             |
| **API**          | Communication Layer | Messenger between frontend and backend        |
| **Database**     | Data Layer          | Stores all data permanently                   |
| **Cache**        | Performance Layer   | Temporary fast storage for frequent data      |
| **Rate Limiting**| Security Layer      | Limits how many requests a user can make      |
| **Cookies**      | Client Storage      | Small data stored in the user's browser       |
| **Sessions**     | Server Storage      | User info stored on the server                |


## 14. Glossary

| Term             | Meaning                                            |
|------------------|----------------------------------------------------|
| **User**         | Person interacting with the application            |
| **Browser**      | Software used to access the web (Chrome, Firefox)  |
| **Frontend**     | The visual part of the application                 |
| **Backend**      | Logic and processing layer of the app              |
| **API**          | Bridge/messenger between frontend and backend      |
| **Database**     | Storage system for permanent data                  |
| **Cache**        | Temporary fast storage to improve speed            |
| **Rate Limiting**| Controls the number of requests per time unit      |
| **Cookie**       | Small data stored in the browser                   |
| **Session**      | User data stored on the server                     |
| **Server**       | A machine that processes requests and sends back responses |
| **Client**       | The browser / app that the user uses               |
| **HTTP**         | Protocol used to communicate on the web            |
| **Request**      | Message sent from client to server                 |
| **Response**     | Message sent back from server to client            |


## 15. Quick Revision

> ⚡ Read this before your exam or interview!

### 🔑 One-Line Definitions

```
Frontend      = What the user sees (HTML, CSS, React)
Backend       = Logic layer (Node.js, Express, Django)
API           = Messenger between frontend & backend
Database      = Stores everything permanently
Cache         = Temporary fast storage (Redis)
Rate Limiting = Limits requests to protect server
Cookie        = Small data stored in the browser
Session       = User info stored on the server
Server        = Machine that processes requests
```

### ✅ Quick True/False Check

- Frontend is what users see → **True** ✅
- Backend stores data permanently → **False** ❌ (that's the Database)
- API connects frontend and backend → **True** ✅
- Cache makes apps slower → **False** ❌ (it makes apps faster)
- Cookies are stored on the server → **False** ❌ (stored in browser)
- Sessions are stored on the server → **True** ✅
- Rate limiting prevents DDoS attacks → **True** ✅
- Redis is used for caching → **True** ✅

### 🧠 Full Stack Architecture (memorise this!):

```
User
 ↓
Browser
 ↓
Frontend
 ↓
API Layer
 ↓
Rate Limiter → (blocks bad requests)
 ↓
Backend
 ↓
Cache → (returns fast if data is cached)
 ↓
Database → (fetches data if not cached)
 ↓
Response flows back up ↑
 ↓
User sees result ✅
```

### 📝 HTTP Status Codes to Remember:

| Code | Meaning                |
|------|------------------------|
| 200  | ✅ OK — Success         |
| 201  | ✅ Created              |
| 400  | ❌ Bad Request          |
| 401  | ❌ Unauthorized (not logged in) |
| 403  | ❌ Forbidden (no permission)    |
| 404  | ❌ Not Found            |
| 429  | ❌ Too Many Requests (rate limited) |
| 500  | ❌ Internal Server Error |

---

> 💬 **Remember:** Full stack = all the layers working together. You are learning to understand and build each one! 🚀

