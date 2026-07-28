title: "Caching"
subtitle: "Performance, Scalability, and Redis"
author: "Principal Systems Engineer"
version: "1.0"
date: "2026"

# Caching
## Engineering Reference

> **Prerequisites:** [09 — Authentication ←](../09-authentication/notes.md) · **Next:** [11 — Message Queues →](../11-message-queues/notes.md)

---

## 1. Fundamentals

- **What is caching?** Storing copies of frequently accessed data in a fast, temporary storage layer (usually RAM) so that future requests can be served significantly faster than querying the primary database.
- **Why caching exists:** Reading from RAM is ~100,000 times faster than reading from a spinning disk, and avoids expensive SQL `JOIN`s and network round-trips. It is the primary way to survive massive traffic spikes.
- **Cache Hit:** The requested data was found in the cache. Fast response.
- **Cache Miss:** The requested data was *not* in the cache. The system must fetch it from the database (slow) and then store it in the cache for next time.
- **Hit Ratio:** The percentage of requests served from the cache. A 90% hit ratio is excellent.
- **TTL (Time to Live):** The lifespan of a cached item before it is automatically deleted.
- **Expiration:** Deleting data when its TTL hits zero.
- **Eviction:** Deleting data *before* its TTL hits zero because the cache ran out of memory (e.g., LRU: Least Recently Used).

## 2. Cache Strategies

How the application interacts with the cache and database.

- **Cache-aside (Lazy Loading):** The application checks the cache first. If it's a miss, the application queries the DB, writes to the cache, and returns the data. (Most common).
- **Read-through:** The application *only* asks the cache for data. If it's a miss, the *cache itself* fetches the data from the DB.
- **Write-through:** When updating data, the application writes it to both the cache and the DB simultaneously. Slower writes, but guarantees the cache is never stale.
- **Write-behind (Write-back):** The application writes *only* to the cache, which responds immediately. The cache asynchronously writes to the DB later. Blazing fast, but risks data loss if the cache crashes.
- **Write-around:** The application writes directly to the DB, bypassing the cache. Used for data that is written once but rarely read.
- **Refresh-ahead:** The cache automatically refreshes popular data *before* it expires.

## 3. HTTP Caching

Browsers and CDNs can cache API responses, reducing load on your backend to zero.

- **Cache-Control:** The primary HTTP header for caching (e.g., `Cache-Control: public, max-age=3600` caches for 1 hour).
- **ETag:** A hash of the response body. The browser sends `If-None-Match: "hash"`. If the data hasn't changed, the server returns `304 Not Modified` (empty body) instead of sending the data again.
- **Last-Modified:** Similar to ETag, but uses timestamps (`If-Modified-Since`).
- **Expires:** Legacy header defining an absolute date/time when the cache expires.
- **Browser Cache:** Lives on the user's laptop.
- **CDN Cache (Content Delivery Network):** Lives in edge servers globally (Cloudflare, AWS CloudFront).
- **Reverse Proxy Cache:** Lives on your infrastructure in front of your API (Nginx, Varnish).

## 4. Redis

Redis is an open-source, in-memory, key-value data store. It is the industry standard for backend caching.

- **Key-Value Model:** Everything is stored under a string key (e.g., `user:123`).
- **Strings:** The most basic type, can hold text, JSON strings, or numbers.
- **Lists:** Linked lists. Great for recent items (e.g., "Top 10 latest tweets").
- **Sets:** Unordered collections of unique strings. (e.g., tracking unique IP addresses).
- **Sorted Sets (ZSET):** Sets ordered by a "score". Perfect for Leaderboards.
- **Hashes:** Maps mapping string fields to string values (representing objects).
- **Pub/Sub:** Publisher/Subscriber messaging pattern for real-time communication.
- **Streams:** An append-only log, similar to Kafka.
- **Transactions:** Using `MULTI` and `EXEC` to group commands.
- **Lua Scripts:** Run custom scripts directly inside Redis atomically (used for rate limiting).
- **Persistence:** Redis can save snapshots to disk (RDB) or log every write (AOF) to survive reboots, even though it operates in RAM.

