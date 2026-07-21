# Module 07 — Node.js & Express

## Purpose

This module is the canonical server-side programming reference in this repository. It covers Node.js from its runtime internals to building a complete production-structured REST API with Express.js.


## Topics Covered

- Node.js runtime, V8 engine, and Event Loop internals
- Non-blocking I/O, libuv, and the Thread Pool
- CommonJS and ES Modules
- npm, package.json, and semantic versioning
- Express application architecture
- Routing, middleware, and request/response lifecycle
- Environment variables and configuration
- Error handling patterns (AppError, asyncHandler, global handler)
- Async programming: callbacks → Promises → async/await
- File system, path, HTTP, events, streams, and buffers
- JWT and session authentication overview
- Complete Student Management CRUD API (project)
- 50 interview Q&As, best practices, common mistakes, cheat sheet


## Learning Objectives

After completing this module, you will be able to:

- Explain how the Node.js Event Loop processes async code.
- Build a structured REST API with Express using the Controller-Service pattern.
- Use middleware for authentication, validation, logging, and error handling.
- Handle errors consistently using a global error handler and AppError class.
- Write async route handlers without callback hell.
- Use core modules: `fs`, `path`, `http`, `events`, `stream`.


## Prerequisites

- [06 — API Design](../06-api-design/README.md)
- JavaScript: functions, objects, arrays, Promises, async/await


## Estimated Reading Time

5–7 hours


## Difficulty

🟥 Advanced


## Next Module

→ [08 — Databases & SQL](../08-databases/README.md)


## Files in This Module

| File | Description |
|:-----|:------------|
| `README.md` | This file — module overview and objectives |
| `notes.md` | Full reference notes for this module (28 chapters) |
