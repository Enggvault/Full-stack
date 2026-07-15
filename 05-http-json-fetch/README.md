# Module 05 — HTTP, JSON & Fetch

## Purpose

This module is the canonical reference for the communication layer between the browser and a server. It bridges the JavaScript language knowledge from Module 04 with the API design principles in Module 06.

The old `05-api-json-fetch-async/` directory has been superseded by this module. All content that duplicated HTML, CSS, or JavaScript fundamentals has been removed. Those topics are documented in their respective modules.

---

## Topics Covered

- HTTP in depth — methods, headers, status codes, the request-response cycle
- How browsers make HTTP requests
- JSON — format specification, `JSON.parse()`, `JSON.stringify()`
- The Fetch API — GET, POST, PUT, PATCH, DELETE requests
- Reading response metadata (status, headers)
- Promises in depth — states, chaining, `Promise.all`, `Promise.allSettled`
- `async`/`await` — syntax, error handling, parallel execution
- Practical CRUD pattern — consuming a REST API from the client
- Reading the Browser Network tab
- Common error handling patterns

---

## Learning Objectives

By the end of this module, the reader should be able to:

- Identify the correct HTTP method and status code for any CRUD operation.
- Send a POST request with a JSON body and an `Authorization` header using `fetch`.
- Handle HTTP errors and network failures correctly using `try/catch`.
- Run multiple async operations in parallel using `Promise.all`.
- Read JSON from a server response and update the DOM with the result.
- Use the browser's Network tab to inspect request/response details.

---

## Prerequisites

- [04 — JavaScript](../04-javascript/README.md) — particularly Promises and async/await introduction.

---

## Estimated Reading Time

50 – 65 minutes

---

## Difficulty

🟦 Intermediate

---

## Next Module

→ [06 — API Design](../06-api-design/README.md)

---

## Files in This Module

| File | Description |
|:-----|:------------|
| `README.md` | This file |
| `notes.md` | Complete HTTP, JSON & Fetch reference |