## 5. Redis Architecture

- **Replication:** A Primary Redis node handles writes, and replicates data to multiple Replica nodes which handle reads.
- **Redis Sentinel:** Monitors the Primary. If the Primary crashes, Sentinel automatically promotes a Replica to Primary (High Availability).
- **Redis Cluster:** Automatically shards data across multiple Primary nodes, allowing Redis to hold more data than can fit on a single machine's RAM.

## 6. Cache Problems

Caching introduces massive complexity.

- **Cache Stampede (Thundering Herd):** A popular cache key (e.g., the Homepage) expires. Suddenly, 10,000 concurrent requests all experience a Cache Miss simultaneously. They all hit the database at the same time, instantly crashing it. (Solved by debouncing DB calls or soft TTLs).
- **Cache Avalanche:** A Redis server crashes, or thousands of keys expire at the exact same millisecond, shifting 100% of traffic to the database. (Solved by adding random "jitter" to TTLs).
- **Cache Penetration:** Attackers intentionally request IDs that don't exist in the database (e.g., `user:-1`). It misses the cache, hits the DB, misses the DB, and never gets cached. (Solved by caching `null` values or using Bloom Filters).
- **Hot Keys:** A single Redis key (e.g., "Super Bowl Score") gets 1,000,000 requests per second, overwhelming the single Redis node holding it. (Solved by local memory caching).
- **Stale Data:** The cache contains old data because the database was updated but the cache wasn't invalidated.

## 7. Cache Consistency

The hardest problem in computer science.

```text
Database
↓
Cache
```

When you update the Database, the Cache now holds incorrect data. You must either:
1. Wait for the TTL to expire organically.
2. Manually **Invalidate** (delete) the cache key immediately after the DB updates.
If the manual invalidation fails (network error), the data remains permanently out of sync until the TTL expires.

## 8. Application Caching Example (Node.js)

```typescript
import { createClient } from 'redis';
const redis = createClient();
await redis.connect();

async function getUserProfile(userId: string) {
  const cacheKey = `user:${userId}`;
  
  // 1. Check Cache
  const cachedData = await redis.get(cacheKey);
  if (cachedData) return JSON.parse(cachedData);
  
  // 2. Cache Miss - Query DB
  const user = await db.users.find(userId);
  
  // 3. Populate Cache with TTL (3600 seconds)
  await redis.setEx(cacheKey, 3600, JSON.stringify(user));
  
  return user;
}
```

## 9. Distributed Caching

- **Shared Cache:** In a microservices environment, instead of each Node.js server keeping data in its own RAM (which leads to server A having different data than server B), all servers connect to a centralized Redis cluster.
- **Invalidation:** If Server A updates a user, it deletes `user:123` from Redis. Next time Server B needs that user, it experiences a cache miss and fetches the fresh data.

## 10. Production

- **Monitoring:** Track your Hit Ratio. If it's below 50%, your cache might be too small (high eviction rate).
- **Memory Usage:** Monitor RAM. Redis stops accepting writes if it hits its `maxmemory` limit.
- **Eviction Policy:** Always set a policy like `volatile-lru` so Redis automatically deletes old keys to make room for new ones.
- **Failure Handling (Fallback):** If Redis crashes, your Node.js app must gracefully degrade and query the DB directly, rather than crashing the entire API.

## 11. Design Examples

- **E-commerce (Product Page):** Rarely changes, highly read. Cache in Redis for 10 minutes. Cache static assets in CDN.
- **Social Media (Feed):** Extremely personalized. Do not cache the final HTML. Cache the individual posts, then assemble the feed dynamically.
- **APIs (Rate Limiting):** Use Redis `INCR` to count requests per IP address.
- **Dashboards (Analytics):** Heavy SQL `SUM()` queries. Pre-compute the results every 5 minutes in a background job and store the JSON in Redis.
- **User Profiles:** Cache-aside. Invalidate the cache key the moment the user clicks "Save Profile".

---

> **Next Module →** [11 — Message Queues](../11-message-queues/notes.md)
> **Previous Module ←** [09 — Authentication](../09-authentication/notes.md)
