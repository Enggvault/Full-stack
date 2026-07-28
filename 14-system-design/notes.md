title: "System Design"
subtitle: "Architecting Massive, Scalable Production Systems"
author: "Principal Systems Engineer"
version: "1.0"
date: "2026"

# System Design
## Engineering Reference

> **Prerequisites:** Modules 07 through 13.

---

## 1. What is System Design?

System design is the process of defining the architecture, components, modules, interfaces, and data for a system to satisfy specified requirements. It is the art of connecting everything learned in Modules 07–13 into a cohesive, scalable product.

## 2. Functional Requirements

What the system *must do* from a user's perspective. 
*Example (Twitter):* Users can post tweets, follow other users, and view a timeline.

## 3. Non-Functional Requirements

How the system *behaves* under the hood.
- **Scalability:** Can it handle 10x traffic?
- **Availability:** Does it stay up 99.99% of the time?
- **Reliability:** Does it work correctly?
- **Durability:** Is data safe from permanent loss?
- **Consistency:** Do all users see the exact same data at the same time?
- **Latency:** How fast does it respond?
- **Throughput:** How many requests per second (RPS) can it handle?
- **Security:** Is the data safe from attackers?

## 4. Core Building Blocks

- **Client:** The browser or mobile app.
- **DNS:** Translates `app.com` to an IP address.
- **CDN:** Caches static assets globally.
- **Load Balancer:** Distributes incoming traffic across multiple servers.
- **API Gateway:** Entry point for APIs (handles routing, auth, rate limiting).
- **Application Servers:** Node.js/Express backends running business logic.
- **Database:** Persistent storage (PostgreSQL/MongoDB).
- **Cache:** High-speed RAM storage (Redis).
- **Queue / Message Broker:** Asynchronous work (Kafka/RabbitMQ).
- **Object Storage:** Storing massive files/images (AWS S3).

## 5. Scalability

