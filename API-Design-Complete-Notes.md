# The Complete Guide to API Design: From Basic to Advanced

Welcome to the definitive guide on Application Programming Interface (API) design. This handbook is designed for developers of all levels to understand, design, and build robust, scalable, and secure APIs for modern web applications.


## 1. Introduction to APIs

### What is an API?
An **API (Application Programming Interface)** is a set of rules, protocols, and tools that allows different software applications to communicate with each other. Think of it as a waiter in a restaurant. You (the client) give your order (request) to the waiter (the API), who takes it to the kitchen (the server). The kitchen prepares the food (data), and the waiter brings it back to you (response).

### Why APIs are Important
* **Integration**: Allows different systems to work together seamlessly.
* **Abstraction**: Hides complex backend logic from the frontend.
* **Reusability**: Core functionality can be built once and consumed by mobile apps, web apps, and third-party services.
* **Scalability**: Decouples the client architecture from the server architecture.

### How APIs Work
APIs rely on a standardized method of communication. For web APIs, this is primarily HTTP (Hypertext Transfer Protocol).

#### Client-Server Architecture
The architecture involves a **Client** (e.g., a web browser, a mobile app, or another server) that requests resources, and a **Server** that holds the resources and processes the requests.

```ascii
+-------------+         Request          +-------------+
|             | -----------------------> |             |
|   Client    |                          |   Server    |
| (Web/Mobile)| <----------------------- | (Database/  |
|             |         Response         |  Logic)     |
+-------------+                          +-------------+
```

#### Request-Response Cycle
1. The **Client** sends an HTTP request to an API endpoint.
2. The **Server** receives the request, processes the data, validates permissions, and queries the database.
3. The **Server** sends back an HTTP response containing a status code and the requested data (usually in JSON format).

#### API Lifecycle
1. **Design**: Planning endpoints, payloads, and behavior (API-First Development).
2. **Develop**: Writing the code to implement the design.
3. **Test**: Verifying security, functionality, and performance.
4. **Deploy**: Releasing the API to a production environment.
5. **Monitor & Manage**: Tracking usage, errors, and rate limits.
6. **Retire**: Deprecating older versions safely.

#### API-first Development
API-first development means designing the API contract (like an OpenAPI spec) before writing any code. This allows frontend and backend teams to work in parallel.


## 2. API History and Evolution

APIs have evolved significantly over the years to meet the demands of changing software architectures.

| Paradigm | Concept | Data Format | Pros | Cons |
| :--- | :--- | :--- | :--- | :--- |
| **RPC (Remote Procedure Call)** | Execute code on a remote server as if it were local. | XML/JSON | Simple, action-oriented. | Tight coupling, lack of standardization. |
| **SOAP (Simple Object Access Protocol)** | Highly structured, strictly typed messaging protocol. | XML | Secure, ACID compliant, reliable. | Heavyweight, hard to parse, verbose. |
| **REST (Representational State Transfer)** | Resource-based architecture using standard HTTP methods. | JSON/XML | Scalable, stateless, cacheable, standard. | Over/under-fetching data. |
| **GraphQL** | Query language allowing clients to ask for exactly what they need. | JSON | No over-fetching, highly flexible. | Complex caching, steep learning curve. |
| **gRPC** | High-performance RPC framework developed by Google. | Protobuf | Very fast, bidirectional streaming. | Not natively supported by web browsers. |
| **WebSockets** | Persistent, bidirectional communication channel. | Binary/Text | Real-time, low latency. | Stateful, harder to scale. |
| **SSE (Server-Sent Events)** | Unidirectional event stream from server to client. | Text | Simple real-time updates. | Server-to-client only. |

## 3. HTTP Fundamentals

Understanding HTTP is critical for API design, as REST APIs are built entirely on top of it.

