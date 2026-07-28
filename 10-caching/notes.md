title: "Caching & Performance: Complete Beginner to Advanced"
subtitle: "From Browser Caches to Global CDN & Redis Architectures"
author: "Principal Systems Engineer"
version: "1.0"
date: "2026"

# Caching & Performance
## Complete Beginner to Advanced Engineering Handbook

> A production-grade, book-quality reference covering every layer of caching, from browser headers to distributed Redis clusters and cache stampede prevention.

> **Prerequisites:** [09 — Authentication ←](../09-authentication/notes.md) · **Next:** 11 — System Design (Planned) →

## Table of Contents

### Part I: Caching Foundations
- [Chapter 1: What is Caching?](#chapter-1-what-is-caching)
- [Chapter 2: Why Do We Cache?](#chapter-2-why-do-we-cache)
- [Chapter 3: The Cache Key](#chapter-3-the-cache-key)

### Part II: Levels of Caching
- [Chapter 4: Client-Side Caching (Browser)](#chapter-4-client-side-caching-browser)
- [Chapter 5: Content Delivery Networks (CDNs)](#chapter-5-content-delivery-networks-cdns)
- [Chapter 6: API Gateway & Reverse Proxy Caching](#chapter-6-api-gateway--reverse-proxy-caching)
- [Chapter 7: Application Level Caching (Redis/Memcached)](#chapter-7-application-level-caching-redis-memcached)
- [Chapter 8: Database Query Caching](#chapter-8-database-query-caching)

### Part III: Cache Management
- [Chapter 9: Cache Eviction Policies](#chapter-9-cache-eviction-policies)
- [Chapter 10: Cache Invalidation Strategies](#chapter-10-cache-invalidation-strategies)
- [Chapter 11: The Cache Stampede Problem](#chapter-11-the-cache-stampede-problem)

### Part IV: Reference
- [Chapter 12: Interview Preparation](#chapter-12-interview-preparation)
- [Chapter 13: Production Checklist](#chapter-13-production-checklist)
- [Chapter 14: Cheat Sheet](#chapter-14-cheat-sheet)

---

# CHAPTER 1: What is Caching?

Caching is the process of storing copies of data in a temporary, high-speed storage layer so that future requests for that data are served faster. 

Whenever a client requests data, the system checks the cache first (a **Cache Hit**). If the data is not there (a **Cache Miss**), the system fetches it from the primary, slower data store, returns it to the client, and saves a copy in the cache for the next time.

# CHAPTER 2: Why Do We Cache?

1. **Reduce Latency:** Reading from RAM (Redis) takes microseconds. Reading from a disk (Database) takes milliseconds. Reading from a remote server takes hundreds of milliseconds.
2. **Reduce Load:** By intercepting traffic, caches protect fragile primary databases from being overwhelmed during traffic spikes.
3. **Save Money:** Serving static assets from a CDN is orders of magnitude cheaper than serving them from expensive compute instances.

# CHAPTER 3: The Cache Key

Caches operate as Key-Value stores. The **Cache Key** is a unique identifier used to retrieve the data.

**Bad Cache Key:** `user_profile` (Will return the same profile for everyone).
**Good Cache Key:** `user_profile:u_12345` (Returns a specific user's profile).

Keys must be deterministic and include all variables that change the output (e.g., if a query depends on `userId` and `sortBy`, the key must include both).

# CHAPTER 4: Client-Side Caching (Browser)

The fastest request is the one that never hits the network. Browsers cache assets (images, CSS, JS) locally.

### Cache-Control Header
The server controls browser caching using the `Cache-Control` HTTP header.
- `Cache-Control: public, max-age=31536000` (Cache for 1 year. Used for static assets with hashed filenames like `app.v2.js`).
- `Cache-Control: no-cache` (Forces the browser to validate with the server before using the cached copy via ETags).
- `Cache-Control: no-store` (Never cache this. Used for sensitive data like bank balances).

### ETags (Entity Tags)
An ETag is a hash of the file's contents. 
1. Server sends: `ETag: "33a64df551425fcc55e4d42a148795d9f25f89d4"`
2. On the next request, Browser asks: `If-None-Match: "33a64df551425fcc55e4d42a148795d9f25f89d4"`
3. If the file hasn't changed, Server replies: `304 Not Modified` (Empty body, saving massive bandwidth).

# CHAPTER 5: Content Delivery Networks (CDNs)

A CDN (e.g., Cloudflare, AWS CloudFront, Fastly) is a globally distributed network of cache servers.
- **Problem:** If your server is in New York, a user in Tokyo experiences 200ms of speed-of-light latency.
- **Solution:** A CDN places servers (Edge nodes) in Tokyo. When the first user in Tokyo requests an image, the CDN fetches it from New York (Cache Miss) and stores it in Tokyo. The next 10,000 users in Tokyo get the image directly from the local node in 10ms (Cache Hit).

# CHAPTER 6: API Gateway & Reverse Proxy Caching

Tools like Nginx or API Gateways can cache entire HTTP responses. If thousands of users request `GET /api/v1/leaderboard`, Nginx can cache the JSON response for 60 seconds. During that minute, your Node.js application receives zero traffic for that route.

# CHAPTER 7: Application Level Caching (Redis/Memcached)

When the application must perform expensive logic or complex database joins, it stores the computed result in an in-memory datastore like Redis.

```javascript
async function getDashboardData(userId) {
  const cacheKey = `dashboard:${userId}`;
  
  // 1. Check Cache
  const cached = await redis.get(cacheKey);
  if (cached) return JSON.parse(cached); // Cache Hit

  // 2. Cache Miss: Do the heavy lifting
  const data = await database.query('SELECT complex_stuff...');
  
  // 3. Populate Cache with a TTL (Time-To-Live) of 5 minutes
  await redis.set(cacheKey, JSON.stringify(data), 'EX', 300);
  
  return data;
}
```

# CHAPTER 8: Database Query Caching

Some databases (like MySQL) historically tried to cache identical queries automatically. This is generally considered an anti-pattern today because any write to a table invalidates the entire cache for that table, leading to massive overhead. It is better to use Redis.

# CHAPTER 9: Cache Eviction Policies

When a cache fills up, it must evict old data to make room for new data.
- **LRU (Least Recently Used):** Evicts the item that hasn't been accessed for the longest time. (Industry standard for most use cases).
- **LFU (Least Frequently Used):** Evicts the item accessed the fewest times. Good for tracking highly viral content.
- **FIFO (First In, First Out):** Evicts the oldest item, regardless of how often it is accessed.

# CHAPTER 10: Cache Invalidation Strategies

"There are only two hard things in Computer Science: cache invalidation and naming things." - Phil Karlton

1. **Write-Through:** Data is written to the Database and the Cache simultaneously. Keeps cache always fresh, but increases write latency.
2. **Write-Around:** Data is written to the Database, bypassing the Cache. The Cache is only populated on a read miss.
3. **Write-Back:** Data is written to the Cache only. The Cache acknowledges the write immediately, and asynchronously syncs to the Database. Dangerous (data loss if cache crashes) but extremely fast.
4. **TTL (Time-To-Live):** The most common approach. Set an expiration (e.g., 5 minutes) and let the cache self-invalidate. Accept that data might be stale for 5 minutes.

# CHAPTER 11: The Cache Stampede Problem

A **Cache Stampede** (or Thundering Herd) occurs when a highly requested cached item (like the homepage of a news site) expires. 

If 10,000 concurrent users request the homepage at the exact millisecond the cache expires, they all experience a Cache Miss. All 10,000 requests hit the primary database simultaneously to recalculate the homepage, instantly crashing the database.

**Solutions:**
1. **Locking (Mutex):** When a miss occurs, acquire a Redis lock. Only the first request goes to the database; the other 9,999 wait and poll the cache.
2. **Probabilistic Early Expiration (XFetch):** A background thread randomly recalculates the cache *before* it strictly expires.

# CHAPTER 12: Interview Preparation

### Beginner
1. **What is a Cache Miss?**
   *Answer:* When requested data is not found in the cache, forcing the system to retrieve it from the slower primary datastore.
2. **What does a CDN do?**
   *Answer:* A CDN distributes cached copies of static assets (like images and JS) to edge servers globally, drastically reducing geographic latency for users far from the origin server.

### Intermediate
3. **Explain the difference between LRU and LFU.**
   *Answer:* LRU (Least Recently Used) evicts items that haven't been accessed recently, focusing on recency. LFU (Least Frequently Used) evicts items with the lowest total access count, focusing on overall popularity.
4. **What is an ETag and how does it save bandwidth?**
   *Answer:* An ETag is a hash of a file's contents. The browser sends it in the `If-None-Match` header. If the file on the server hasn't changed, the server returns `304 Not Modified` with an empty body, saving the bandwidth of re-downloading the file.

### Advanced
5. **How do you prevent a Cache Stampede (Thundering Herd)?**
   *Answer:* A cache stampede happens when a popular cached item expires, and thousands of concurrent requests instantly hit the database. You can prevent it by implementing a mutex lock in Redis (so only one worker fetches the new data) or by using Probabilistic Early Recomputation, where the cache is refreshed *before* it actually expires.

# CHAPTER 13: Production Checklist

- [ ] Static assets have `Cache-Control: public, max-age=31536000` and use hashed filenames (e.g., `main.a4b3.js`).
- [ ] Sensitive API endpoints explicitly return `Cache-Control: no-store`.
- [ ] API Gateway / Nginx is configured to strip cookies before caching public responses to prevent cross-user data leakage.
- [ ] Redis is configured with an explicit eviction policy (e.g., `allkeys-lru`) and a memory limit (`maxmemory`).
- [ ] All Application-Level cached items have a strict TTL to prevent stale data accumulation.
- [ ] Heavy / viral endpoints are protected against Cache Stampedes (e.g., using locks).

# CHAPTER 14: Cheat Sheet

| Strategy | Description | Best For |
| :--- | :--- | :--- |
| **Write-Through** | Write to Cache & DB simultaneously | Data that must never be stale |
| **Write-Around** | Write to DB, Cache populated on Read | Data written once, read rarely |
| **Write-Back** | Write to Cache, Async to DB | Extreme write-heavy workloads |
| **TTL Expiry** | Expire after X seconds | Leaderboards, Analytics, Dashboards |

> **Next Module →** 11 — System Design (Planned)
> **Previous Module ←** [09 — Authentication](../09-authentication/notes.md)
