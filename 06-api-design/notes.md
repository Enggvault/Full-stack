# API Design

> **Module 06** · Prerequisites: [HTTP, JSON & Fetch ←](../05-http-json-fetch/notes.md)

> This module covers **designing and building** server-side APIs. For **consuming** APIs from the browser using the Fetch API, see [Module 05](../05-http-json-fetch/notes.md). HTTP fundamentals (methods, status codes, headers) are defined in [Module 05 — §1](../05-http-json-fetch/notes.md#1-http-in-depth); this module applies them in an API design context.


## Table of Contents

1. [Introduction to APIs](#1-introduction-to-apis)
2. [API Paradigms](#2-api-paradigms)
3. [REST Architecture](#3-rest-architecture)
4. [Resource Naming](#4-resource-naming)
5. [URL Design](#5-url-design)
6. [Request Design](#6-request-design)
7. [Response Design](#7-response-design)
8. [JSON Conventions](#8-json-conventions)
9. [CRUD API Blueprint](#9-crud-api-blueprint)
10. [Validation](#10-validation)
11. [Error Handling](#11-error-handling)
12. [Authentication](#12-authentication)
13. [Authorization](#13-authorization)
14. [Versioning](#14-versioning)
15. [Pagination](#15-pagination)
16. [Filtering and Sorting](#16-filtering-and-sorting)
17. [Rate Limiting](#17-rate-limiting)
18. [Caching](#18-caching)
19. [File Uploads](#19-file-uploads)
20. [API Security](#20-api-security)
21. [API Documentation and OpenAPI](#21-api-documentation-and-openapi)
22. [GraphQL](#22-graphql)
23. [gRPC](#23-grpc)
24. [WebSockets](#24-websockets)
25. [API Testing](#25-api-testing)
26. [Implementation Patterns](#26-implementation-patterns)
27. [Production Operations](#27-production-operations)
28. [Best Practices and Common Mistakes](#28-best-practices-and-common-mistakes)


## 1. Introduction to APIs

An **API (Application Programming Interface)** is a contract that defines how software components communicate. In the context of web development, an API exposes server-side resources and operations to clients over HTTP.

### Why APIs Matter

| Concern | Role of the API |
|:--------|:----------------|
| **Integration** | Connects disparate systems — web, mobile, third-party services — through a single interface |
| **Abstraction** | Hides database schemas, business rules, and infrastructure from the consumer |
| **Reusability** | A single API serves web apps, mobile apps, CLI tools, and partner integrations |
| **Scalability** | Decouples client deployment from server deployment |

### Request-Response Cycle

> **Prerequisite** — The HTTP request-response model, methods, headers, and status codes are defined in [Module 05 — HTTP in Depth](../05-http-json-fetch/notes.md#1-http-in-depth). This section assumes that knowledge.

1. The client sends an HTTP request to an API **endpoint** — a specific URL representing a resource.
2. The server validates the request, applies business logic, and queries the database.
3. The server returns an HTTP response containing a status code and a payload (typically JSON).

### API Lifecycle

| Phase | Description |
|:------|:------------|
| **Design** | Define endpoints, payloads, and behaviour — ideally before writing code |
| **Develop** | Implement the contract in server-side code |
| **Test** | Verify security, correctness, and performance |
| **Deploy** | Release to staging, then production |
| **Monitor** | Track usage, latency, error rates |
| **Retire** | Deprecate older versions with a migration timeline |

### API-First Development

API-first development means authoring the API contract (e.g., an OpenAPI specification) **before** writing implementation code. This enables frontend and backend teams to work in parallel against an agreed interface.


## 2. API Paradigms

APIs have evolved through several architectural paradigms.

| Paradigm | Transport | Data Format | Strengths | Weaknesses |
|:---------|:----------|:------------|:----------|:-----------|
| **RPC** | TCP / HTTP | XML / JSON | Action-oriented, simple | Tight coupling, no standardisation |
| **SOAP** | HTTP / SMTP | XML | ACID compliance, WS-Security | Verbose, heavyweight, difficult to parse |
| **REST** | HTTP | JSON / XML | Stateless, cacheable, widely adopted | Over-fetching, under-fetching |
| **GraphQL** | HTTP | JSON | Client-specified queries, no over-fetching | Complex caching, steep learning curve |
| **gRPC** | HTTP/2 | Protobuf | High throughput, bidirectional streaming | No native browser support |
| **WebSockets** | TCP (upgraded from HTTP) | Binary / Text | Real-time, full-duplex | Stateful, harder to scale horizontally |
| **SSE** | HTTP | Text | Simple server-to-client push | Unidirectional only |

> **Note:** REST is the dominant paradigm for public-facing APIs. GraphQL and gRPC are covered in [§22](#22-graphql) and [§23](#23-grpc).


## 3. REST Architecture

REST (Representational State Transfer) is an architectural style — not a protocol or specification. An API is considered RESTful when it satisfies these six constraints:

| # | Constraint | Requirement |
|:--|:-----------|:------------|
| 1 | **Client-Server** | The client handles the UI; the server handles data and logic. They evolve independently. |
| 2 | **Stateless** | Every request carries all the information the server needs. No session state is stored on the server between requests. |
| 3 | **Cacheable** | Responses must declare themselves cacheable or non-cacheable so clients and intermediaries can avoid redundant requests. |
| 4 | **Uniform Interface** | Resources are identified by URIs. Interactions use standard HTTP methods applied consistently. |
| 5 | **Layered System** | A client cannot distinguish whether it connects directly to the origin server or through a load balancer, CDN, or proxy. |
| 6 | **Code on Demand**  | The server may extend client functionality by transferring executable code (e.g., JavaScript). |

### Richardson Maturity Model

A framework for evaluating how closely an API follows REST principles:

| Level | Name | Characteristics |
|:------|:-----|:----------------|
| 0 | The Swamp of POX | HTTP used as a transport tunnel — one endpoint, one method |
| 1 | Resources | Individual URIs per resource (`/users`, `/orders`) |
| 2 | HTTP Verbs | Correct use of GET, POST, PUT, DELETE with appropriate status codes |
| 3 | Hypermedia (HATEOAS) | Responses contain links to discover related actions dynamically |

Most production APIs target **Level 2**. Level 3 (HATEOAS) is rare in practice.


## 4. Resource Naming

Resource naming is one of the most visible aspects of API design. Consistent naming reduces cognitive load for consumers.

### Rules

- **Use nouns, not verbs.** The URI identifies the resource; the HTTP method defines the action.
- **Use plural nouns.** `/users` is a collection; `/users/1` is an item within that collection.
- **Use lowercase kebab-case.** Hyphens improve readability in URLs.

### Examples

```text
# Correct
GET    /users
GET    /users/123
POST   /posts/10/comments
GET    /company-reports

# Incorrect
GET    /getUsers              ← verb in URI
POST   /createUser            ← verb in URI
POST   /users/123/delete      ← verb in path segment
GET    /UserList               ← mixed casing
GET    /company_reports        ← underscores (hyphens preferred in URLs)
```


## 5. URL Design

### Base URL and Version Prefix

Every API should expose a predictable base URL:

```text
https://api.example.com/v1/
```

### Nested Resources

When resources have a parent-child relationship, the URL reflects that hierarchy — but nesting should not exceed two to three levels.

```text
GET /posts/123/comments          ← all comments on post 123
GET /posts/123/comments/456      ← specific comment
```

If a resource can exist independently, prefer a flat structure:

```text
# Prefer
GET /comments/456

# Avoid
GET /authors/12/posts/34/comments/456
```

### Query Parameters

Query parameters modify the collection without changing the resource identity.

| Purpose | Example |
|:--------|:--------|
| **Filtering** | `/users?role=admin&status=active` |
| **Sorting** | `/users?sort=-createdAt,name` |
| **Pagination** | `/users?page=2&limit=20` |
| **Searching** | `/users?q=john` |


## 6. Request Design

### Path Parameters vs Query Parameters

| Type | Purpose | Example | Required? |
|:-----|:--------|:--------|:---------:|
| **Path** | Identify a specific resource | `/users/:id` | Yes |
| **Query** | Filter, sort, or modify a collection | `/users?status=active` | No |

### Headers

Metadata belongs in headers, not in the request body.

| Header | Usage |
|:-------|:------|
| `Authorization` | Bearer tokens, API keys |
| `Accept` | Expected response format (`application/json`) |
| `Accept-Language` | Localisation (`en-US`) |
| `Content-Type` | Format of the request body (`application/json`) |
| `Idempotency-Key` | Prevents duplicate processing of retried requests |

> **Prerequisite** — For the full HTTP headers reference, see [Module 05 — §2](../05-http-json-fetch/notes.md#2-http-headers).

### Request Body

Use JSON for standard data payloads. Keep structures flat where possible; group logically when complexity demands it.

```json
{
  "firstName": "Tushar",
  "lastName": "Dey",
  "email": "thetushardev0@gmail.com",
  "preferences": {
    "newsletter": true,
    "theme": "dark"
  }
}
```


## 7. Response Design

### Standard Response Envelope

A consistent response structure makes an API predictable for consumers.

**Success:**

```json
{
  "success": true,
  "data": {
    "id": 123,
    "name": "Jane Doe"
  },
  "message": "User retrieved successfully."
}
```

**Error:**

```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "User with ID 999 does not exist."
  }
}
```

### Paginated Collections

When returning collections, include metadata that enables the client to navigate pages.

```json
{
  "success": true,
  "data": [],
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


## 8. JSON Conventions

| Rule | Guidance |
|:-----|:---------|
| **Key naming** | Choose `camelCase` (standard in JavaScript ecosystems) or `snake_case` (Python/Ruby) — apply the choice uniformly |
| **Consistency** | A field named `userId` in one endpoint must not appear as `user_id` or `id_user` in another |
| **Booleans** | Always `true` / `false` — never `"true"`, `1`, or `0` |
| **Dates** | ISO 8601 strings: `2025-07-15T08:00:00Z` |
| **Null values** | Return `null` for missing scalar values — not `""` or omission — unless payload size is a documented concern |
| **Empty collections** | Always return `[]` — never `null` for a field that represents a list |

> **Prerequisite** — JSON syntax and `JSON.parse()` / `JSON.stringify()` are covered in [Module 05 — §4](../05-http-json-fetch/notes.md#4-json).


## 9. CRUD API Blueprint

A standard mapping of CRUD operations to HTTP methods for a `products` resource:

| Action | Method | Endpoint | Request Body | Response Status |
|:-------|:-------|:---------|:-------------|:----------------|
| Create | POST | `/products` | `{ "name": "Laptop", "price": 999 }` | `201 Created` |
| Read all | GET | `/products` | — | `200 OK` |
| Read one | GET | `/products/{id}` | — | `200 OK` |
| Replace | PUT | `/products/{id}` | `{ "name": "Laptop Pro", "price": 1299 }` | `200 OK` |
| Partial update | PATCH | `/products/{id}` | `{ "price": 1199 }` | `200 OK` |
| Delete | DELETE | `/products/{id}` | — | `204 No Content` |

> **Prerequisite** — HTTP method semantics (safety, idempotency) are defined in [Module 05 — §1](../05-http-json-fetch/notes.md#1-http-in-depth).


## 10. Validation

Client input must never be trusted. Validation prevents malformed or malicious data from reaching the database.

### Validation Types

| Type | Example |
|:-----|:--------|
| **Required fields** | `name` must be present |
| **Data type** | `price` must be a number |
| **Length** | `password` must be 8–128 characters |
| **Format** | `email` must match RFC 5322 syntax |
| **Pattern** | Custom regex for phone numbers, postal codes |

### Validation Error Response

Return `400 Bad Request` or `422 Unprocessable Entity` with field-level details:

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
        "message": "Must be at least 8 characters."
      }
    ]
  }
}
```


## 11. Error Handling

A standardised error format eliminates the need for consumers to write varied parsing logic for different failure modes.

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

Internal error codes (e.g., `AUTH_001`, `DB_TIMEOUT`) help support teams debug issues faster than generic text messages.

### Error Categories

| Category | Status Codes | Cause |
|:---------|:-------------|:------|
| **Validation** | `400`, `422` | Malformed input, missing required fields |
| **Authentication** | `401` | Missing, invalid, or expired credentials |
| **Authorisation** | `403` | Valid credentials, insufficient permissions |
| **Not Found** | `404` | Resource does not exist |
| **Conflict** | `409` | Duplicate entry, state conflict |
| **Rate Limit** | `429` | Too many requests in the current window |
| **Server** | `500`, `502`, `503` | Unhandled exceptions, upstream failures |

> **Warning:** Never expose stack traces, internal file paths, or database errors to the client in production. Log them server-side; return a generic message to the consumer.

> **Prerequisite** — For the full HTTP status code reference, see [Module 05 — §3](../05-http-json-fetch/notes.md#3-http-status-codes).


## 12. Authentication

Authentication verifies **who** the caller is.

| Method | Mechanism | Strengths | Weaknesses |
|:-------|:----------|:----------|:-----------|
| **API Keys** | Static string in header (`x-api-key: abc123`) | Simple to implement | Hard to revoke individually; easily leaked |
| **Basic Auth** | Base64-encoded `username:password` in `Authorization` header | Built into HTTP | Sends credentials on every request; requires HTTPS |
| **Session Cookies** | Server creates a session; ID stored in a `HttpOnly` cookie | Secure against XSS; native browser support | Difficult to scale across microservices without shared session store (Redis) |
| **JWT (Bearer Token)** | Stateless signed token in `Authorization: Bearer <token>` | Stateless, scalable, suited to microservices and mobile | Cannot be revoked before expiry without a deny list; XSS risk if stored in `localStorage` |
| **OAuth 2.0** | Delegated authorisation framework (e.g., "Sign in with Google") | Industry standard, extremely secure | Complex to implement from scratch |

### JWT Structure

A JWT consists of three Base64URL-encoded parts separated by dots:

```text
header.payload.signature
```

| Part | Contains |
|:-----|:---------|
| **Header** | Algorithm (`HS256`, `RS256`) and token type (`JWT`) |
| **Payload** | Claims — user ID, roles, issued-at (`iat`), expiry (`exp`) |
| **Signature** | HMAC or RSA signature over header + payload, using a server-side secret or private key |

> **Warning:** Never store sensitive data (passwords, credit card numbers) in the JWT payload. The payload is encoded, not encrypted — anyone with the token can decode it.

### OAuth 2.0 Flows

| Flow | Use Case |
|:-----|:---------|
| **Authorization Code** | Server-side web apps (most secure) |
| **Authorization Code + PKCE** | Single-page apps, mobile apps |
| **Client Credentials** | Machine-to-machine (no user context) |
| **Device Code** | Smart TVs, CLI tools with limited input |


## 13. Authorization

Authorisation determines **what** an authenticated user is allowed to do.

### RBAC (Role-Based Access Control)

Permissions are assigned to **roles**; users are assigned roles.

| Role | Permissions |
|:-----|:------------|
| `viewer` | `GET /users`, `GET /posts` |
| `editor` | All viewer permissions + `POST /posts`, `PATCH /posts/:id` |
| `admin` | All permissions including `DELETE /users/:id` |

### ABAC (Attribute-Based Access Control)

Access decisions depend on **attributes** of the user, the resource, and the environment.

Example: An `editor` can only update a `post` if `post.authorId === user.id`.

ABAC is more granular than RBAC and is suited to systems with complex ownership or tenancy rules.


## 14. Versioning

APIs evolve. Versioning ensures that breaking changes do not disrupt existing consumers.

### Strategies

| Strategy | Example | Strengths | Weaknesses |
|:---------|:--------|:----------|:-----------|
| **URI versioning** | `GET /api/v1/users` | Explicit, easy to route at the load balancer | Clutters the URL |
| **Header versioning** | `Accept-Version: v1` | Clean URLs | Harder to test in a browser |
| **Query parameter** | `GET /api/users?version=1` | Easy to test | Conflates versioning with query semantics |

> **Best Practice:** Use URI versioning (`/v1/`) for major breaking changes. Non-breaking, additive changes (new fields, new optional parameters) do not require a version bump.


## 15. Pagination

Unbounded collection responses degrade performance for both client and server. Pagination is mandatory for any endpoint that returns a list.

### Offset Pagination

```text
GET /users?page=3&limit=20
```

| Aspect | Detail |
|:-------|:-------|
| **Implementation** | SQL `LIMIT` and `OFFSET` |
| **Strengths** | Simple; allows jumping to arbitrary pages |
| **Weaknesses** | Degrades on large datasets (the database must scan all skipped rows); vulnerable to data shifts during traversal |

### Cursor Pagination

```text
GET /users?cursor=eyJpZCI6MTU2fQ==&limit=20
```

| Aspect | Detail |
|:-------|:-------|
| **Implementation** | `WHERE id > :lastId ORDER BY id LIMIT :limit` |
| **Strengths** | Consistent performance regardless of dataset size; resilient to insertions and deletions |
| **Weaknesses** | Cannot jump to an arbitrary page; sequential navigation only |

> **Best Practice:** Use cursor pagination for high-volume, real-time datasets. Use offset pagination when page-jumping is a product requirement.


## 16. Filtering and Sorting

### Filtering

Allow exact matches and range operators via query parameters:

```text
GET /products?category=electronics&brand=apple
GET /products?price[gte]=100&price[lte]=500
```

### Sorting

Use a `sort` parameter. Prefix with `-` for descending order.

```text
GET /users?sort=-createdAt,name
```

This sorts by `createdAt` descending (newest first), then by `name` ascending.


## 17. Rate Limiting

Rate limiting protects an API from abuse, DDoS attacks, and brute-force attempts.

### Algorithms

| Algorithm | Behaviour |
|:----------|:----------|
| **Token Bucket** | Tokens replenish at a fixed rate; each request consumes one token |
| **Leaky Bucket** | Requests are queued and processed at a constant rate |
| **Fixed Window** | Counter resets at fixed intervals (e.g., 100 requests per minute) |
| **Sliding Window** | Rolling time window that smooths burst edges |

### Response Headers

```text
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 42
X-RateLimit-Reset: 1752590400
```

When the limit is exceeded, return `429 Too Many Requests` with a `Retry-After` header indicating when the client may retry.


## 18. Caching

Caching reduces database load and improves response times.

### HTTP Caching Headers

| Header | Purpose | Example |
|:-------|:--------|:--------|
| `Cache-Control` | Directs browsers and CDNs on cache behaviour | `Cache-Control: public, max-age=3600` |
| `ETag` | Hash representing the current state of the resource | Client sends `If-None-Match: <etag>`; server returns `304 Not Modified` if unchanged |
| `Last-Modified` | Timestamp of the most recent change | Used with `If-Modified-Since` conditional requests |

### Infrastructure Caching

| Layer | Tool | Use Case |
|:------|:-----|:---------|
| **Application** | Redis, Memcached | Cache database query results |
| **Edge** | CDN (Cloudflare, CloudFront) | Cache static or semi-static API responses close to the user |


## 19. File Uploads

File handling differs from standard JSON payloads.

| Concern | Guidance |
|:--------|:---------|
| **Content-Type** | Use `multipart/form-data`, not `application/json` |
| **Validation** | Never trust file extensions alone — validate MIME types; scan for malware |
| **Storage** | Store files in object storage (AWS S3, Google Cloud Storage); save the URL in the database |
| **Endpoint** | Use a resource-specific path: `/users/{id}/avatar` |


## 20. API Security

Security must be implemented at every layer.

| Practice | Detail |
|:---------|:-------|
| **HTTPS** | All traffic must be encrypted via TLS. Never serve APIs over plain HTTP. |
| **CORS** | Restrict which origins can call the API from a browser. Deny `*` in production. |
| **Rate Limiting** | Prevent abuse, scraping, and credential stuffing (see [§17](#17-rate-limiting)). |
| **Input Sanitisation** | Guard against SQL injection, NoSQL injection, and XSS via parameterised queries and strict validation. |
| **Secure Headers** | Set `X-Frame-Options`, `X-Content-Type-Options`, `Strict-Transport-Security`, `Content-Security-Policy`. Tools: Helmet (Node.js). |
| **Least Privilege** | The database user behind the API should have only the permissions it needs — no `DROP TABLE`. |
| **No Secrets in URLs** | Passwords, tokens, and PII must never appear in query parameters. URLs are logged in server access logs and browser history. |


## 21. API Documentation and OpenAPI

An API is only as useful as its documentation. Consumers should be able to integrate without reading source code.

### OpenAPI Specification (OAS)

The industry standard for defining REST APIs. Written in JSON or YAML, an OpenAPI spec describes every endpoint, parameter, request body, and response schema.

**What to include:**

- Authentication instructions and required scopes
- Request parameters (path, query, header, body) with types and validation rules
- Example requests and responses for each endpoint
- All possible status codes and error formats
- Rate limiting policy

### Tooling

| Tool | Purpose |
|:-----|:--------|
| **Swagger UI** | Generates interactive documentation from an OpenAPI spec — consumers can test endpoints directly |
| **Redoc** | Generates clean, static documentation from OpenAPI |
| **Swagger Codegen / OpenAPI Generator** | Generates client SDKs in Python, Java, TypeScript, Go, etc., from the spec |


## 22. GraphQL

GraphQL is a query language that allows clients to request exactly the data they need from a single endpoint.

### Schema Definition

```graphql
type User {
  id: ID!
  name: String!
  email: String
  posts: [Post!]!
}

type Post {
  id: ID!
  title: String!
  author: User!
}
```

### Query (Reading Data)

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

The response includes **only** the fields requested — no over-fetching.

### Mutation (Writing Data)

```graphql
mutation {
  createUser(name: "Tushar kanti Dey") {
    id
    name
  }
}
```

### REST vs GraphQL

| Dimension | REST | GraphQL |
|:----------|:-----|:--------|
| **Endpoints** | Multiple (`/users`, `/posts`) | Single (`/graphql`) |
| **Data shape** | Server-defined | Client-specified |
| **Over-fetching** | Common | Eliminated |
| **Under-fetching** | N+1 request problem | Eliminated (nested queries) |
| **Caching** | Native HTTP caching | Requires specialised tools (Apollo, Relay) |
| **Learning curve** | Low | Steep (schema language, resolvers) |

> **Best Practice:** REST is the default choice for most APIs. GraphQL is suited to frontends with complex, varied data requirements (dashboards, mobile apps with limited bandwidth).


## 23. gRPC

gRPC is a high-performance RPC framework developed by Google. It uses **Protocol Buffers** (Protobuf) for serialisation and **HTTP/2** for transport.

### RPC Types

| Type | Description |
|:-----|:------------|
| **Unary** | Standard request-response |
| **Server Streaming** | Client sends one request; server responds with a stream of messages |
| **Client Streaming** | Client sends a stream of messages; server responds once |
| **Bidirectional Streaming** | Both sides send message streams independently |

**Use case:** Internal microservice-to-microservice communication where high throughput and low latency are critical. gRPC is not natively supported by web browsers.


## 24. WebSockets

WebSockets provide a **persistent, full-duplex** communication channel over a single TCP connection.

| Aspect | Detail |
|:-------|:-------|
| **Connection** | Initiated via an HTTP handshake, then upgraded to `ws://` or `wss://` |
| **Direction** | Bidirectional — the server can push data at any time without a client request |
| **State** | Stateful — the connection persists until explicitly closed |
| **Use cases** | Real-time chat, live scores, collaborative editing, multiplayer games |

> **Note:** WebSockets are appropriate when the server must push data to the client unprompted. For simple server-to-client event streams, consider **Server-Sent Events (SSE)**, which use standard HTTP and are simpler to implement.


## 25. API Testing

### Tools

| Tool | Type |
|:-----|:-----|
| **Postman / Insomnia** | GUI-based manual testing and collection building |
| **cURL** | Command-line HTTP client |
| **Jest / Mocha + Supertest** | Automated testing frameworks for Node.js |
| **k6 / Artillery** | Load testing and performance benchmarking |

### What to Test

| Scenario | Expected Outcome |
|:---------|:-----------------|
| Happy path | `200 OK` / `201 Created` with correct payload |
| Missing required fields | `400 Bad Request` with field-level error details |
| Invalid authentication | `401 Unauthorized` |
| Insufficient permissions | `403 Forbidden` |
| Non-existent resource | `404 Not Found` |
| Duplicate creation | `409 Conflict` |
| Rate limit exceeded | `429 Too Many Requests` |
| Server failure | `500 Internal Server Error` (generic — no stack trace) |


## 26. Implementation Patterns

### Express.js API Example

A minimal but production-shaped REST endpoint in Node.js:

```javascript
import express from 'express';

const app = express();
app.use(express.json());

// In-memory data store (replace with a database in production)
const users = [{ id: 1, name: 'Alice' }];

// GET — retrieve all users
app.get('/api/v1/users', (req, res) => {
  res.status(200).json({ success: true, data: users });
});

// POST — create a user with validation
app.post('/api/v1/users', (req, res) => {
  const { name } = req.body;

  if (!name || typeof name !== 'string') {
    return res.status(400).json({
      success: false,
      error: { code: 'VALIDATION_ERROR', message: 'Name is required and must be a string.' },
    });
  }

  const newUser = { id: users.length + 1, name };
  users.push(newUser);

  res.status(201).json({ success: true, data: newUser });
});

app.listen(3000, () => console.log('API running on port 3000'));
```

### Folder Structure

A scalable project layout for Node.js (Service-Controller pattern):

```text
src/
├── config/           # Environment variables, database config
├── controllers/      # Route handlers — translate HTTP into service calls
├── services/         # Business logic — orchestrate data access and rules
├── models/           # Database schemas and entities
├── routes/           # Express router definitions
├── middlewares/      # Authentication, error handling, rate limiting
├── utils/            # Shared helper functions
├── validators/       # Request validation schemas (Joi, Zod)
└── app.js            # Express application initialisation
```

### Design Patterns

| Pattern | Purpose |
|:--------|:--------|
| **Repository** | Abstracts database queries from business logic — enables swapping databases without changing services |
| **Service Layer** | Encapsulates business rules independently of the HTTP layer |
| **DTO (Data Transfer Object)** | Defines the exact shape of data passed between layers (e.g., strips password hashes before sending to the client) |
| **Dependency Injection** | Injects database clients and services into controllers — simplifies unit testing with mocks |


## 27. Production Operations

### Microservices API Design

| Concept | Description |
|:--------|:------------|
| **API Gateway** | Single entry point for all clients; routes requests to internal microservices; handles rate limiting and authentication |
| **Service Discovery** | Mechanism for microservices to dynamically locate each other's network addresses |
| **Synchronous communication** | REST or gRPC between services |
| **Asynchronous communication** | Event-driven messaging via brokers (Kafka, RabbitMQ) to decouple services |

### Performance

| Optimisation | Impact |
|:-------------|:-------|
| **Database indexing** | Ensures queries hit indexed columns instead of scanning full tables |
| **Avoid N+1 queries** | Use `JOIN` operations or GraphQL DataLoaders to batch related data retrieval |
| **Response compression** | Enable gzip or Brotli middleware to reduce JSON payload size |
| **Application caching** | Cache frequently read, rarely written data in Redis |
| **Connection pooling** | Reuse database connections instead of opening a new connection per request |

### Monitoring

| Concern | Tooling |
|:--------|:--------|
| **Request logging** | Log method, path, status code, and latency for every request (Winston, Morgan) |
| **Metrics** | Track CPU, memory, error rates, and p95 latency (Prometheus + Grafana) |
| **APM** | Trace requests end-to-end across services to find bottlenecks (Datadog, New Relic) |
| **Health checks** | Expose `GET /health` that verifies database connectivity and returns the service version |


## 28. Best Practices and Common Mistakes

### Best Practices

1. Use HTTP methods according to their defined semantics.
2. Use plural nouns for resource URIs.
3. Return a consistent response envelope for both success and error cases.
4. Paginate all collection endpoints.
5. Prefer UUIDs over sequential integers for public-facing resource IDs.
6. Implement rate limiting and CORS from day one.
7. Version the API from the first release.
8. Keep endpoints stateless — no server-side session dependency.
9. Author an OpenAPI specification before writing code (API-first).
10. Validate every input; sanitise every output.

### Common Mistakes

| Mistake | Alternative |
|:--------|:-----------|
| Returning `200 OK` for errors and embedding an error flag in the body | Use the correct HTTP status code (`400`, `404`, `500`) |
| Deeply nested URLs: `/users/1/posts/5/comments/10` | Keep URLs shallow: `/comments/10` |
| Breaking the API contract without versioning | Deploy `/v2/` alongside `/v1/`; deprecate with a timeline |
| Returning `null` for empty collections | Always return `[]` for list-type fields |

### Real-World API Examples

**E-Commerce:**

```text
POST   /api/v1/auth/login
GET    /api/v1/products?category=shoes&sort=-price
POST   /api/v1/cart/items
POST   /api/v1/orders/checkout
GET    /api/v1/orders/{id}/tracking
```

**Blog Platform:**

```text
GET    /api/v1/posts
GET    /api/v1/posts/{slug}
POST   /api/v1/posts/{id}/comments
PATCH  /api/v1/users/me/settings
DELETE /api/v1/posts/{id}
```


> This is the final module in the current roadmap. Return to the [root README](../README.md) for the full module index and planned future modules.