### Core Concepts
* **HTTP (Hypertext Transfer Protocol)**: The foundation of data communication on the Web.
* **HTTPS**: HTTP over SSL/TLS for encrypted, secure communication.
* **URL (Uniform Resource Locator)**: The address of a web resource.
* **URI (Uniform Resource Identifier)**: A broader term; all URLs are URIs.
* **Endpoint**: A specific URL where an API can be accessed (e.g., `/api/v1/users`).
* **Resource**: The object or representation of data you are interacting with (e.g., a User).

### HTTP Request Structure
1. **Method**: Defines the action (GET, POST, etc.).
2. **URI**: The target resource path.
3. **Headers**: Metadata about the request (e.g., `Authorization: Bearer <token>`, `Content-Type: application/json`).
4. **Body**: Data sent with the request (used in POST/PUT/PATCH).
5. **Query Parameters**: Key-value pairs in the URL (`?sort=desc&limit=10`) for filtering/sorting.
6. **Path Parameters**: Variables within the URL path (`/users/123` where `123` is the parameter).
7. **Cookies/Sessions**: Used for state management, though less common in stateless REST APIs.

### HTTP Response Structure
1. **Status Code**: Indicates the result of the request (e.g., 200 OK).
2. **Headers**: Metadata about the response (e.g., `Content-Type: application/json`).
3. **Body**: The requested data or error details.


## 4. HTTP Methods

HTTP methods define the action to be performed on a resource.

### GET
* **Purpose**: Retrieve a resource or a list of resources.
* **Idempotency**: Yes (Calling it multiple times has the same result).
* **Safe**: Yes (Does not modify data).
* **Example Request**: `GET /api/users/123`
* **Example Response**: `{ "id": 123, "name": "John Doe" }`

### POST
* **Purpose**: Create a new resource.
* **Idempotency**: No (Calling it twice creates two resources).
* **Safe**: No.
* **Example Request**: `POST /api/users` with body `{ "name": "Jane Doe" }`
* **Example Response**: `201 Created` with body `{ "id": 124, "name": "Jane Doe" }`