- **Vertical Scaling (Scaling Up):** Buying a bigger server (more RAM/CPU). Limited by hardware constraints.
- **Horizontal Scaling (Scaling Out):** Adding more servers to the pool. Limitless, but requires **Stateless Services** (e.g., storing Sessions in Redis, not in the server's RAM).

## 6. Database Scaling

- **Indexes:** Speed up reads at the cost of slightly slower writes.
- **Read Replicas:** Send all `SELECT` queries to Replica databases, keeping the Primary database free for `INSERT/UPDATE` queries.
- **Partitioning:** Splitting a large table into smaller tables on the same server (e.g., by month).
- **Sharding:** Splitting a large database across completely different servers (e.g., Users A-M on Server 1, N-Z on Server 2). Very complex.

## 7. Caching

[See Module 10: Caching](../10-caching/notes.md). Place caches in front of the database to intercept heavy read traffic. Use TTLs or Write-Through policies to keep data fresh.

## 8. Queues

[See Module 11: Message Queues](../11-message-queues/notes.md). Use queues to decouple slow processes. The API server accepts the request, puts it in a queue, and returns `202 Accepted` immediately.

## 9. Real-Time

[See Module 12: WebSockets](../12-websockets-realtime/notes.md). Use WebSockets and Redis Pub/Sub for features requiring instant server-to-client push (e.g., chat, live scores).

## 10. Microservices

[See Module 13: Microservices](../13-microservices/notes.md). Break the monolith apart only when organizational scaling requires it.

## 11. Reliability

- **Retry / Timeout:** Never hang forever; always timeout. Retry with Exponential Backoff.
- **Circuit Breaker:** If a downstream service fails, "open the circuit" to fail fast and let it recover.
- **Redundancy:** Eliminate Single Points of Failure (SPOF) by running multiple instances of every component across different physical data centers.

## 12. CAP Theorem

In a distributed data store, you can only guarantee two out of three:
- **Consistency:** Every read receives the most recent write.
- **Availability:** Every request receives a non-error response.
- **Partition Tolerance:** The system continues to operate despite network failures dropping messages between nodes.

*Reality:* Networks *always* fail, so you must choose Partition Tolerance. Therefore, you are forced to choose between **Consistency (CP)** or **Availability (AP)**.

## 13. Consistency Models

- **Strong Consistency (CP):** If you write data, the very next read is guaranteed to see it. (e.g., Banking systems). Slower.
- **Eventual Consistency (AP):** If you write data, it might take a few seconds to propagate to all read replicas. Faster, highly available. (e.g., Social media likes).

## 14. Availability & Observability

- **SLA / SLO:** Service Level Agreement (contractual uptime, e.g., 99.9%) / Service Level Objective (internal goal).
- **Observability:** You cannot fix what you cannot see. Combine **Logs** (what happened), **Metrics** (how often it happened, e.g., CPU %), and **Traces** (following a single request across 5 microservices).

## 15. Security

[See Module 09: Authentication](../09-authentication/notes.md). Ensure HTTPS, strict CORS, rate limiting, and secure JWT/Session handling.

## 16. System Design Process

When designing a system, follow this exact flow:
1. **Clarify requirements:** (Functional and Non-Functional).
2. **Estimate scale:** (RPS, Storage).
3. **Define APIs:** (Endpoints, Payloads).
4. **Design data model:** (SQL vs NoSQL, schemas).
5. **High-level architecture:** (Draw the boxes).
6. **Database selection & Scaling.**
7. **Caching strategy.**
8. **Async processing (Queues).**
9. **Failure handling (Reliability).**
10. **Security & Observability.**
11. **Trade-offs:** (What did you sacrifice for speed?)

## 17. Capacity Estimation

- **Requests Per Second (RPS):** `Daily Active Users * Daily Requests Per User / 86,400 seconds`.
- **Storage:** `Daily New Items * Size per Item * 365 days`.
- **Bandwidth:** `RPS * Average Response Size`.

---

## 18. Real System Design Examples

### 1. URL Shortener (e.g., Bitly)
- **Requirements:** Generate short aliases; redirect to original URL; highly available.
- **APIs:** `POST /api/v1/data/shorten`, `GET /{alias}`.
- **Architecture:** Client → API Gateway → Application Server.
- **Database:** Key-Value store or NoSQL (MongoDB) holding `alias` -> `long_url`.
- **Cache:** Heavily cache the `GET` route in Redis. Reads vastly outnumber writes (100:1).
- **Trade-offs:** AP system; eventual consistency is fine for analytics.

### 2. Chat Application (e.g., WhatsApp)
- **Requirements:** 1-on-1 chat, group chat, online presence.
- **Architecture:** Client → WebSocket Server.
- **Scaling:** Use Redis Pub/Sub to broadcast messages across hundreds of stateful WebSocket servers.
- **Database:** NoSQL (Cassandra/DynamoDB) for fast, massive write ingestion of chat history.

### 3. Social Media Feed (e.g., Twitter)
- **Requirements:** Post tweets, view home timeline.
- **Architecture:** Fan-out on write vs Fan-out on read.
- **Queue:** When a user tweets, push an event to Kafka. Workers calculate the timeline for all followers and push it to Redis (Fan-out on write).
- **Trade-offs:** For celebrities with 10M followers, use Fan-out on read to prevent queue backing up.

### 4. E-commerce Platform (e.g., Amazon)
- **Requirements:** Product catalog, shopping cart, checkout.
- **Architecture:** Microservices (Catalog, Cart, Order, Payment).
- **Database:** PostgreSQL (strict ACID) for Orders/Payments. NoSQL/Search for Catalog.
- **Cache:** Heavy CDN and Redis caching for product pages.

### 5. Food Delivery (e.g., UberEats)
- **Requirements:** View restaurants, track driver location in real-time.
- **Real-Time:** Drivers push location via WebSockets. Customers subscribe to driver location via WebSockets.
- **Database:** Geospatial indexing (PostGIS or Redis Geospatial) to find nearby drivers.

### 6. Ride Sharing (e.g., Uber)
- **Requirements:** Match rider to driver, calculate fare, track ride.
- **Architecture:** Complex Event-Driven Architecture. Matchmaking service uses geospatial queries.
- **Queue:** Kafka to ingest millions of GPS pings per second.

### 7. Video Streaming (e.g., Netflix)
- **Requirements:** Upload video, stream video smoothly globally.
- **Architecture:** Upload to S3 → Queue → Workers transcode video into 10 different resolutions → Save back to S3.
- **CDN:** Push videos to Edge CDNs globally. The application servers only handle metadata/auth, not video streaming.

### 8. Notification System
- **Requirements:** Send Push, SMS, and Email to millions of users.
- **Architecture:** API Server → Kafka → Notification Workers → 3rd Party APIs (Twilio, SendGrid).
- **Reliability:** Strict Retry and DLQ mechanisms to handle 3rd Party API outages.

### 9. File Storage (e.g., Google Drive)
- **Requirements:** Upload, download, sync files across devices.
- **Architecture:** Client calculates file checksum. If file exists on server, skip upload (Deduplication). Upload files in chunks directly to S3 using Presigned URLs to bypass the API server.

### 10. API Rate Limiter
- **Requirements:** Limit a user to 100 requests per minute.
- **Architecture:** Middleware in the API Gateway checks Redis before forwarding the request.
- **Algorithm:** Use Token Bucket or Sliding Window algorithm in Redis via Lua scripts.

### 11. Distributed Cache (Building Redis)
- **Requirements:** Fast key-value store, highly available.
- **Architecture:** In-memory hash map.
- **Scaling:** Consistent Hashing to distribute keys across 100 servers. Replication for durability.

### 12. Search System (e.g., Google Search)
- **Requirements:** Crawl web, index words, return search results fast.
- **Architecture:** Crawler workers → Queue → Indexer workers → Inverted Index Database (Elasticsearch).
- **Database:** The Inverted Index maps words directly to Document IDs for blazing-fast lookups.

### 13. Payment System (e.g., Stripe)
- **Requirements:** Process payments, zero dropped transactions.
- **Architecture:** Strong Consistency (CP). Strict PostgreSQL ACID transactions.
- **Reliability:** Idempotency keys are mandatory to prevent charging a user twice during a network retry.

### 14. Real-Time Collaboration (e.g., Google Docs)
- **Requirements:** Multiple users typing simultaneously without overwriting each other.
- **Architecture:** WebSockets.
- **Algorithm:** Operational Transformation (OT) or Conflict-Free Replicated Data Types (CRDTs) to merge concurrent edits mathematically.
