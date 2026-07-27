# Authentication: The Complete Engineering Handbook

Welcome to the definitive, deep-dive engineering handbook on Authentication. This document provides a comprehensive exploration of authentication principles, mechanics, security vectors, and production architectures, expanding on every crucial topic to ensure a thorough understanding.

**Difficulty:** Beginner to Advanced (Progressive)
**Prerequisites:** HTTP, REST APIs, Basic Node.js/Database knowledge
**Target Audience:** Software Engineers, Backend Developers, Full-stack Developers, Security Architects
**Learning Objectives:** Master the intricate details of passwords, sessions, cookies, stateless tokens, JWTs, OAuth 2.0, OpenID Connect, MFA, WebAuthn, authorization models, and highly scalable production system architectures.

## Table of Contents
1. [Authentication Fundamentals](#1-authentication-fundamentals)
2. [Complete Authentication Lifecycle](#2-complete-authentication-lifecycle)
3. [Password Authentication](#3-password-authentication)
4. [Sessions & Cookies](#4-sessions--cookies)
5. [Token Authentication & JWT](#5-token-authentication--jwt)
6. [OAuth 2.0 & OpenID Connect](#6-oauth-20--openid-connect)
7. [Multi-Factor Authentication & Passwordless](#7-multi-factor-authentication--passwordless)
8. [Authorization Models & Implementation](#8-authorization-models--implementation)
9. [Authentication Security & Threat Mitigation](#9-authentication-security--threat-mitigation)
10. [Authentication Architecture Patterns](#10-authentication-architecture-patterns)
11. [Production Implementation (Node.js & TypeScript)](#11-production-implementation-nodejs--typescript)
12. [Database & API Design](#12-database--api-design)
13. [Framework Integration Differences](#13-framework-integration-differences)
14. [Real-World Authentication System Design](#14-real-world-authentication-system-design)
15. [Interview Preparation (Beginner to Advanced)](#15-interview-preparation-beginner-to-advanced)
16. [Common Mistakes & Mitigations](#16-common-mistakes--mitigations)
17. [Production Checklist & Final Cheat Sheet](#17-production-checklist--final-cheat-sheet)

---

## 1. Authentication Fundamentals

### Identity and Credentials
At its core, **Authentication (AuthN)** is the process of verifying a claimed identity. When a client asserts, "I am User A," the system demands proof. This proof is provided via **credentials**. 

### Authentication vs Authorization
It is a common pitfall to conflate these two concepts, yet they represent entirely different boundaries in system design:
- **Authentication (AuthN):** Validates the *identity* of the user. (e.g., Verifying a password). Failure results in `401 Unauthorized`.
- **Authorization (AuthZ):** Validates the *permissions* of an already authenticated user. (e.g., Checking if the user has the 'admin' role). Failure results in `403 Forbidden`.

| Feature | Authentication (AuthN) | Authorization (AuthZ) |
| :--- | :--- | :--- |
| **Question** | Who are you? | What are you allowed to do? |
| **Mechanism** | Passwords, OTPs, Biometrics, SSO | RBAC, ABAC, ACLs, Policies |
| **Artifact Produced**| Session ID, ID Token, Access Token | Access Granted / Denied decision |
| **HTTP Status Code** | `401 Unauthorized` (You are unknown) | `403 Forbidden` (You are known, but lack rights) |

### Authentication Factors
Authentication mechanisms are categorized into logical "factors." To achieve strong security, systems require multiple distinct factors (MFA).
1. **Knowledge Factor (Something you know):** Passwords, PIN codes, answers to security questions. (Highly vulnerable to phishing and data breaches).
2. **Possession Factor (Something you have):** A smartphone (running an authenticator app), a hardware security key (YubiKey), a smart card, or access to a specific email inbox.
3. **Inherence Factor (Something you are):** Biometrics such as fingerprints, facial recognition (FaceID), retina scans, or voice prints.

---

## 2. Complete Authentication Lifecycle

Authentication is not a single `/login` endpoint; it is an entire lifecycle that must be securely managed from account creation to deletion.

### 1. Registration
The user provides initial identity claims (email/username) and sets up their initial credentials (password). Security considerations during registration include preventing user enumeration (not revealing if an email is already registered during the flow) and enforcing strong password policies.

### 2. Email Verification / Account Activation
Before an account is fully active, the system must verify the user controls the provided communication channel. This is typically done by sending a unique, time-limited token (e.g., a random hex string) to the email address. This prevents spam accounts and ensures the user can receive recovery emails later.

### 3. Login
The user provides their credentials. The system verifies them against the stored records. If successful, the system transitions the user from an unauthenticated state to an authenticated state by generating a Session ID or issuing an Access Token.

### 4. Password Reset and Account Recovery
When a user forgets their credentials, the system must provide a secure recovery path.
- **Flow:** User requests a reset -> System generates a short-lived (e.g., 15-minute), cryptographically secure, single-use token -> Sends token via email -> User clicks link -> Provides new password.
- **Security:** The reset endpoint must be rate-limited to prevent email spamming. The token must be single-use and invalidated immediately upon successful password change.

### 5. Logout
Terminating the authenticated state. For sessions, this means destroying the session on the server and clearing the cookie on the client. For stateless JWTs, it requires clearing the client-side token and optionally adding the token to a server-side denylist.

---

## 3. Password Authentication

Despite its flaws, password authentication remains the backbone of the internet. Proper implementation is critical.

### Plaintext Password Risks
Storing passwords in plaintext (or even weakly hashing them) is a catastrophic security failure. If a database is dumped, attackers gain immediate access to all accounts. Furthermore, due to widespread password reuse, a breach on your site compromises your users' accounts on other platforms (email, banking).

### Hashing vs Encryption
- **Encryption:** A reversible, two-way mathematical operation. You encrypt a string with a key, and decrypt it later with the same (or corresponding) key. Passwords should **never** be encrypted, because if the key is stolen, all passwords are compromised.
- **Hashing:** A one-way mathematical function. It transforms an input of any size into a fixed-length string. You cannot reverse a hash back to the original password. To verify a login, you hash the provided password and compare it to the stored hash.

### Salt and Pepper
- **Salt:** A unique, randomly generated string added to each user's password *before* hashing. 
  - *Purpose:* It defeats **Rainbow Tables** (massive pre-computed dictionaries of hashes). Because the salt is unique per user, an attacker would have to compute a completely new rainbow table for every single user, rendering the attack computationally unfeasible. The salt is stored in plaintext in the database alongside the hash.
- **Pepper:** A global secret string added to the password *before* hashing (or used as an HMAC key). 
  - *Purpose:* Unlike the salt, the pepper is **not** stored in the database. It is stored in the application's environment variables or a Secret Manager. If an attacker dumps the database via SQL Injection but doesn't compromise the application server, they cannot crack the hashes because they lack the pepper.

### Hashing Algorithms (KDFs)
Standard cryptographic hashes (like SHA-256) are designed to be extremely fast. This is terrible for passwords, as modern GPUs can calculate billions of SHA-256 hashes per second. We must use **Key Derivation Functions (KDFs)** that are intentionally slow and resource-intensive (Key Stretching).

| Algorithm | Mechanism | Security | Recommendation |
| :--- | :--- | :--- | :--- |
| **Argon2** | Memory-hard & CPU-hard. Winner of the Password Hashing Competition. | Highest | **Best Choice** (Use the `Argon2id` variant). |
| **bcrypt** | CPU-hard. Truncates passwords at 72 characters. | High | **Excellent standard**. Widely supported and battle-tested. |
| **scrypt** | Memory-hard. Designed to be expensive for ASICs. | High | Good alternative if Argon2 is unavailable. |
| **PBKDF2** | CPU-hard. Uses repeated hashing (variable iterations). | Moderate | NIST approved, but vulnerable to GPU cracking compared to others. |

### Password Policies & Password Managers
- **Policies:** NIST guidelines recommend moving away from arbitrary complexity rules (requiring one uppercase, one number, one symbol) as they lead to predictable passwords (e.g., `Password123!`). Instead, prioritize **length** (minimum 8-12 characters) and screen against lists of known breached passwords (e.g., using the HaveIBeenPwned API).
- **Password Managers:** Encourage users to use password managers (1Password, Bitwarden). Password managers generate long, complex, unique passwords for every site, mitigating credential stuffing.

### Common Password Attacks
- **Brute Force:** Systematically trying every possible combination of characters. 
  - *Mitigation:* Rate limiting (e.g., max 5 attempts per minute), account lockout after N failures, CAPTCHA.
- **Dictionary Attacks:** Trying common words, names, and variations of leaked passwords.
  - *Mitigation:* Enforce minimum length, check against breached password lists, use Argon2.
- **Credential Stuffing:** Attackers use massive lists of stolen username/password pairs from one breach to log into other services, assuming users reuse passwords.
  - *Mitigation:* Multi-Factor Authentication (MFA), monitoring for impossible travel or unusual login patterns.
- **Password Spraying:** Instead of trying many passwords against one account (which triggers lockouts), attackers try one common password (e.g., `Spring2024!`) against thousands of different accounts.
  - *Mitigation:* MFA, robust rate limiting at the IP level, not just the account level.