### PUT
* **Purpose**: Replace an existing resource entirely (or create if it doesn't exist).
* **Idempotency**: Yes.
* **Safe**: No.
* **Example Request**: `PUT /api/users/123` with body `{ "name": "John Smith", "age": 30 }`

### PATCH
* **Purpose**: Partially update an existing resource.
* **Idempotency**: Usually yes, but not guaranteed.
* **Safe**: No.
* **Example Request**: `PATCH /api/users/123` with body `{ "age": 31 }`

### DELETE
* **Purpose**: Delete a resource.
* **Idempotency**: Yes.
* **Safe**: No.
* **Example Request**: `DELETE /api/users/123`

### OPTIONS
* **Purpose**: Returns the HTTP methods supported by the server for a specific endpoint (used heavily in CORS).

### HEAD
* **Purpose**: Identical to GET, but returns only the headers without the response body. Useful for checking if a resource exists or its size.

---

## 5. HTTP Status Codes

Status codes inform the client about the outcome of the request.

### 1xx: Informational
Request received, continuing process.

### 2xx: Success
The action was successfully received, understood, and accepted.
* **200 OK**: Standard success for GET, PUT, PATCH.
* **201 Created**: Successful POST request that created a resource.
* **202 Accepted**: Request accepted for processing, but processing is not complete (e.g., background jobs).
* **204 No Content**: Successful request, but no data to return (common for DELETE).

### 3xx: Redirection
Further action must be taken to complete the request.
* **301 Moved Permanently**: Resource has a new permanent URI.
* **302 Found**: Resource resides temporarily under a different URI.
* **304 Not Modified**: Client's cached version is still valid.

### 4xx: Client Errors
The request contains bad syntax or cannot be fulfilled.
* **400 Bad Request**: Malformed syntax, validation error.
* **401 Unauthorized**: Authentication is required and has failed or has not yet been provided.
* **403 Forbidden**: Authenticated, but lacks permission to access the resource.
* **404 Not Found**: The resource could not be found.
* **405 Method Not Allowed**: Method specified is not allowed for the resource (e.g., POSTing to a read-only endpoint).
* **409 Conflict**: Request conflicts with the current state of the server (e.g., duplicate email creation).
* **415 Unsupported Media Type**: Payload format is in an unsupported format (e.g., sending XML when only JSON is accepted).
* **422 Unprocessable Entity**: Semantic errors, often used for data validation failures.
* **429 Too Many Requests**: Rate limit exceeded.

### 5xx: Server Errors
The server failed to fulfill an apparently valid request.
* **500 Internal Server Error**: Generic server error.
* **502 Bad Gateway**: Invalid response from an upstream server (Gateway/Proxy issue).
* **503 Service Unavailable**: Server is overloaded or down for maintenance.
* **504 Gateway Timeout**: Upstream server failed to send a request in the time allowed.

---

## 6. REST API Principles

REST (Representational State Transfer) is an architectural style, not a strict standard. To be truly RESTful, an API must adhere to these constraints:

1. **Client-Server Architecture**: Separation of concerns. The client handles UI, the server handles data.
2. **Statelessness**: Every request from the client must contain all the information needed to understand and process the request. The server stores no session state about the client.
3. **Cacheability**: Responses must define themselves as cacheable or not to prevent clients from reusing stale data.
4. **Uniform Interface**: A standard, uniform way of interacting with resources (using HTTP verbs consistently).
5. **Layered System**: A client cannot ordinarily tell whether it is connected directly to the end server or an intermediary (like a load balancer).
6. **Code on Demand (Optional)**: Servers can temporarily extend client functionality by transferring executable code (e.g., JavaScript).

### The Richardson Maturity Model
A model to measure how closely an API adheres to REST principles:
* **Level 0**: The Swamp of POX (Plain Old XML) - Using HTTP purely as a transport mechanism (like RPC).
* **Level 1**: Resources - Introducing the concept of individual resources (`/users`, `/orders`).
* **Level 2**: HTTP Verbs - Using GET, POST, PUT, DELETE properly alongside HTTP status codes.
* **Level 3**: Hypermedia Controls (HATEOAS) - Responses contain links to discover other related actions/resources dynamically.

---

## 7. Resource Naming

Resource naming is one of the most visible parts of API design.

### Best Practices
* **Use Nouns, Not Verbs**: The URI should represent an entity, while the HTTP method represents the action.
* **Use Plural Nouns**: It keeps the API consistent. `/users` is a collection; `/users/1` is an item in the collection.
* **Use lowercase and hyphens (kebab-case)**: Improves readability in URLs.

### Examples
**Good Endpoints:**
```text
GET /users
GET /users/123
POST /posts/10/comments
GET /company-reports
```

**Bad Endpoints:**
```text
GET /getUsers             (Uses verb)
POST /createUser          (Uses verb)
POST /users/123/delete    (Uses verb in path)
GET /UserList             (Mixed casing, not a clear resource)
GET /company_reports      (Uses underscores, hyphens are preferred in URLs)
```

---

## 8. API URL Design

### Base URL & Versioning
Your API should have a clear base URL, usually incorporating the API version.
```text
https://api.example.com/v1/
```

### Nested Resources and Relationships
When resources are logically nested, the URL structure should reflect that, but avoid nesting too deeply (maximum 2-3 levels).

* **Get all comments for a post**: `GET /posts/123/comments`
* **Get a specific comment on a post**: `GET /posts/123/comments/456`

If a resource can exist independently, prefer a flat structure:
Instead of `GET /authors/12/posts/34/comments`, use `GET /posts/34/comments`.

### Query Parameters for Modifiers
Use query parameters for operations that modify the collection without changing the resource itself.

* **Filtering**: `/users?role=admin&status=active`
* **Sorting**: `/users?sort=-createdAt` (descending) or `/users?sort=name` (ascending)
* **Pagination**: `/users?page=2&limit=20`
* **Searching**: `/users?q=John`

---

## 9. Request Design

### Path Parameters vs. Query Parameters
* **Path Parameters**: Used to identify a specific resource (`/users/:id`). Mandatory.
* **Query Parameters**: Used to filter, sort, or modify a collection (`/users?status=active`). Optional.

### Headers
Pass metadata in headers rather than the body.
* `Authorization`: For tokens.
* `Accept`: To specify expected response format (`application/json`).
* `Accept-Language`: For localization (`en-US`).

### Request Body
Use JSON for standard data transfer. Ensure the structure is flat where possible, grouped logically when complex.

```json
// POST /api/v1/users
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "preferences": {
    "newsletter": true,
    "theme": "dark"
  }
}
```

---

## 10. Response Design

A consistent response structure makes an API predictable and easy to consume.

### Standard Response Envelope
While some prefer returning plain JSON arrays/objects, a structured envelope is often better for metadata.

**Success Response:**
```json
{
  "success": true,
  "data": {
    "id": 123,
    "name": "Jane Doe"
  },
  "message": "User successfully retrieved."
}
```

### Pagination Metadata
When returning collections, include metadata to help the client navigate.
```json
{
  "success": true,
  "data": [ ... array of users ... ],
  "meta": {
    "page": 2,
    "limit": 20,
    "totalCount": 150,
    "totalPages": 8
  },
  "links": {
    "self": "/api/v1/users?page=2",
    "next": "/api/v1/users?page=3",
    "prev": "/api/v1/users?page=1"
  }
}
```

---

## 11. JSON Best Practices

* **Naming Conventions**: Use `camelCase` for JSON keys (standard in JS ecosystems) or `snake_case` (standard in Ruby/Python). Pick one and stick to it strictly.
* **Consistency**: A field named `userId` should not be `user_id` or `id_user` in a different endpoint.
* **Data Types**: Be strict. A boolean should be `true`/`false`, not `"true"`, `1`, or `0`. Dates should be ISO 8601 strings (`2023-10-25T14:30:00Z`).
* **Null Values**: Return `null` for missing scalar values rather than an empty string `""` or omitting the key entirely, unless omitting the key reduces payload size significantly and is documented.
* **Arrays**: Always return arrays for collections, even if empty `[]`. Do not return `null` when a list is requested but empty.

---

## 12. CRUD API Design

Here is a blueprint for standard CRUD operations.

### Resource: Products
| Action | HTTP Method | Endpoint | Request Body | Response Status |
| :--- | :--- | :--- | :--- | :--- |
| **Create** | POST | `/products` | `{ "name": "Laptop", "price": 999 }` | 201 Created |
| **Read All**| GET | `/products` | - | 200 OK |
| **Read One**| GET | `/products/{id}`| - | 200 OK |
| **Update** | PUT | `/products/{id}`| `{ "name": "Laptop Pro", "price": 1299 }`| 200 OK |
| **Partial Update**| PATCH| `/products/{id}`| `{ "price": 1199 }` | 200 OK |
| **Delete** | DELETE | `/products/{id}`| - | 204 No Content |

---

## 13. Validation

Never trust client input. Validation ensures data integrity before it reaches your database.

* **Required Fields**: Ensure mandatory data is present.
* **Data Types**: Ensure strings are strings, numbers are numbers.
* **Length Validation**: Min/Max characters for strings, min/max boundaries for numbers.
* **Format Validation**: Use Regex for custom formats (e.g., strong passwords).
* **Email Validation**: Ensure valid email syntax.

### Validation Response Format
When validation fails, return a `400 Bad Request` or `422 Unprocessable Entity` with details on exactly what failed.

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Input validation failed.",
    "details": [
      {
        "field": "email",
        "message": "Must be a valid email address."
      },
      {
        "field": "password",
        "message": "Password must be at least 8 characters long."
      }
    ]
  }
}
```

---

## 14. Error Handling

A standardized error format prevents clients from having to write complex, varied parsing logic.

### Standard Error Structure
```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "The requested user ID 123 does not exist."
  }
}
```

### Common Error Codes (Internal)
Using internal codes (e.g., `AUTH_001`, `DB_TIMEOUT`) helps support teams debug issues rapidly compared to generic text messages.

### Categories of Errors
1. **Validation Errors (400/422)**: Bad input.
2. **Authentication Errors (401)**: Missing, invalid, or expired tokens.
3. **Authorization Errors (403)**: Valid token, but insufficient privileges (e.g., User trying to access Admin route).
4. **Server Errors (500)**: Unhandled exceptions. Never leak stack traces to the client in production!

---

## 15. Authentication

Authentication proves **who** the user is.

| Method | How it Works | Pros | Cons |
| :--- | :--- | :--- | :--- |
| **API Keys** | A static string passed in headers (`x-api-key: 123`). | Simple to implement and use. | Hard to revoke individually, easily compromised if leaked. |
| **Basic Auth**| Base64 encoded `username:password` in headers. | Built into HTTP. | Insecure unless over HTTPS; sends credentials every time. |
| **Session Cookies**| Server creates a session, stores ID in a secure, HttpOnly cookie. | Highly secure against XSS, native browser support. | Hard to scale across microservices without a shared store (Redis). Not great for mobile apps. |
| **JWT (Bearer Token)**| Stateless token containing encrypted/signed user payload. Passed as `Authorization: Bearer <token>`. | Stateless, scalable, great for microservices and mobile. | Cannot be easily invalidated before expiry; risk of XSS if stored in localStorage. |
| **OAuth 2.0**| Framework for delegated access (e.g., "Log in with Google"). | Extremely secure, industry standard. | Complex to implement from scratch. |

---

## 16. Authorization

Authorization dictates **what** an authenticated user is allowed to do.

### RBAC (Role-Based Access Control)
Users are assigned roles (e.g., `Admin`, `Editor`, `Viewer`). Permissions are assigned to roles.
* *Example*: Only `Admin` can call `DELETE /users/:id`.

### ABAC (Attribute-Based Access Control)
Permissions depend on the attributes of the user, the resource, and the environment.
* *Example*: An `Editor` can only edit a `Post` if `post.authorId === user.id`.

---

## 17. API Security

Security must be implemented at every layer.

* **HTTPS**: Always use TLS/SSL to encrypt data in transit. Never serve APIs over HTTP.
* **CORS (Cross-Origin Resource Sharing)**: Restrict which domains can call your API from a browser.
* **Rate Limiting**: Prevent abuse, DDoS attacks, and brute-force attempts.
* **Input Validation & Sanitization**: Protect against SQL Injection and NoSQL Injection.
* **Secure Headers**: Use tools like Helmet (Node.js) to set `X-Frame-Options`, `X-Content-Type-Options`, `Content-Security-Policy`.
* **Least Privilege**: Ensure the database user connecting from your API has only necessary permissions.
* **Avoid Sensitive Data in URLs**: Never pass passwords, tokens, or PII in query parameters, as URLs are logged in server access logs.

---

## 18. Pagination

Returning 100,000 rows in one request will crash your server and the client. Pagination is mandatory for collections.

### 1. Offset Pagination (Limit/Offset)
The most common approach using SQL `LIMIT` and `OFFSET`.
* **URL**: `/users?page=3&limit=20`
* **Pros**: Easy to implement, allows jumping to a specific page.
* **Cons**: Inefficient for large datasets (database must scan all skipped rows). Vulnerable to data shifting if items are added/deleted while paginating.

### 2. Cursor Pagination (Keyset Pagination)
Uses an identifier (cursor) of the last retrieved item to fetch the next set.
* **URL**: `/users?cursor=eyJpZCI6MTU2fQ==&limit=20`
* **Pros**: Highly performant on large datasets, resilient to data shifts.
* **Cons**: Cannot easily "jump" to page 10; requires sequential navigation.

---

## 19. Filtering and Sorting

Design flexible query parameters for searching and sorting.

### Filtering
Allow precise matches:
`GET /products?category=electronics&brand=apple`

Allow operators for ranges (using standard prefixes):
`GET /products?price[gte]=100&price[lte]=500` (Greater than/Less than)

### Sorting
Use a `sort` parameter. Prefix with `-` for descending.
`GET /users?sort=-createdAt,name` (Sort by newest first, then alphabetical by name)

---

## 20. API Versioning

APIs change over time. Versioning ensures that breaking changes don't destroy existing clients.

### 1. URI Versioning (Most Common)
`GET /api/v1/users`
* **Pros**: Explicit, easy to route at the load balancer level.
* **Cons**: Clutters the URL.

### 2. Header Versioning
`GET /api/users` with header `Accept-Version: v1`
* **Pros**: Keeps URLs clean.
* **Cons**: Harder to test in a browser directly.

### 3. Query Parameter Versioning
`GET /api/users?version=1`
* **Pros**: Easy to test.
* **Cons**: Messy for complex queries.

*Best Practice*: Use URI versioning (`v1`) for major breaking changes. Minor non-breaking changes should be additive and not require a version bump.

---

## 21. Rate Limiting

Rate limiting protects your API from overwhelming traffic, scraping, and brute force attacks.

* **Algorithms**: Token Bucket, Leaky Bucket, Fixed Window, Sliding Window.
* **Implementation**: Often done at the API Gateway level, or using Redis in application middleware.

**Standard Rate Limiting Headers:**
```text
X-RateLimit-Limit: 100      (Total requests allowed per window)
X-RateLimit-Remaining: 99   (Requests left in current window)
X-RateLimit-Reset: 1609459200 (Timestamp when the limit resets)
```
If a limit is exceeded, return `429 Too Many Requests` with a `Retry-After` header.

---

## 22. Caching

Caching improves performance drastically by reducing database load.

### HTTP Caching Headers
* **Cache-Control**: Directs the browser/CDN on how to cache. (`Cache-Control: public, max-age=3600`)
* **ETag**: A hash representing the state of the resource. The client sends `If-None-Match: <ETag>`. If unchanged, the server returns `304 Not Modified` (empty body).
* **Last-Modified**: Timestamp of the last update.

### Infrastructure Caching
* **Redis/Memcached**: Application-level caching for database query results.
* **CDN (Content Delivery Network)**: Edge caching for static API responses (like configuration JSONs).

---

## 23. File Upload APIs

Handling files differs from handling standard JSON payloads.

* **Content-Type**: Use `multipart/form-data` instead of `application/json`.
* **Validation**: Never trust file extensions. Check MIME types and use tools to scan for malware.
* **Storage**: Do not store files in the database. Store them in object storage (AWS S3, Google Cloud Storage) and save the URL in the database.
* **Endpoint Design**: `/users/{id}/avatar`

---

## 24. API Documentation

An API is only as good as its documentation.

* **Swagger / OpenAPI**: The industry standard for documenting REST APIs. It generates interactive documentation where developers can test endpoints directly.
* **What to include**:
  * Authentication instructions.
  * Request parameters (path, query, body) and validation rules.
  * Example requests and responses.
  * Definitions of all status codes and error formats returned by the endpoint.

---

## 25. REST vs GraphQL

| Feature | REST | GraphQL |
| :--- | :--- | :--- |
| **Architecture** | Multiple endpoints (`/users`, `/posts`) | Single endpoint (`/graphql`) |
| **Data Fetching**| Server decides what data is returned. | Client dictates exactly what data is returned. |
| **Over-fetching**| High (getting full user object just to show name). | None. |
| **Under-fetching**| High (N+1 request problem). | None (can query nested relations in one go). |
| **Caching** | Native HTTP caching works perfectly. | Complex, requires specialized tools (Apollo). |
| **Learning Curve**| Low (Standard HTTP rules). | Steep (requires learning Schema definition, Resolvers). |

---

## 26. GraphQL Basics

GraphQL is a query language for your API.

### Schema (Types)
```graphql
type User {
  id: ID!
  name: String!
  email: String
  posts: [Post!]!
}
```

### Query (Reading Data)
Client requests exactly what they need:
```graphql
query {
  user(id: "1") {
    name
    posts {
      title
    }
  }
}
```

### Mutation (Modifying Data)
```graphql
mutation {
  createUser(name: "John") {
    id
    name
  }
}
```

---

## 27. gRPC Basics

gRPC uses Protocol Buffers (Protobuf) instead of JSON and HTTP/2 instead of HTTP/1.1. It is exceptionally fast and strictly typed.

* **Unary RPC**: Standard Request-Response.
* **Server Streaming RPC**: Client sends one request, server responds with a stream of messages.
* **Client Streaming RPC**: Client sends a stream of messages, server responds once.
* **Bidirectional Streaming**: Both sides send a sequence of messages independently.

*Use Case*: Internal microservice-to-microservice communication where high throughput and low latency are critical.

---

## 28. WebSockets

WebSockets provide a persistent, full-duplex communication channel over a single TCP connection.

* **Use Cases**: Real-time chat apps, live sports tickers, collaborative editing (Google Docs), multiplayer games.
* **Connection**: Initiated via an HTTP handshake, then "upgrades" to the `ws://` or `wss://` protocol.
* **Event-Driven**: The server can push data to the client at any time without the client requesting it.

