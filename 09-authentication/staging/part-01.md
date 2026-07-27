# PART I: Authentication Fundamentals

## Chapter 1: What is Authentication?

### What is it?
Authentication is the foundational process of verifying the identity of a user, device, or system. In computer science, it is the mechanism by which a system mathematically or logically proves that an entity (the "principal") is indeed who or what they claim to be. 

Whenever you log into a website, unlock your phone with your face, or an API service communicates securely with a database, authentication is occurring. It is the digital equivalent of a bouncer checking your ID at a club, or border control checking your passport.

### History and Evolution
The concept of authentication predates computing by millennia.
- **Physical Era (Pre-Computing):** Passwords ("shibboleths"), wax seals on letters, and physical keys.
- **Early Computing (1960s-1970s):** Fernando Corbató introduced the first computer password at MIT in 1961 for the Compatible Time-Sharing System (CTSS). 
- **Network Era (1980s-1990s):** As computers networked, protocols like Kerberos emerged to securely authenticate users over insecure networks without sending passwords in plaintext.
- **Web Era (2000s):** Cookies and sessions became the standard for web applications to remember authenticated state.
- **Modern Era (2010s-Present):** Passwords proved too fragile (due to phishing and data breaches). The industry moved towards Multi-Factor Authentication (MFA), OAuth, JSON Web Tokens (JWT), OpenID Connect (OIDC), and Passwordless standards like WebAuthn and Passkeys.

### Why do we need it?
Without authentication, systems have zero accountability. If an application cannot securely identify who is performing an action:
1. **No Data Privacy:** Anyone could read anyone else's private messages, bank statements, or health records.
2. **No Data Integrity:** Anyone could delete or modify a database maliciously.
3. **No Non-Repudiation:** A user could perform an action (e.g., wire transfer) and later deny it, and the system would have no mathematical proof they were the ones who initiated it.

### How does it work? (The Internal Working)
At its core, authentication relies on one or more "factors" of identity proof. An entity asserts an identity (e.g., "I am user_123") and then provides proof. The system validates the proof against a stored credential.

Authentication factors are broadly categorized into three types:
1. **Knowledge (Something you know):** A password, a PIN, or answers to security questions.
2. **Possession (Something you have):** A smartphone (for OTPs), a hardware security key (YubiKey), or a smart card.
3. **Inherence (Something you are):** Biometrics like fingerprints, facial recognition, or retina scans.

**The Internal Validation Process:**
1. **Claim:** The client sends an identifier (e.g., `email@example.com`).
2. **Challenge/Proof:** The client sends the proof (e.g., plaintext password).
3. **Retrieval:** The server retrieves the stored cryptographic representation of the proof (e.g., a hashed password and salt) associated with that identifier.
4. **Computation:** The server applies a one-way cryptographic function to the provided proof (e.g., hashing the provided plaintext password with the stored salt).
5. **Comparison:** The server compares the computed hash with the stored hash using a constant-time string comparison algorithm to prevent timing attacks.
6. **Assertion:** If they match, the system creates a secure session or issues a token representing the authenticated state.

### Real-World Analogy
Imagine arriving at a secure bank vault. 
- You walk up and state your name (Claiming Identity).
- The guard asks for your government-issued ID (Something you have).
- The guard asks for your vault PIN (Something you know).
- The guard verifies your face matches the ID (Something you are).
Once validated, the guard gives you a temporary visitor badge (Session Token) that lets you walk around the vault for the next hour without showing your ID at every single door.

### Production Example
In a standard Node.js/PostgreSQL backend:
1. Client POSTs `{ email: "user@x.com", password: "Password123!" }` to `/api/login`.
2. The server queries PostgreSQL: `SELECT id, password_hash FROM users WHERE email = $1`.
3. The server uses `bcrypt.compare(providedPassword, storedHash)`.
4. If `true`, the server signs a JWT and sets it as an `HttpOnly` cookie.

### Advantages and Disadvantages
**Advantages:**
- Secures sensitive data and enforces privacy.
- Enables personalized user experiences (profiles, histories).
- Creates an audit trail for compliance and security monitoring.

**Disadvantages:**
- Adds friction to the user experience (UX).
- Creates a massive attack surface; authentication endpoints are the most heavily targeted by hackers.
- Implementing authentication securely is notoriously difficult and error-prone.

### The Authentication Lifecycle
1. **Enrollment/Registration:** Establishing the identity and binding a credential (signing up).
2. **Authentication:** The actual login event.
3. **Session Management:** Maintaining the authenticated state across multiple requests.
4. **Step-Up Authentication:** Asking for stronger proof (like an OTP) when a user attempts a highly sensitive action (e.g., changing a password).
5. **Revocation/Logout:** Terminating the authenticated state and invalidating credentials.

### Interview Questions
- **Q:** Explain the difference between identification and authentication.
  - **A:** Identification is claiming who you are (providing a username). Authentication is proving you are who you claim to be (providing the password).
