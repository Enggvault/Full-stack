<div align="center">

# Full Stack Web Development

**A structured, single-source-of-truth documentation project for learning full-stack web development — from the fundamentals of the web to API design.**

[![Modules](https://img.shields.io/badge/Modules-6-4f46e5?style=flat-square)](.)
[![Format](https://img.shields.io/badge/Format-Markdown-0ea5e9?style=flat-square)](.)
[![Level](https://img.shields.io/badge/Level-Beginner%20→%20Intermediate-10b981?style=flat-square)](.)
[![Maintained](https://img.shields.io/badge/Maintained-Yes-f59e0b?style=flat-square)](.)

*Maintained by **[Tushar Kanti Dey](https://tushardevx01.tech)** under [EnggVault](https://github.com/Enggvault)*

</div>

---

## Purpose

This repository is a curated, structured learning resource for developers who want to understand how the modern web works and how to build production-quality full-stack applications.

Each module covers exactly one topic. There are no repeated definitions or overlapping explanations. Every concept has a single canonical location, and modules cross-reference each other where continuity is needed.

The writing follows the conventions of professional technical documentation — concise, correct, and example-driven.

---

## Learning Roadmap

Read modules in order. Each module assumes the reader has completed all preceding modules.

```
01 - Full Stack Fundamentals
     Web architecture, client-server model, HTTP overview,
     cookies, sessions, tech stacks, databases, deployment.
          ↓
02 - HTML
     Document structure, semantic markup, forms,
     accessibility, SEO, HTML5 APIs.
          ↓
03 - CSS
     Selectors, cascade, Box Model, Flexbox, Grid,
     responsive design, animations, CSS variables.
          ↓
04 - JavaScript
     Language core, DOM, events, browser APIs,
     classes, modules, async introduction.
          ↓
05 - HTTP, JSON & Fetch
     HTTP in depth, JSON format, Fetch API,
     Promises, async/await, client-side CRUD.
          ↓
06 - API Design
     REST architecture, resource design, auth,
     versioning, rate limiting, GraphQL, gRPC, OpenAPI.
```

---

## Repository Structure

```
Full-stack/
├── 01-full-stack-fundamentals/
│   ├── README.md          — Module overview, objectives, prerequisites
│   └── notes.md           — Full stack fundamentals reference
│
├── 02-html/
│   ├── README.md
│   └── notes.md           — HTML reference
│
├── 03-css/
│   ├── README.md
│   └── notes.md           — CSS reference
│
├── 04-javascript/
│   ├── README.md
│   └── notes.md           — JavaScript language & browser APIs
│
├── 05-http-json-fetch/
│   ├── README.md
│   └── notes.md           — HTTP, JSON, Fetch, async/await
│
├── 06-api-design/
│   ├── README.md
│   └── notes.md           — REST, GraphQL, gRPC, API design patterns
│
└── README.md              — This file
```

---

## Module Overview

| # | Module | Topics | Difficulty |
|:--|:-------|:-------|:----------:|
| 01 | [Full Stack Fundamentals](./01-full-stack-fundamentals/README.md) | Web architecture, browsers, protocols, tech stacks | ⬛ Beginner |
| 02 | [HTML](./02-html/README.md) | Document structure, semantics, forms, a11y, SEO | ⬛ Beginner |
| 03 | [CSS](./03-css/README.md) | Styling, layouts, responsive design, animations | ⬛ Beginner |
| 04 | [JavaScript](./04-javascript/README.md) | Language core, DOM, browser APIs, OOP, modules | 🟦 Intermediate |
| 05 | [HTTP, JSON & Fetch](./05-http-json-fetch/README.md) | HTTP protocol, JSON, Fetch API, Promises, async/await | 🟦 Intermediate |
| 06 | [API Design](./06-api-design/README.md) | REST, resource naming, auth, GraphQL, gRPC, security | 🟥 Advanced |

---

## How to Use This Repository

1. **Read sequentially.** The modules are ordered by dependency. Module 05 explicitly requires concepts from 04, which requires 03, and so on.
2. **Use the notes as a reference.** After the initial read, each `notes.md` is designed to be revisited as a quick-reference document.
3. **Follow cross-references.** When a `notes.md` file references another module (`→ See 04-javascript/notes.md#closures`), follow it. Concepts are not repeated — they are explained once and referenced everywhere they apply.
4. **Run the code examples.** Every code block is complete and runnable. Open a browser console or VS Code and execute them.

---

## Contribution Guide

Contributions that maintain the documentation's quality and architecture are welcome.

**Before submitting a pull request:**

- Ensure the change belongs in the correct module. Do not add explanations that duplicate content already present in another module — add a cross-reference instead.
- Follow the established Markdown conventions: `#` for the page title, `##` for major sections, `###` for topics.
- Use `const` and `let` in all JavaScript examples. Never `var`.
- Write in the second or third person. Avoid first-person plural ("we", "let's").
- Keep prose concise. This is documentation, not a tutorial blog post.

---

## Future Modules

Planned additions to the roadmap:

- `07-nodejs-express` — Server-side JavaScript, Express.js, middleware, routing
- `08-databases` — SQL with PostgreSQL, NoSQL with MongoDB, ORMs
- `09-authentication` — JWT, OAuth 2.0, session strategies, secure implementation
- `10-react` — Components, hooks, state management, React Router
- `11-nextjs` — App Router, server components, SSR, deployment
- `12-deployment` — Docker, CI/CD, cloud platforms, environment configuration

---

<div align="center">

Developed and maintained by **[Tushar Kanti Dey](https://tushardevx01.tech)**

If this repository is useful, consider starring it. ⭐

</div>