---

## 29. API Testing

API testing is essential to ensure reliability.

* **Tools**:
  * **Postman / Insomnia**: GUI tools for manual testing and building collections.
  * **cURL**: Command-line tool. (`curl -X GET https://api.example.com/data`)
  * **Jest / Mocha / Supertest**: Automated testing frameworks for Node.js.
* **What to Test**:
  1. Happy path (200 OK).
  2. Edge cases (missing fields, invalid data - 400).
  3. Security (unauthorized access - 401/403).
  4. Performance (Load testing).

---

## 30. API Documentation Standards

* **OpenAPI Specification (OAS)**: A JSON/YAML format to define your API's structure.
* **Redoc**: A tool to generate beautiful static documentation from OpenAPI specs.
* **SDK Generation**: Tools like Swagger Codegen can automatically generate client libraries (SDKs) in Python, Java, JS, etc., based on your OpenAPI spec.

---

## 31. Node.js Express API Example

A practical example of a robust REST endpoint in Express.js (TypeScript context).

```javascript
import express from 'express';
const app = express();
app.use(express.json());

// Mock Database
let users = [{ id: 1, name: "Alice" }];

// GET: Fetch all users
app.get('/api/v1/users', (req, res) => {
  res.status(200).json({
    success: true,
    data: users
  });
});

// POST: Create a user with validation
app.post('/api/v1/users', (req, res) => {
  const { name } = req.body;
  
  if (!name || typeof name !== 'string') {
    return res.status(400).json({
      success: false,
      error: { message: "Name is required and must be a string." }
    });
  }

  const newUser = { id: users.length + 1, name };
  users.push(newUser);

  res.status(201).json({
    success: true,
    data: newUser
  });
});

app.listen(3000, () => console.log('API running on port 3000'));
```

