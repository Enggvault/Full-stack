# Module 06 — API Design

## Purpose

This module covers the design and architecture of server-side APIs. It is the canonical reference for REST principles, resource design, authentication, versioning, rate limiting, caching, and emerging API paradigms (GraphQL, gRPC, WebSockets).

This module is distinct from Module 05, which covers **consuming** APIs from the browser. This module covers **building** APIs on the server.


## Topics Covered

- What is API design?
- REST and the Richardson Maturity Model
- Resource naming and URL structure
- HTTP methods and idempotency in API context
- Request and response design
- HTTP status codes — selecting the correct code
- API versioning strategies
- Authentication and authorization (API Keys, JWT, OAuth 2.0)
- Input validation and sanitisation
- Error response format standardisation
- Rate limiting design
- Caching headers (`Cache-Control`, `ETag`, conditional requests)
- CORS
- GraphQL — queries, mutations, subscriptions
- gRPC
- WebSockets
- API documentation — OpenAPI/Swagger
- Node.js/Express implementation examples
- Security best practices


## Learning Objectives

By the end of this module, the reader should be able to:

- Design a complete REST API with correct resource names, methods, and status codes.
- Implement JWT authentication middleware in Express.
- Write a consistent, machine-readable error response format.
- Version an API non-destructively.
- Configure `Cache-Control` and `ETag` headers for appropriate caching behaviour.
- Describe when to use REST, GraphQL, or gRPC for a given problem.
- Document an API using OpenAPI 3.0.


## Prerequisites

- [05 — HTTP, JSON & Fetch](../05-http-json-fetch/README.md)


## Estimated Reading Time

90 – 120 minutes


## Difficulty

 Advanced


## Files in This Module

| File | Description |
|:-----|:------------|
| `README.md` | This file |
| `notes.md` | Complete API design reference |
