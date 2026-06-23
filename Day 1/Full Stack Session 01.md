# Full Stack Basics: Beginner Notes 

## 1. What Happens When a User Interacts With a Website?

When a user interacts with a website, a complete system works behind the scenes to process that action. This system is called **full stack architecture**, and it ensures that whatever the user does (like clicking a button or submitting a form) is properly handled and responded to.

Imagine you open a website and click a "Login" button. That simple action triggers a chain of events across multiple layers of the system.

### Request Flow (Architecture View)

```text
User → Browser (Client) → Frontend (UI Layer) → API Layer → Backend (Application Layer) → Database (Data Layer)
```

Here’s what happens step by step:

* The **user** performs an action (like clicking or typing).
* The **browser** captures that action and sends it forward.
* The **frontend** prepares the request.
* The **API layer** acts as a messenger.
* The **backend** processes the request.
* The **database** stores or retrieves data.

### Response Flow

```text
Database → Backend → API Layer → Frontend → Browser → User
```

After processing:

* The database sends data back to the backend.
* The backend prepares a response.
* The API sends it to the frontend.
* The frontend updates the UI.
* The browser displays the result to the user.

---

## 2. Full Stack Architecture Overview

A full stack application is organized into layers, where each layer has a specific responsibility. This layered structure helps keep the system organized and scalable.

```text
[ User ]
   ↓
[ Client / Browser ]
   ↓
[ Frontend (Presentation Layer) ]
   ↓
[ API Layer (Communication Layer) ]
   ↓
[ Backend (Application Logic Layer) ]
   ↓
[ Database (Data Storage Layer) ]
```

Think of it like a restaurant:

* User = Customer
* Frontend = Menu & waiter
* Backend = Kitchen
* Database = Storage room

---

## 3. Components of Full Stack Architecture

### 1. User

The user is the person interacting with the application.

Examples:

* Clicking buttons
* Filling out forms
* Searching for products
* Logging in or signing up

The entire system exists to serve the user’s actions.

---

### 2. Browser / Client Layer

The browser is the tool the user uses to access the application.

Examples:

* Chrome
* Firefox
* Safari

Responsibilities:

* Displays the website visually
* Runs frontend code (JavaScript)
* Sends requests to the server
* Stores small pieces of data like cookies

---

### 3. Frontend (Presentation Layer)

The frontend is what the user sees and interacts with directly.

Responsibilities:

* Displaying content (text, images, buttons)
* Handling user input (clicks, typing)
* Sending requests to the backend via APIs
* Updating the UI based on responses

Technologies:

```text
HTML, CSS, JavaScript
React, Next.js, Angular
Tailwind CSS
```

Example: When you click "Add to Cart", the frontend captures that action and sends a request.

---

### 4. API Layer (Communication Layer)

The API layer acts like a messenger between the frontend and backend.

Responsibilities:

* Receives requests from the frontend
* Sends them to the backend
* Returns responses back to the frontend

Example endpoints:

```text
POST /login
GET /products
```

Think of API as a waiter taking your order to the kitchen and bringing back the food.

---

### 5. Backend (Application Layer)

The backend is where the main logic of the application lives.

Responsibilities:

* Processing requests
* Validating user input
* Handling authentication and authorization
* Communicating with the database
* Applying business rules

Technologies:

```text
Node.js, Express.js, NestJS
Django, FastAPI, Spring Boot
```

Example: Checking if a username and password are correct.

---

### 6. Database (Data Layer)

The database stores all important data permanently.

Examples:

* User accounts
* Orders
* Products
* Messages

Technologies:

```text
MongoDB, PostgreSQL, MySQL, SQLite, Redis
```

The database ensures that data is saved and can be retrieved later.

---

## 4. Example Architecture: Login Flow

### Step-by-Step Flow

```text
User → Browser → Frontend → API → Backend → Database
```

### Detailed Steps

1. The user enters their username and password.
2. The frontend sends a request:

   ```text
   POST /login
   ```
3. The backend receives the request and checks the credentials.
4. The backend queries the database to verify the user.
5. If the credentials are correct:

   * A session is created on the server
   * A cookie is sent to the browser
6. The browser stores the cookie.
7. For future requests, the browser automatically sends the cookie.

This allows the system to remember that the user is logged in.

---

## 5. Server in Architecture

A **server** is a machine or system that processes incoming requests and sends back responses.

```text
Client → Server → Response
```

Responsibilities:

* Receiving API requests
* Running backend logic
* Communicating with the database
* Sending responses back to the client

