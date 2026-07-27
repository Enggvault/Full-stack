---
title: "Password Authentication"
part: 2
description: "Deep dive into password mechanics, secure hashing algorithms, salts, peppers, and password attack vectors."
difficulty: "Intermediate"
---

# Part II: Password Authentication

Despite the rise of biometrics and Passwordless flows, the humble password remains the foundation of internet authentication. However, securely storing and verifying passwords is one of the most frequently bungled tasks in software engineering.

---

## 1. Password Authentication

### What is it?
Password authentication is a "Knowledge-Based" factor. It relies on a pre-shared secret (the password) that is established between the user and the server during registration. 

When a user attempts to log in, they provide their username (or email) and the password. The server verifies that the provided password matches the secret stored in its database.

---

## 2. Plaintext Passwords

The most egregious error a developer can make is storing a password in "plaintext"—exactly as the user typed it.

### The Plaintext Flow (Dangerous)
```text
User Password (e.g., "hunter2")
     ↓
Application
     ↓
Database (Stores "hunter2")
```

### Why is this dangerous?
If your database is breached (via SQL injection, a leaked backup, or an insider threat), the attacker instantly possesses the passwords of every user. Because users frequently reuse passwords across multiple sites, a breach on your site means the attacker can now log into the user's bank or email account.

### The Correct Approach
```text
User Password (e.g., "hunter2")
     ↓
Password Hashing Function
     ↓
Password Hash (e.g., "$2b$12$Dk9...")
     ↓
Database
```

---

## 3. Hashing

To avoid plaintext storage, we use **Cryptographic Hashing**.

### What is a Hash Function?
A hash function is a mathematical algorithm that takes an input of any size (a password) and produces a fixed-size string of characters, which typically looks like random gibberish.

### Core Properties of a Secure Hash
1. **One-Way Function:** It must be mathematically infeasible to reverse the hash to find the original password. (This is why hashing is *not* encryption. Encryption is meant to be reversed via decryption. Hashing is permanent destruction of the input).
2. **Deterministic:** Hashing "hunter2" must always produce the exact same output hash.
3. **Collision-Resistant:** It must be mathematically infeasible to find two different passwords that produce the same hash.

---

## 4. Salt

Hashing alone is not enough. If two users have the password "password123", their hashes will be identical. An attacker can pre-compute the hashes for millions of common passwords (creating a "Rainbow Table"). When they breach your database, they just match the hashes to their table to instantly reveal the passwords.

### What is a Salt?
A salt is a cryptographically random string generated uniquely for *each user* during registration.

### How it works
Instead of hashing just the password, the server concatenates the password and the unique salt.
`Hash(Salt + Password) = Salted Hash`

### Storage
The salt is NOT a secret. It is stored in plaintext directly alongside the hash in the database. When the user logs in, the server retrieves their specific salt, appends it to the incoming password attempt, hashes the combination, and checks if it matches the stored hash.

Because every user has a unique salt, a Rainbow Table attack is completely neutralized.

---

## 5. Pepper

### What is a Pepper?
While a salt is unique per user and stored in the database, a **Pepper** is a global secret key used across all users, and it is *never* stored in the database.

### Why is it useful?
If an attacker steals your database via SQL Injection, they get the hashes and the salts. They can take these offline and start cracking them.
However, if you used a pepper, they cannot crack the hashes unless they also steal the pepper. 

### Storage
The pepper must be stored securely in the application's environment variables or a Secret Manager (like AWS KMS or HashiCorp Vault), completely separate from the database.

---

## 6. Password Hashing Algorithms

Not all hashing algorithms are suitable for passwords. General-purpose cryptographic hashes like **MD5, SHA-1, and SHA-256** are designed to be extremely fast. This is great for verifying file integrity, but terrible for passwords. An attacker with modern GPUs can compute billions of SHA-256 hashes per second, rapidly brute-forcing your database.

You must use a **Key Derivation Function (KDF)** designed specifically for passwords. These algorithms are intentionally slow and computationally expensive.

| Algorithm | Password Storage | Speed | Salt | Modern Recommendation |
| --------- | ---------------- | ----- | ---- | --------------------- |
| MD5 | ❌ NEVER | Extremely Fast | Manual | Severely Compromised |
| SHA-256 | ❌ NEVER | Very Fast | Manual | Too fast for passwords |
| PBKDF2 | ⚠️ Acceptable | Adjustable CPU | Built-in | Legacy Standard (NIST) |
| bcrypt | ✅ Recommended | Adjustable CPU | Built-in | Industry Standard |
| scrypt | ✅ Recommended | CPU + Memory | Built-in | Strong against ASICs |
| Argon2 | 🏆 Best in Class | CPU + Memory | Built-in | Winner of PHC |