- **Q:** Why do we hash passwords instead of encrypting them?
  - **A:** Encryption is two-way (reversible if you have the key). Hashing is a one-way mathematical function. If a database is breached, hackers cannot reverse the hashes to get the plaintext passwords, whereas if they steal an encryption key, all passwords are compromised.

### Summary
Authentication is the gatekeeper of modern applications. It has evolved from simple passwords to complex cryptographic exchanges. Building a secure system requires understanding not just how to check a password, but how to maintain state, defend against brute force, and protect the identity lifecycle.

---

## Chapter 2: Authentication vs Authorization

One of the most common mistakes made by junior developers and interview candidates is confusing Authentication and Authorization. While they are intrinsically linked, they are entirely different security domains.

### Authentication (AuthN)
- **Question:** "Who are you?"
- **Concept:** Identity Verification.
- **Mechanism:** Passwords, Biometrics, OTPs.
- **Example:** Logging into AWS using your email and a YubiKey.
- **Acronym:** AuthN

### Authorization (AuthZ)
- **Question:** "What are you allowed to do?"
- **Concept:** Access Control and Permissions.
- **Mechanism:** Role-Based Access Control (RBAC), Attribute-Based Access Control (ABAC), Access Control Lists (ACL).
- **Example:** An IAM policy in AWS that allows your logged-in user to read an S3 bucket but denies you permission to delete the bucket.
- **Acronym:** AuthZ

### Comparison Table

| Feature | Authentication (AuthN) | Authorization (AuthZ) |
| :--- | :--- | :--- |
| **Primary Goal** | Verify identity | Determine access rights |
| **Timing** | Happens **before** Authorization | Happens **after** Authentication |
| **Failure Result** | 401 Unauthorized | 403 Forbidden |
| **Data Handled** | Credentials (Passwords, Tokens) | Rules, Roles, Policies, Permissions |
| **Standard Protocols** | OpenID Connect (OIDC), SAML | OAuth 2.0 (historically), XACML, OPA |

### Identity, Access, and Permission
To fully grasp AuthZ, you must understand these terms:
- **Identity:** The verified entity (e.g., User ID 991).
- **Permission:** A specific action on a specific resource (e.g., `article:delete`, `user:read`).
- **Access Control:** The system that evaluates the Identity against the Permission. If User 991 tries to trigger `article:delete`, the Access Control engine returns `True` or `False`.

### Real Examples
- **Authentication:** Showing your boarding pass and passport at TSA security. They verify you are John Doe.
- **Authorization:** Showing your boarding pass at the gate. The gate agent verifies you are allowed to board Flight 101 in Seat 12A. Even though you are authenticated (you are John Doe), you are not authorized to board Flight 999.

### Production Example (Express.js)
```javascript
// AUTHENTICATION MIDDLEWARE: Determines WHO you are
const authenticate = (req, res, next) => {
  const token = req.cookies.session_token;
  if (!token) return res.status(401).send("Who are you? Login first."); // 401 Unauthorized
  
  const user = verifyToken(token);
  if (!user) return res.status(401).send("Invalid token.");
  
  req.user = user; // Attach identity to request
  next();
};

// AUTHORIZATION MIDDLEWARE: Determines WHAT you can do
const authorizeAdmin = (req, res, next) => {
  // We already know who they are (req.user exists)
  if (req.user.role !== 'admin') {
    return res.status(403).send("You do not have permission to do this."); // 403 Forbidden
  }
  next();
};

// Route
app.delete('/api/users/:id', authenticate, authorizeAdmin, (req, res) => {
  // Only authenticated admins reach this point
  deleteUser(req.params.id);
  res.send("User deleted.");
});
```
*Note how `authenticate` runs first, setting `req.user`. `authorizeAdmin` runs second, checking the properties of `req.user`.*

### Common Interview Questions
- **Q:** A user is logged in, but when they click "Delete Post", the server returns a 401 Unauthorized error. Is this the correct HTTP status code?
  - **A:** No. If the user is logged in (authenticated) but lacks permission, the server should return `403 Forbidden`. `401` implies the server does not know who the user is. (Historically, HTTP standard named 401 "Unauthorized" instead of "Unauthenticated", leading to decades of confusion, but technically 401 means AuthN failure, and 403 means AuthZ failure).

---

## Chapter 3: Identity

Before we can authenticate a user, we must understand what "Identity" actually means in a distributed computing environment.

### User Identity vs Digital Identity
- **User Identity:** The flesh-and-blood human being interacting with the keyboard.
- **Digital Identity:** The representation of that human within a computer system. This usually consists of a unique identifier (like a UUID) and a collection of attributes (email, name, role).

The core challenge of authentication is firmly binding the Digital Identity to the User Identity. If an attacker steals a password, the system cannot distinguish between the attacker and the real user; the digital identity has been compromised.

