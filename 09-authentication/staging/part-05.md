# PART V: JWT (JSON Web Tokens)

## Chapter 19: The Anatomy of a JWT

### JWT History and The Problem It Solves
Historically, sessions required a centralized database (or Redis) to store the state of an authenticated user (as discussed in Part III). Every API request required a database lookup. As architectures shifted from monoliths to microservices, this became a severe bottleneck. 

JSON Web Tokens (JWT, pronounced "jot") were introduced in 2010 to solve this. JWTs are **Stateless Authentication Tokens**. 
Instead of storing the user's data in a database and giving the user a random ID, the server packages the user's data (e.g., `user_id`, `role`) into a JSON object, cryptographically signs it, and hands the whole package to the user.
When the user sends it back, the server verifies the signature to ensure the data wasn't tampered with. **No database lookup is required.**

### JWT Structure
A JWT is a string consisting of three parts separated by dots (`.`):
`Header.Payload.Signature`

**Example JWT:**
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

#### 1. Header
Contains metadata about the token. Usually just the type (`JWT`) and the signing algorithm (`alg`).
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```
*Note: This JSON is Base64Url encoded to form the first part of the token.*

#### 2. Payload (Claims)
Contains the actual data (called "Claims"). There are three types of claims:
1. **Registered Claims:** Predefined by the JWT standard (RFC 7519). 
   - `iss` (Issuer)
   - `sub` (Subject - usually the user ID)
   - `aud` (Audience)
   - `exp` (Expiration time - crucial!)
   - `iat` (Issued At)
2. **Public Claims:** Custom claims registered in the IANA JSON Web Token Registry to avoid collisions.
3. **Private Claims:** Custom data agreed upon by your application (e.g., `role: "admin"`, `tenant_id: "xyz"`).

```json
{
  "sub": "user_991",
  "role": "admin",
  "exp": 1700000000
}
```
*Note: This JSON is Base64Url encoded to form the second part of the token. **IT IS NOT ENCRYPTED.** Anyone who intercepts the token can read this data.*

#### 3. Signature
The most critical part. To create the signature part, you have to take the encoded header, the encoded payload, a secret, and the algorithm specified in the header.
```javascript
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  your_256_bit_secret
)
```
If a hacker modifies their `role` from "user" to "admin", the Payload changes. When the server recalculates the signature using its secret, it won't match the signature on the token, and the server will reject it.

---

## Chapter 20: Access Tokens, Refresh Tokens, and Architecture

### The Expiration Dilemma
Because JWTs are stateless, you cannot easily revoke them. If a user is fired, but they have a JWT valid for 30 days, they can access the API for 30 days because the API doesn't check the database; it only checks the signature.

To solve this, we use the **Access Token + Refresh Token** architecture.

### Access Tokens
- **What it is:** A short-lived JWT (e.g., 15 minutes).
- **Purpose:** Used to access the API.
- **Storage:** Stored in memory or an `HttpOnly` cookie.

### Refresh Tokens
- **What it is:** A long-lived token (e.g., 7 days). It is often a random string, NOT a JWT.
- **Purpose:** Used ONLY to get a new Access Token when the old one expires.
- **Storage:** Must be stored securely (e.g., `HttpOnly` cookie). Must be stored in the database so it can be revoked.

### The Refresh Flow

```mermaid
sequenceDiagram
    autonumber
    actor Client
    participant API
    participant AuthServer
    participant DB

    Client->>AuthServer: POST /login {email, password}
    AuthServer->>DB: Verify & Store Refresh Token
    AuthServer-->>Client: HTTP 200 (Sets AT & RT Cookies)
    
    Client->>API: GET /data (with Access Token)
    API-->>Client: HTTP 200 (Success)
    
    Note over Client, API: ... 15 minutes later ...
    
    Client->>API: GET /data (with expired Access Token)
    API-->>Client: HTTP 401 Unauthorized
    
    Client->>AuthServer: POST /refresh (with Refresh Token)
    AuthServer->>DB: Check if RT is valid and not revoked
    AuthServer-->>Client: HTTP 200 (Sets New AT Cookie)
    
    Client->>API: GET /data (with New Access Token)
    API-->>Client: HTTP 200 (Success)
```

### Revocation Strategies
Since Access Tokens are stateless, how do we revoke access before the 15 minutes are up?
1. **Blacklisting:** Store a list of revoked token IDs (`jti` claim) in Redis. The API must check Redis on every request. (This defeats the purpose of statelessness).
2. **Short Expiration (Best Practice):** Keep the Access Token lifespan extremely short (e.g., 5 minutes). When you "revoke" a user, you just delete their Refresh Token from the database. They will lose access within 5 minutes.

### Token Rotation
A security measure for Refresh Tokens. Every time the Refresh Token is used to get a new Access Token, the server ALSO issues a brand new Refresh Token and invalidates the old one. If an attacker steals a Refresh Token, the moment they try to use it, the server will detect a re-use of an old token and instantly revoke the entire token family, locking out the attacker.

### Sliding Sessions
Every time the user is active and triggers a refresh, the new Refresh Token's expiration is extended by 7 days. If they are inactive for 7 days, they are logged out.

### Logout Flow
1. Client sends a request to `/logout`.
2. Server deletes the Refresh Token from the database.
3. Server clears the `HttpOnly` cookies for both Access and Refresh tokens on the client.

---

## Chapter 21: JWT Implementation Examples

### 1. Node.js / Express
```javascript
// npm install jsonwebtoken
import jwt from 'jsonwebtoken';

