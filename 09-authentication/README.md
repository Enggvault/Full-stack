# Module 09: Authentication

Welcome to the comprehensive Authentication engineering module for EnggVault. This module covers everything from authentication fundamentals to advanced production system design, security, and real-world implementations.

## Overview
Authentication is the process of verifying who a user or system is. In modern applications, this encompasses far more than just verifying passwords; it includes managing sessions securely, utilizing tokens, integrating delegated authorization, protecting against sophisticated threats, and scaling authentication across distributed microservices.

## Prerequisites
- Basic understanding of HTTP (requests, responses, headers).
- Familiarity with web application architectures (client-server model).
- Basic knowledge of databases and SQL.
- Foundational understanding of Node.js and REST APIs.

## Learning Objectives
By the end of this module, you will be able to:
1. Understand the core differences between Authentication and Authorization.
2. Securely handle and store passwords using modern hashing algorithms (Argon2, bcrypt).
3. Design and implement robust session management systems using cookies and Redis.
4. Architect stateless authentication using JWTs, Access Tokens, and Refresh Token rotation.
5. Understand and implement OAuth 2.0 and OpenID Connect (OIDC) flows.
6. Enhance security with Multi-Factor Authentication (MFA) and WebAuthn (Passkeys).
7. Apply production-grade security measures against common vulnerabilities (XSS, CSRF, Token Theft).
8. Architect authentication for diverse systems, including monoliths, microservices, and multi-tenant SaaS.

## Curriculum Topics
The detailed curriculum is located in [`notes.md`](./notes.md).

- **Authentication Fundamentals** (Identity, Credentials, AuthN vs AuthZ, Factors)
- **Password Authentication** (Hashing, Salts, Peppers, Algorithms, Attacks)
- **Sessions & Cookies** (Stateful Auth, Redis Storage, Cookie Attributes, Security)
- **Token Authentication & JWT** (Stateless Auth, Bearer Tokens, JWT Structure, Signing)
- **OAuth 2.0 & OpenID Connect** (Delegated Auth, PKCE Flow, ID Tokens)
- **MFA & Passwordless** (TOTP, FIDO2, WebAuthn, Passkeys, Biometrics)
- **Authorization Essentials** (RBAC, ABAC, Scopes, Claims, Policies)
- **Authentication Security** (XSS, CSRF, Brute Force, Rate Limiting, Threat Mitigation)
- **Authentication Architecture** (Monoliths, Microservices, API Gateways, SSO, Zero Trust)
- **Production Implementation** (Node.js, Express, TypeScript, Prisma, PostgreSQL, Redis)
- **Database & API Design** (Schemas, Relationships, ER Diagrams, Endpoints)
- **Framework Comparisons** (Node.js, Next.js, Spring Boot, Go, etc.)
- **System Design** (Small SaaS, E-Commerce, Microservices, Enterprise SSO)
- **Interview Preparation** (Questions, Scenarios, Follow-ups)
- **Common Mistakes & Checklists** (Pitfalls, Production Checklist, Cheat Sheet)

## Estimated Reading Time
5–7 hours

## Difficulty
🟥 Advanced

## Next Module
→ 10 — Authorization (Planned)

## Files in This Module
| File | Description |
|:-----|:------------|
| `README.md` | This file — module overview and objectives |
| `notes.md` | Full reference notes for this module |