Think of the server as the brain of the application.

---

## 6. Rate Limiting (Security Layer)

Rate limiting controls how many requests a user can send in a given time.

Example:

```text
100 requests per minute
```

### Architecture Placement

```text
Client → API Gateway → Rate Limiter → Backend
```

It acts as a protective layer before requests reach the backend.

---

## 7. Why Rate Limiting is Important

Rate limiting helps protect the system from misuse.

It prevents:

* DDoS attacks (flooding the server with requests)
* Bots making too many requests
* Spam actions
* Server overload

Without rate limiting, the system could crash under heavy traffic.

---

## 8. Rate Limiting Example

```text
POST /login
Limit: 5 attempts per minute
```

If the limit is exceeded:

```text
429 Too Many Requests
```

This ensures users cannot repeatedly try passwords too quickly.

---

## 9. Cache (Performance Layer)

Cache is a temporary storage that keeps frequently used data for faster access.

### Without Cache

```text
User → Server → Database → Server → User
```

Every request goes to the database, which can be slow.

### With Cache

```text
User → Server → Cache → User
```

If data is already in cache, it is returned instantly.

---

## 10. Cache Hit vs Cache Miss

### Cache Hit

```text
Request → Cache → Response (Fast)
```

The requested data is found in cache.

### Cache Miss

```text
Request → Cache (Not Found) → Database → Cache → Response
```

The data is not in cache, so it is fetched from the database and stored in cache for future use.

---

## 11. Benefits of Cache

* Faster response times
* Reduced load on the database
* Improved scalability
* Better user experience

Tool:

```text
Redis
```

---

## 12. Cookies (Client Storage Layer)

Cookies are small pieces of data stored in the browser.

Example:

```text
sessionId=abc123
```

Used for:

* Keeping users logged in
* Tracking sessions
* Storing small preferences

Cookies are automatically sent with every request to the server.

---

## 13. Sessions (Server Storage Layer)

Sessions store user-related data on the server.

Example:

```text
sessionId: abc123
userId: 45
role: admin
```

The session ID is stored in the cookie, and the actual data is stored on the server.

---

## 14. Session-Based Authentication Architecture

### First Request

```text
Browser → Server
POST /login
```

Server actions:

* Validates user credentials
* Creates a session
* Sends a cookie with session ID

---

### Subsequent Requests

```text
Browser → Server
Cookie: sessionId=abc123
```

Server actions:

* Reads the session ID
* Finds session data
* Confirms user identity
* Grants access

---

## 15. Complete Full Stack Architecture Flow

```text
User Action
   ↓
Browser (Client)
   ↓
Frontend (UI Layer)
   ↓
API Layer
   ↓
Backend (Logic Layer)
   ↓
Cache / Database (Data Layer)
   ↓
Backend Response
   ↓
Frontend Update
   ↓
User Sees Result
```

This flow shows how every layer works together to deliver a smooth experience.

---

## 16. Real-World Example (Instagram Login)

1. User enters username and password.
2. Frontend sends login request.
3. Backend checks credentials in the database.
4. If valid, a session is created.
5. A cookie is stored in the browser.
6. The user remains logged in for future actions.

---

## 17. Key Architecture Concepts

* **Frontend** = UI Layer (what users see)
* **Backend** = Logic Layer (how things work)
* **API** = Communication Layer (connects frontend and backend)
* **Database** = Data Layer (stores information)
* **Cache** = Performance Layer (speeds up access)
* **Rate Limiting** = Security Layer (controls request flow)
* **Cookies** = Client Storage (stored in browser)
* **Sessions** = Server Storage (stored on server)

---

## 18. Glossary

| Term          | Meaning                                 |
| ------------- | --------------------------------------- |
| User          | Person interacting with the application |
| Browser       | Software used to access the web         |
| Frontend      | Visual part of the application          |
| Backend       | Logic and processing layer              |
| API           | Bridge between frontend and backend     |
| Database      | Storage system for persistent data      |
| Cache         | Temporary fast storage                  |
| Rate Limiting | Controls number of requests             |
| Cookie        | Small data stored in browser            |
| Session       | User data stored on server              |

---

## 19. Quick Revision

```text
Frontend = UI Layer (what user sees)
Backend = Logic Layer (how system works)
API = Communication Layer (connects layers)
Database = Persistent Storage (stores data)
Cache = Fast Storage (improves speed)
Rate Limiting = Security Control (limits requests)
Cookie = Browser Data (stored on client)
Session = Server Data (stored on server)
```