const ACCESS_SECRET = process.env.ACCESS_SECRET;
const REFRESH_SECRET = process.env.REFRESH_SECRET;

// Issue Token
export const generateTokens = (user) => {
    const accessToken = jwt.sign(
        { sub: user.id, role: user.role }, 
        ACCESS_SECRET, 
        { expiresIn: '15m' }
    );
    // Refresh token is often opaque, but can be a long-lived JWT
    const refreshToken = jwt.sign(
        { sub: user.id }, 
        REFRESH_SECRET, 
        { expiresIn: '7d' }
    );
    return { accessToken, refreshToken };
};

// Middleware to protect routes
export const requireAuth = (req, res, next) => {
    const token = req.cookies.access_token;
    if (!token) return res.status(401).send("No token");

    try {
        const decoded = jwt.verify(token, ACCESS_SECRET);
        req.user = decoded; // Attach claims to request
        next();
    } catch (err) {
        if (err.name === 'TokenExpiredError') {
            return res.status(401).send("Token Expired");
        }
        return res.status(403).send("Invalid Token");
    }
};
```

### 2. Next.js (Edge Middleware)
Next.js Middleware runs on the Edge, meaning you cannot use Node's `jsonwebtoken` library. You must use `jose`.
```typescript
// middleware.ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';
import { jwtVerify } from 'jose';

export async function middleware(request: NextRequest) {
    const token = request.cookies.get('access_token')?.value;
    
    if (!token) return NextResponse.redirect(new URL('/login', request.url));

    try {
        const secret = new TextEncoder().encode(process.env.ACCESS_SECRET);
        await jwtVerify(token, secret);
        return NextResponse.next();
    } catch (err) {
        return NextResponse.redirect(new URL('/login', request.url));
    }
}

export const config = { matcher: '/dashboard/:path*' };
```

### 3. Go (Golang)
```go
// go get github.com/golang-jwt/jwt/v5
package auth

import (
	"time"
	"github.com/golang-jwt/jwt/v5"
)

var secretKey = []byte("super_secret_key")

func GenerateToken(userID string) (string, error) {
	claims := jwt.MapClaims{
		"sub": userID,
		"exp": time.Now().Add(time.Minute * 15).Unix(),
	}
	token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
	return token.SignedString(secretKey)
}

func VerifyToken(tokenString string) (*jwt.Token, error) {
	return jwt.Parse(tokenString, func(token *jwt.Token) (interface{}, error) {
		return secretKey, nil
	})
}
```

### 4. Spring Boot (Java)
```java
// Using io.jsonwebtoken.jjwt
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.security.Keys;

import java.security.Key;
import java.util.Date;

public class JwtUtil {
    private final Key key = Keys.hmacShaKeyFor("a_very_long_secret_key_that_is_at_least_256_bits".getBytes());

    public String generateToken(String username) {
        return Jwts.builder()
                .setSubject(username)
                .setIssuedAt(new Date(System.currentTimeMillis()))
                .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 15)) // 15 mins
                .signWith(key)
                .compact();
    }
}
```

### 5. Client-Side Apps (React / Flutter / React Native)
**CRITICAL RULE:** Client-side apps should *not* parse JWTs to enforce security. Security is enforced on the backend. The client parses the JWT solely to update the UI (e.g., showing a "Welcome, Admin" banner).
For React/React Native, intercept HTTP requests to handle 401s and trigger the Refresh Flow.

```javascript
// React/Axios Interceptor for automatic Refresh
import axios from 'axios';

const api = axios.create({ baseURL: '/api', withCredentials: true });

api.interceptors.response.use(
    (response) => response,
    async (error) => {
        const originalRequest = error.config;
        // If 401 and we haven't retried yet
        if (error.response.status === 401 && !originalRequest._retry) {
            originalRequest._retry = true;
            try {
                // Call refresh endpoint
                await axios.post('/api/refresh', {}, { withCredentials: true });
                // Retry the original request
                return api(originalRequest);
            } catch (refreshError) {
                // Refresh failed, user is actually logged out. Redirect to login.
                window.location.href = '/login';
            }
        }
        return Promise.reject(error);
    }
);
```

### Interview Questions
- **Q:** Can an attacker change the JWT algorithm to `None`?
  - **A:** This was a famous vulnerability in early JWT libraries. If the header specified `{"alg": "none"}`, the library would skip signature verification entirely, allowing anyone to forge tokens. Modern libraries block this, but it's a critical historical vulnerability to mention.
- **Q:** Should you store sensitive data (like a credit card number) in a JWT?
  - **A:** Never. JWTs are encoded (Base64), not encrypted. Anyone can decode them. If you must put sensitive data in a token, you must use JWE (JSON Web Encryption).