---

## 32. API Folder Structure

A scalable folder structure for Node.js (MVC / Service-Controller pattern):

```text
src/
├── config/           # Environment variables, database config
├── controllers/      # Route handlers (req, res logic)
├── services/         # Business logic (DB calls)
├── models/           # Database schemas/entities
├── routes/           # Express router definitions
├── middlewares/      # Auth, error handling, rate limiting
├── utils/            # Helper functions
└── app.js            # Express app initialization
```

---

## 33. Microservices API Design

In a microservices architecture, APIs communicate across networks heavily.

* **API Gateway**: A single entry point for all clients. It routes requests to the correct internal microservice, handles rate limiting, and auth checking.
* **Service Discovery**: Microservices dynamically find each other's IPs.
* **Communication**: 
  * Synchronous: HTTP REST or gRPC.
  * Asynchronous: Event-Driven architectures using Message Brokers (Kafka, RabbitMQ) to decouple services.

---

## 34. API Performance

Fast APIs lead to great user experiences.

1. **Database Indexing**: Ensure database queries hit indexed columns.
2. **N+1 Query Problem**: Use SQL `JOIN`s or GraphQL Dataloaders to fetch related data efficiently.
3. **Compression**: Enable gzip or Brotli compression middleware to shrink JSON payloads.
4. **Caching**: Implement Redis for frequently accessed, rarely changed data.
5. **Connection Pooling**: Reuse database connections instead of opening new ones per request.