### Identity Providers (IdP)
In modern architecture, we rarely build identity databases from scratch. We delegate identity management to an **Identity Provider (IdP)**.
An IdP is a centralized service that creates, manages, and stores digital identities. 
- **Examples:** Google Workspace, Microsoft Entra ID (Active Directory), Okta, Auth0, AWS Cognito.

Instead of your application verifying a password, your application redirects the user to the IdP. The IdP authenticates the user and sends a cryptographic token back to your application saying, "I verify this is User X."

### Trust
Identity relies entirely on **Trust**. 
If you allow users to log into your site using "Sign in with Google," you are configuring a trust relationship. You trust that Google has properly authenticated the user and that the tokens Google issues are mathematically valid. If Google's security fails, your security fails.

### Credentials
A credential is the data used to prove identity. It is crucial to separate the *identity* from the *credential*. 
A single digital identity (`user_id: 123`) can have multiple credentials attached to it:
1. A bcrypt-hashed password.
2. A WebAuthn public key for FaceID.
3. A connected Google Account (OAuth connection).
If the user forgets their password, you don't delete their identity; you revoke the old credential and enroll a new one.

### Production Example: Identity Schema
A robust production database separates Users from Credentials.
```sql
-- The Digital Identity
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);

-- The Credentials attached to the Identity
CREATE TABLE credentials (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    credential_type VARCHAR(50) NOT NULL, -- 'PASSWORD', 'OAUTH_GOOGLE', 'PASSKEY'
    credential_data TEXT NOT NULL, -- The hash, the provider ID, or the public key
    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## Chapter 4: Authentication Flow

To truly understand authentication, we must trace the exact flow of data from the user's brain to the secure resource. We will map a standard Session-based Authentication Flow.

### The Request Lifecycle

```mermaid
sequenceDiagram
    autonumber
    actor User
    participant Browser as Browser (Client)
    participant API as API Gateway / Backend
    participant DB as Database

    User->>Browser: Enters email and password
    Browser->>API: POST /login {email, password} (Over HTTPS)
    API->>DB: Query User by email
    DB-->>API: Returns (userId, passwordHash)
    Note over API: API hashes input password<br/>and compares with DB hash.
    API->>API: Validates Password -> Success!
    API->>DB: Create Session (userId, expiresAt)
    DB-->>API: Returns Session ID (e.g. s_123xyz)
    API-->>Browser: HTTP 200 OK<br/>Set-Cookie: session=s_123xyz; HttpOnly; Secure
    Note over Browser: Browser securely stores the cookie.
    
    User->>Browser: Clicks "View Dashboard"
    Browser->>API: GET /dashboard<br/>Cookie: session=s_123xyz
    API->>DB: Query Session (s_123xyz)
    DB-->>API: Returns Session Data (userId, valid)
    Note over API: API identifies the User<br/>and processes the request.
    API-->>Browser: HTTP 200 OK (Dashboard Data)
```

### Step-by-Step Breakdown

#### 1. User -> Browser
The user types their plaintext credentials into a form. This form must be served over HTTPS to ensure the plaintext password isn't intercepted on the local network (a MITM attack).

#### 2. Browser -> Frontend -> API
The frontend (React, Vue, or raw HTML) intercepts the form submission, prevents the default page reload, and sends an AJAX/Fetch `POST` request to the backend. The payload contains the raw credentials.

#### 3. API -> Database
The backend receives the credentials. It first performs a lookup. It does **not** look up by password. It looks up by the unique identifier (email or username).

#### 4. The Authentication Server / Backend Logic
The backend receives the `password_hash` from the database. It uses a cryptographic library (like bcrypt) to hash the incoming plaintext password and compare it. 
*Why does it do this instead of decrypting the database hash? Because hashes are one-way functions; they cannot be decrypted.*

#### 5. Issuing the Token (State Creation)
If the comparison is successful, the server must create "State". HTTP is a stateless protocol; every request is entirely independent. If we don't create state, the user will have to send their password with every single click.
The server generates a unique, unguessable string (a Session ID or a JWT) and sends it back to the client.

#### 6. Transporting the Token
The server sends this token back in an HTTP header, usually a `Set-Cookie` header. 
`Set-Cookie: session_id=abc123random; HttpOnly; Secure; SameSite=Strict`
This tells the browser: "Save this string, and automatically attach it to all future requests to my domain."

#### 7. Accessing Protected Resources
On the next request (e.g., loading the dashboard), the browser automatically attaches the Cookie.
The backend intercepts this request. Before reaching the route handler, an Authentication Middleware intercepts the request, reads the cookie, looks up the session ID in Redis or the Database, finds the associated `user_id`, and attaches it to the request context. 
The request is now authenticated.

### Key Takeaway
Authentication is not just the act of checking a password. It is a continuous loop of establishing trust (login), issuing a proof of trust (token/cookie), and verifying that proof on every subsequent action.
