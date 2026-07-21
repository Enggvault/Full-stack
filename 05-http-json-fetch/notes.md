title: "HTTP, JSON & Fetch: Complete Beginner to Advanced"
subtitle: "From First Principles to Production-Grade Architecture"
author: "Principal Backend Engineer — 15+ Years Industry Experience"
version: "2.0"
date: "2025"

# HTTP, JSON & Fetch
## Complete Beginner to Advanced Engineering Handbook

> A production-grade, book-quality reference covering HTTP protocols, JSON, Fetch API, Promises, and async/await. Written for engineers at all levels — from beginners to FAANG system designers.

> **Prerequisites:** [04 — JavaScript ←](../04-javascript/notes.md) · **Next:** [06 — API Design →](../06-api-design/notes.md)


## Table of Contents

### Part I: HTTP Foundations
- [Chapter 1: HTTP in Depth](#chapter-1-http-in-depth)
- [Chapter 2: HTTP Headers](#chapter-2-http-headers)
- [Chapter 3: HTTP Status Codes](#chapter-3-http-status-codes)

### Part II: Data & API Communication
- [Chapter 4: JSON](#chapter-4-json)
- [Chapter 5: The Fetch API](#chapter-5-the-fetch-api)
- [Chapter 6: Sending Data with Fetch](#chapter-6-sending-data-with-fetch)
- [Chapter 7: Fetch with Authentication](#chapter-7-fetch-with-authentication)
- [Chapter 8: Error Handling](#chapter-8-error-handling)

### Part III: Async Patterns
- [Chapter 9: Promises in Depth](#chapter-9-promises-in-depth)
- [Chapter 10: async/await in Depth](#chapter-10-asyncawait-in-depth)
- [Chapter 11: Parallel Async Operations](#chapter-11-parallel-async-operations)
- [Chapter 12: Practical CRUD Pattern](#chapter-12-practical-crud-pattern)

### Part IV: Tools & Reference
- [Chapter 13: Reading the Network Tab](#chapter-13-reading-the-network-tab)


# CHAPTER 1: HTTP in Depth

**HTTP (HyperText Transfer Protocol)** is the application-layer protocol that governs how clients (browsers, apps) and servers exchange data. Every API call is an HTTP transaction.

### Anatomy of an HTTP Request

```
POST /api/v1/orders HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer eyJhbGc...
Accept: application/json

{
  "productId": "prod_123",
  "quantity": 2
}
```

| Part | Description |
|:-----|:------------|
| **Method** | The action to perform (`POST`) |
| **Path** | The resource being addressed (`/api/v1/orders`) |
| **HTTP version** | Protocol version (`HTTP/1.1` or `HTTP/2`) |
| **Headers** | Key-value metadata about the request |
| **Body** | Optional payload — used by POST, PUT, PATCH |

### Anatomy of an HTTP Response

```
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/v1/orders/ord_9981

{
  "orderId": "ord_9981",
  "status": "pending"
}
```

### HTTP Methods — Full Reference

| Method | Semantic Meaning | Has Body | Safe | Idempotent |
|:-------|:----------------|:--------:|:----:|:----------:|
| `GET` | Read a resource | ✗ | ✓ | ✓ |
| `POST` | Create a resource | ✓ | ✗ | ✗ |
| `PUT` | Replace a resource (entire representation) | ✓ | ✗ | ✓ |
| `PATCH` | Partially update a resource | ✓ | ✗ | Usually |
| `DELETE` | Remove a resource | Optional | ✗ | ✓ |
| `HEAD` | Same as GET, but response body is omitted | ✗ | ✓ | ✓ |
| `OPTIONS` | Describe communication options for the target resource | ✗ | ✓ | ✓ |

**Safe** — does not modify server state.
**Idempotent** — multiple identical requests produce the same result as one.


# CHAPTER 2: HTTP Headers

Headers are key-value metadata attached to both requests and responses.

### Common Request Headers

| Header | Example Value | Purpose |
|:-------|:-------------|:--------|
| `Content-Type` | `application/json` | Format of the request body |
| `Accept` | `application/json` | Format the client expects in the response |
| `Authorization` | `Bearer <token>` | Authentication credential |
| `Cookie` | `sessionId=abc123` | Session or tracking cookies |
| `User-Agent` | `Mozilla/5.0 ...` | Browser / client identification |
| `Origin` | `https://app.example.com` | Origin of the cross-origin request (CORS) |

### Common Response Headers

| Header | Example Value | Purpose |
|:-------|:-------------|:--------|
| `Content-Type` | `application/json; charset=utf-8` | Format of the response body |
| `Cache-Control` | `max-age=3600, public` | Caching directives |
| `Location` | `/api/v1/orders/ord_9981` | URL of a newly created resource |
| `Set-Cookie` | `sessionId=abc; HttpOnly; Secure` | Sets a browser cookie |
| `X-RateLimit-Remaining` | `42` | Requests remaining in the rate limit window |
| `Access-Control-Allow-Origin` | `https://app.example.com` | CORS policy |


# CHAPTER 3: HTTP Status Codes

| Code | Name | Meaning |
|:-----|:-----|:--------|
| **200** | OK | Request succeeded |
| **201** | Created | Resource created successfully — typically follows POST |
| **204** | No Content | Success with no response body — typically follows DELETE |
| **301** | Moved Permanently | Permanent redirect |
| **304** | Not Modified | Cached version is still valid |
| **400** | Bad Request | Client sent malformed or invalid data |
| **401** | Unauthorized | Authentication required or credentials invalid |
| **403** | Forbidden | Authenticated but not authorised for this resource |
| **404** | Not Found | Resource does not exist |
| **409** | Conflict | Request conflicts with current state (e.g., duplicate email) |
| **422** | Unprocessable Entity | Semantically invalid request (validation failed) |
| **429** | Too Many Requests | Rate limit exceeded |
| **500** | Internal Server Error | Unexpected server-side failure |
| **502** | Bad Gateway | Upstream server returned an invalid response |
| **503** | Service Unavailable | Server is temporarily unable to handle requests |


# CHAPTER 4: JSON

**JSON (JavaScript Object Notation)** is the universal data format for web APIs. It is purely text-based and language-agnostic.

### JSON Syntax Rules

1. Keys **must** be double-quoted strings.
2. Values may be: `string`, `number`, `boolean`, `null`, `array`, or `object`.
3. No comments, no trailing commas, no functions.

```json
{
  "userId": 42,
  "name": "Tushar",
  "email": "thetushardev0@gmail.com",
  "isVerified": true,
  "score": 9.5,
  "tags": ["admin", "beta-tester"],
  "address": {
    "city": "Kolkata",
    "country": "India"
  },
  "deletedAt": null
}
```

### JSON ↔ JavaScript

```js
// JavaScript object → JSON string (for sending in a request body)
const payload = { name: 'Alice', role: 'admin' };
const json = JSON.stringify(payload);
// '{"name":"Alice","role":"admin"}'

// Pretty-print for logging
console.log(JSON.stringify(payload, null, 2));

// JSON string → JavaScript object (from a response body)
const obj = JSON.parse('{"name":"Alice"}');
// { name: 'Alice' }
```

> `response.json()` in the Fetch API does `JSON.parse` automatically — it is not needed manually when using Fetch.


# CHAPTER 5: The Fetch API

`fetch()` is the browser-native API for making HTTP requests. It returns a **Promise** that resolves to a `Response` object.

### Basic GET Request

```js
const response = await fetch('https://api.example.com/users');

// The Promise resolves as soon as the response headers arrive — even for 4xx/5xx.
// A fetch() Promise only rejects on network failure (no connection, DNS failure).
if (!response.ok) {
  throw new Error(`HTTP error: ${response.status}`);
}

const users = await response.json(); // Parses the body as JSON
console.log(users);
```

### Reading Response Metadata

```js
const response = await fetch('/api/resource');

response.status;      // 200
response.statusText;  // 'OK'
response.ok;          // true if status is 200–299
response.headers.get('Content-Type'); // 'application/json; charset=utf-8'
response.url;         // Final URL (after redirects)
```

### Response Body Methods (each can only be called once)

| Method | Returns | Use When |
|:-------|:--------|:---------|
| `response.json()` | Promise → JS object | Server returns JSON |
| `response.text()` | Promise → string | Server returns plain text or HTML |
| `response.blob()` | Promise → Blob | Server returns binary data (images, files) |
| `response.arrayBuffer()` | Promise → ArrayBuffer | Low-level binary processing |


# CHAPTER 6: Sending Data with Fetch

### POST — Create a Resource

```js
const response = await fetch('/api/v1/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Accept': 'application/json',
  },
  body: JSON.stringify({
    name: 'Alice',
    email: 'alice@example.com',
  }),
});

if (!response.ok) {
  const error = await response.json();
  throw new Error(error.message ?? 'Failed to create user');
}

const user = await response.json();
console.log('Created:', user);
```

### PUT — Replace a Resource

```js
const response = await fetch(`/api/v1/users/${userId}`, {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'Alice Smith', email: 'alice@example.com', role: 'admin' }),
});
```

### PATCH — Partial Update

```js
const response = await fetch(`/api/v1/users/${userId}`, {
  method: 'PATCH',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'Alice Smith' }), // Only the changed field
});
```

### DELETE — Remove a Resource

```js
const response = await fetch(`/api/v1/users/${userId}`, {
  method: 'DELETE',
});

if (response.status !== 204) {
  throw new Error('Delete failed');
}
// No body to parse — 204 No Content
```


# CHAPTER 7: Fetch with Authentication

### Bearer Token (JWT)

```js
const token = localStorage.getItem('auth_token');

const response = await fetch('/api/v1/profile', {
  headers: {
    'Authorization': `Bearer ${token}`,
    'Accept': 'application/json',
  },
});
```

### With Cookies (Session-based Auth)

```js
const response = await fetch('/api/v1/profile', {
  credentials: 'include', // Send cookies cross-origin
});
```

| `credentials` value | Behavior |
|:--------------------|:---------|
| `omit` | Never send cookies |
| `same-origin` | Send cookies only to the same origin (default) |
| `include` | Always send cookies, including cross-origin |


# CHAPTER 8: Error Handling

`fetch()` **only** rejects the Promise on a network-level failure. HTTP 4xx and 5xx responses resolve the Promise — they must be checked manually.

### Robust Error Handler

```js
async function apiFetch(url, options = {}) {
  let response;

  try {
    response = await fetch(url, options);
  } catch (networkError) {
    // No internet connection, DNS resolution failure, etc.
    throw new Error('Network unavailable. Check your connection.');
  }

  if (!response.ok) {
    let errorMessage = `HTTP ${response.status}`;
    try {
      const body = await response.json();
      errorMessage = body.message ?? body.error ?? errorMessage;
    } catch {
      // Body was not JSON — use status code message
    }
    throw new Error(errorMessage);
  }

  // 204 No Content — no body to parse
  if (response.status === 204) return null;

  return response.json();
}

// Usage
try {
  const user = await apiFetch(`/api/v1/users/${id}`);
  renderUser(user);
} catch (err) {
  showError(err.message);
}
```


# CHAPTER 9: Promises in Depth

A **Promise** represents the eventual result of an asynchronous operation.

### Promise States

```
pending  →  fulfilled  (resolved with a value)
         →  rejected   (resolved with a reason/error)
```

Once settled, a Promise's state cannot change.

### Creating a Promise

```js
const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

const fetchWithTimeout = (url, ms) =>
  new Promise((resolve, reject) => {
    const timer = setTimeout(() => reject(new Error('Request timed out')), ms);
    fetch(url)
      .then((res) => { clearTimeout(timer); resolve(res); })
      .catch(reject);
  });
```

### Promise Chaining

Each `.then()` receives the return value of the previous handler. Errors fall through to the nearest `.catch()`.

```js
fetch('/api/users')
  .then(res => {
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    return res.json();
  })
  .then(users => users.filter(u => u.isActive))
  .then(activeUsers => renderList(activeUsers))
  .catch(err => showError(err.message))
  .finally(() => hideSpinner());
```

### Promise Combinators

| Method | Behaviour |
|:-------|:----------|
| `Promise.all(promises)` | Resolves when **all** settle successfully; rejects if **any** reject |
| `Promise.allSettled(promises)` | Resolves when **all** settle; never rejects; returns status + value/reason for each |
| `Promise.race(promises)` | Resolves/rejects with the **first** settled Promise |
| `Promise.any(promises)` | Resolves with the **first fulfilled** Promise; rejects if all reject |


# CHAPTER 10: async/await in Depth

`async`/`await` is syntactic sugar over Promises. An `async` function always returns a Promise.

```js
async function getUser(id) {
  // await suspends this function — not the main thread
  const response = await fetch(`/api/users/${id}`);
  if (!response.ok) throw new Error(`User ${id} not found`);
  return response.json(); // Returns a Promise that resolves to the parsed object
}

// Top-level await (ES2022, works in modules)
const user = await getUser(1);
```

### Async Flow Diagram

```
getUser(id) is called
      │
      ▼
 await fetch(url)       ← function suspends, control returns to caller
      │
      │  (network request happens in the background)
      │
      ▼
 Response headers received — function resumes
      │
      ▼
 await response.json()  ← suspends again to parse body
      │
      ▼
 Returns parsed JavaScript object
```

### Common Async Patterns

```js
// Sequential — second waits for first
const user   = await fetchUser(id);
const orders = await fetchOrders(user.id); // Depends on user.id

// Parallel — both start simultaneously
const [user, settings] = await Promise.all([
  fetchUser(id),       // Does not depend on settings
  fetchSettings(id),   // Does not depend on user
]);

// Error boundary — catch in the calling context
async function load() {
  try {
    const data = await apiFetch('/api/data');
    render(data);
  } catch (err) {
    reportError(err);
    showFallback();
  }
}
```


# CHAPTER 11: Parallel Async Operations

When multiple async operations do not depend on each other, run them in parallel.

```js
// ❌ Sequential — unnecessarily slow
const users    = await fetchUsers();   // 500ms
const products = await fetchProducts(); // 500ms
// Total: ~1000ms

// ✓ Parallel — concurrent
const [users, products] = await Promise.all([
  fetchUsers(),    // 500ms
  fetchProducts(), // 500ms
]);
// Total: ~500ms

// When some may fail — use allSettled
const results = await Promise.allSettled([
  fetchUsers(),
  fetchProducts(),
  fetchSettings(),
]);

results.forEach(result => {
  if (result.status === 'fulfilled') {
    console.log('Success:', result.value);
  } else {
    console.error('Failed:', result.reason);
  }
});
```


# CHAPTER 12: Practical CRUD Pattern

A reusable client-side API layer encapsulates all network logic in one place.

```js
// api/client.js
const BASE_URL = 'https://api.example.com/v1';

async function request(path, options = {}) {
  const token = localStorage.getItem('token');

  const response = await fetch(`${BASE_URL}${path}`, {
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json',
      ...(token && { Authorization: `Bearer ${token}` }),
      ...options.headers,
    },
    ...options,
  });

  if (response.status === 204) return null;
  if (!response.ok) {
    const err = await response.json().catch(() => ({}));
    throw new Error(err.message ?? `HTTP ${response.status}`);
  }
  return response.json();
}

// api/users.js
export const usersApi = {
  getAll: ()       => request('/users'),
  getById: (id)    => request(`/users/${id}`),
  create: (data)   => request('/users', { method: 'POST', body: JSON.stringify(data) }),
  update: (id, d)  => request(`/users/${id}`, { method: 'PATCH', body: JSON.stringify(d) }),
  remove: (id)     => request(`/users/${id}`, { method: 'DELETE' }),
};

// Usage in a component
const users = await usersApi.getAll();
const newUser = await usersApi.create({ name: 'Bob', email: 'bob@example.com' });
await usersApi.remove(42);
```


# CHAPTER 13: Reading the Network Tab

The browser **DevTools → Network tab** is the primary tool for debugging HTTP requests.

**How to use it:**

1. Open DevTools (`F12` or `Ctrl+Shift+I`).
2. Select the **Network** tab.
3. Reload the page or trigger the action.
4. Click any request row.

**What to inspect:**

| Panel | Shows |
|:------|:------|
| **Headers** | Request method, URL, status code, request and response headers |
| **Payload** | The request body (form data or JSON) |
| **Preview** | Formatted response body |
| **Response** | Raw response body |
| **Timing** | DNS lookup, TCP connection, time to first byte, content download |

**Filter by type:**

- `Fetch/XHR` — shows only API calls made by JavaScript
- `Doc` — shows the main HTML document
- `Img` / `Media` / `Font` — static assets

**Common debugging checks:**
- Is the request reaching the correct URL?
- Are the correct headers (`Authorization`, `Content-Type`) being sent?
- Is the response body what the server actually returned?
- Is the status code what you expect?


> **Next:** [06 — API Design →](../06-api-design/notes.md)