---

## 35. API Monitoring

Once deployed, you must know what your API is doing.

* **Logging**: Log incoming requests, status codes, and execution times (e.g., using Winston/Morgan in Node.js).
* **Metrics**: Monitor CPU, memory, and error rates using Prometheus/Grafana.
* **APM (Application Performance Monitoring)**: Tools like Datadog or New Relic trace requests end-to-end to find bottlenecks.
* **Health Checks**: Implement a `GET /health` endpoint that ping the database to ensure the service is fully operational.

---

## 36. API Best Practices Summary

1. **Use HTTP verbs appropriately**.
2. **Use Plural Nouns for URIs**.
3. **Return Standard Error Envelopes**.
4. **Always Paginate large datasets**.
5. **Never expose internal IDs** if security is a concern (use UUIDs).
6. **Implement Rate Limiting and CORS**.
7. **Version your APIs from day one**.
8. **Keep endpoints stateless**.
9. **Write OpenAPI specs first (API-First)**.
10. **Validate everything**.

---

## 37. Common API Design Mistakes

* **Mistake**: Using `200 OK` for all responses, even errors, and putting an "error" flag inside the JSON.
  * **Alternative**: Use proper HTTP status codes (400, 404, 500).
* **Mistake**: Deeply nested URLs like `/users/1/posts/5/comments/10`.
  * **Alternative**: Keep URLs shallow `/comments/10`.