*Note: Argon2id is currently the most secure algorithm available, as it is resistant to both GPU cracking and side-channel timing attacks.*

---

## 7. Password Policies

How strict should password rules be?

### The Old Way (NIST Special Publication 800-63B - Deprecated)
"Must be 8 characters, contain one uppercase, one number, one special character, and expire every 90 days."
*Why it failed:* Users just changed `Password1!` to `Password2!`. It encouraged predictable, weak passwords and frustrated users.

### The Modern Way (Current NIST Guidelines)
- **Minimum Length:** At least 8 characters (12+ preferred).
- **Maximum Length:** At least 64 characters to support passphrases and password managers.
- **No Arbitrary Complexity:** Drop the "must have special characters" rule.
- **No Periodic Expiration:** Do not force users to change passwords every 90 days unless there is evidence of a breach.
- **Breach Detection:** Check the password against a list of known compromised passwords (e.g., HaveIBeenPwned API) during registration.

---

## 8. Password Reset

The "Forgot Password" flow is a prime target for attackers.

```text
Forgot Password
      ↓
Email / Recovery Request
      ↓
Generate Secure Reset Token
      ↓
Send Secure Link via Email
      ↓
User clicks link (Validate Token)
      ↓
Set New Password
      ↓
Invalidate Existing Sessions
```

### Security Concerns
- **Token Entropy:** The reset token must be a highly random string (e.g., 32 bytes from a CSPRNG) or a signed JWT.
- **Expiration:** Reset links must expire quickly (e.g., 15-30 minutes).
- **Single Use:** Once a password is changed, the reset token must be instantly destroyed.
- **Session Revocation:** Changing a password MUST invalidate all active sessions for that user across all devices.

---

## 9. Account Recovery

If a user loses their password and their MFA device, how do they get back in?

- **Recovery Codes:** Provide 10 static, single-use codes during MFA setup. Users print these and put them in a safe.
- **Email Recovery:** Sending a reset link to a verified fallback email.
- **Recovery Abuse:** Attackers often use social engineering (calling customer support) to bypass authentication by pretending to have lost their credentials. Automated recovery codes remove the human vulnerability.

---

## 10. Password Attacks

### Brute Force
- **Attack:** An attacker tries every possible combination of characters (a, b, c... aa, ab) against a single account.
- **Detection:** High volume of failed logins for one username from a single IP.
- **Prevention:** Account lockouts (e.g., lock after 5 failed attempts) or progressive delays (rate limiting).

### Credential Stuffing
- **Attack:** Attackers take lists of usernames and passwords breached from *Site A*, and automate login attempts against *Site B*. 
- **Why it works:** Because users reuse passwords.
- **Prevention:** Multi-Factor Authentication (MFA). Even if the attacker has the correct password, they lack the second factor.

### Password Spraying
- **Attack:** Instead of trying 10,000 passwords on 1 account (which triggers lockouts), the attacker tries 1 common password (like `Spring2026!`) across 10,000 different accounts.
- **Detection:** Difficult. Looks like a few failed logins across many accounts.
- **Mitigation:** Enforcing strong passwords at registration, preventing common passwords, and IP-based rate limiting.

### Dictionary Attacks
- **Attack:** Hashing every word in the dictionary and comparing it to the stolen database hashes.
- **Prevention:** Using salts neutralizes this completely, forcing the attacker to compute the dictionary specifically for every single user.

---

## Common Mistakes
- **Rolling your own crypto:** Never try to invent a hashing algorithm. Use standard libraries for bcrypt or Argon2.
- **Using fast hashes:** Using SHA-256 or MD5 for passwords.
- **Not limiting login attempts:** Allowing an attacker to submit 1,000 passwords a second to your `/login` endpoint.
- **Leaking account existence:** If a user resets a password for an email not in your system, do not say "Email not found." Say "If that email exists, a reset link has been sent." This prevents attackers from enumerating your user base.

## Best Practices
- Store passwords using a dedicated password hashing algorithm such as **Argon2id** or **bcrypt** with a high work factor.
- Implement strict rate-limiting on all authentication endpoints (`/login`, `/register`, `/reset-password`).
- Encourage or enforce the use of Multi-Factor Authentication (MFA).

---

## Summary
Passwords are the most common form of authentication. Because they are vulnerable to human error (reuse, weak choices), the engineering systems supporting them must be robust. Never store passwords in plaintext, always use slow cryptographic hashes (like bcrypt or Argon2) with unique salts, and defend your login routes against automated stuffing and spraying attacks.

---

**Previous:** [← Authentication Foundations](./part-01-foundations.md)

**Next:** [Sessions and Cookies →](./part-03-sessions-and-cookies.md)