* **Mistake**: Breaking the API contract without versioning.
  * **Alternative**: Use `/v1/` and deploy `/v2/` side-by-side.
* **Mistake**: Returning null instead of empty arrays `[]`.
  * **Alternative**: Consistency is key. Collections should always be arrays.

---

## 38. Real-World API Examples

### Example: E-Commerce System
* `POST /api/v1/auth/login` (Authentication)
* `GET /api/v1/products?category=shoes&sort=-price` (Catalog search)
* `POST /api/v1/cart/items` (Add to cart)
* `POST /api/v1/orders/checkout` (Process payment and create order)

### Example: Blog Platform
* `GET /api/v1/posts`
* `GET /api/v1/posts/{slug}`
* `POST /api/v1/posts/{post_id}/comments`
* `PATCH /api/v1/users/me/settings`

---

## 39. API Design Patterns

* **Repository Pattern**: Abstracts database queries away from business logic.
* **Service Layer**: Contains business logic, independent of the HTTP request/response cycle.
* **DTO (Data Transfer Object)**: Defines the exact structure of data passed between layers (e.g., stripping password hashes before sending a user object to the controller).
* **Dependency Injection**: Injecting database instances or services into controllers, making testing significantly easier.

---

## 40. API Cheat Sheet

### Status Codes Quick Reference
* **200**: OK (GET, PUT, PATCH)
* **201**: Created (POST)
* **204**: No Content (DELETE)
* **400**: Bad Request (Invalid data)
* **401**: Unauthorized (No token/invalid token)
* **403**: Forbidden (No permission)
* **404**: Not Found (Resource missing)
* **422**: Unprocessable Entity (Validation failed)
* **429**: Too Many Requests (Rate limited)
* **500**: Internal Server Error

### HTTP Methods Quick Reference
* **GET**: Read (Safe, Idempotent)
* **POST**: Create (Not Safe, Not Idempotent)
* **PUT**: Replace (Not Safe, Idempotent)
* **PATCH**: Update Part (Not Safe, Usually Idempotent)
* **DELETE**: Remove (Not Safe, Idempotent)

---
*End of Guide*
