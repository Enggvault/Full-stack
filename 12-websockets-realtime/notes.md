title: "WebSockets & Real-Time Systems"
subtitle: "Building Persistent, Bi-Directional Communication"
author: "Principal Systems Engineer"
version: "1.0"
date: "2026"

# WebSockets & Real-Time Systems
## Engineering Reference

> **Prerequisites:** [11 — Message Queues ←](../11-message-queues/notes.md) · **Next:** [13 — Microservices →](../13-microservices/notes.md)

---

## 1. Fundamentals of Real-Time

Standard HTTP is strictly Request-Response. The server cannot send data to the client unless the client asks for it. For real-time features (chat, live stock prices), we need alternative mechanisms.

- **Short Polling:** The client sends an HTTP request every 3 seconds to check for new data. 
  - *Pros:* Simple to implement.
  - *Cons:* Wastes massive amounts of bandwidth and server CPU (99% of requests return "no new data").
- **Long Polling:** The client sends a request. The server holds the request open until new data is available, then responds. The client immediately opens a new request.
  - *Pros:* Reduces empty responses.
  - *Cons:* High server memory usage (holding open thousands of HTTP connections).
- **Server-Sent Events (SSE):** A unidirectional HTTP connection where the server streams data to the client.
  - *Pros:* Built on standard HTTP, works perfectly with existing load balancers.
  - *Cons:* Unidirectional (Server → Client only). 
- **WebSockets:** A persistent, fully bi-directional TCP connection. Both client and server can send messages at any time.

## 2. The WebSocket Protocol

WebSockets (`ws://` and `wss://`) provide full-duplex communication over a single TCP connection. 

- **Persistent Connection:** Once open, the connection stays open until explicitly closed. There is no HTTP overhead (headers, cookies) sent with every message, drastically reducing latency and bandwidth.
- **Frames:** Data is sent in small chunks called frames (text or binary).
- **Full-Duplex:** The client and server can send data simultaneously.

## 3. WebSocket Lifecycle

```text
Client                          Server
  |                               |
  |--- 1. HTTP GET (Upgrade) ---->|  (Client asks to switch to WebSocket)
  |<-- 2. HTTP 101 Switching -----|  (Server agrees, HTTP connection becomes TCP socket)
  |                               |
  |<====== 3. Open Socket =======>|
  |                               |
  |---- 4. Send Frame (Data) ---->|  (No HTTP headers attached)
  |<--- 5. Send Frame (Data) -----|
  |                               |
  |--- 6. Close Frame ----------->|  (Client disconnects)
  |<-- 7. Close Acknowledged -----|
```

## 4. Scaling WebSockets (The Challenge)

WebSockets break traditional stateless backend scaling.

- **The Problem:** In standard HTTP, a load balancer can send Request 1 to Server A, and Request 2 to Server B. With WebSockets, if Alice connects to Server A, and Bob connects to Server B, how does Alice send a chat message to Bob? Server A doesn't know Bob exists.
- **The Solution (Redis Pub/Sub):** All WebSocket servers connect to a central Redis Pub/Sub instance. 
  - Alice sends a message to Server A: "Hello Bob."
  - Server A publishes the message to Redis channel `chat:bob`.
  - Server B (and all other servers) are subscribed to `chat:bob`.
  - Server B receives the message from Redis and pushes it down the WebSocket to Bob.

## 5. Load Balancing & Sticky Sessions

Load balancers must be configured specifically for WebSockets.
- **Connection Draining:** If you deploy new code, you can't just shut down the server, or you'll instantly drop 10,000 WebSocket connections.
- **Sticky Sessions:** Often used during the initial HTTP Upgrade handshake to ensure a client talking to Server A completes the handshake with Server A.

## 6. Security

- **Authentication:** WebSockets cannot use standard HTTP `Authorization` headers during the Upgrade request in the browser API. Best practice is to authenticate via a query parameter (`wss://api.app.com/?token=abc`) or send the token as the very first WebSocket frame after connecting.
- **Connection Validation:** Validate the `Origin` header during the HTTP Upgrade request to prevent Cross-Site WebSocket Hijacking (CSWSH).
- **Rate Limiting:** A malicious client can blast thousands of WebSocket frames per second. You must implement rate limiting per socket connection.

## 7. Node.js Implementation Example

A concise implementation using `socket.io` (which provides automatic fallbacks to Long Polling if WebSockets fail):

```typescript
import { Server } from "socket.io";
import { createServer } from "http";

const httpServer = createServer();
const io = new Server(httpServer, {
  cors: { origin: "https://myfrontend.com" } // Prevent CSWSH
});

// Middleware for Authentication
io.use((socket, next) => {
  const token = socket.handshake.auth.token;
  if (isValid(token)) return next();
  return next(new Error("Authentication error"));
});

io.on("connection", (socket) => {
  console.log("Client connected:", socket.id);

  // Client joins a specific "room" (e.g., a chat channel)
  socket.on("join_room", (roomId) => {
    socket.join(roomId);
  });

  // Client sends a message
  socket.on("send_message", (data) => {
    // Broadcast the message to everyone in the room
    io.to(data.roomId).emit("new_message", data.message);
  });
});

httpServer.listen(3000);
```

## 8. Feature Architectures

- **Chat App:** Client joins a Room (WebSocket). Server broadcasts messages to that room.
- **Live Notifications:** Server listens to a Message Queue (Kafka/RabbitMQ). When a "User Registered" event fires, the WebSocket server pushes an alert to the Admin Dashboard client.
- **Multiplayer Systems (Gaming/Docs):** High-frequency updates. Use binary frames (ArrayBuffers) instead of JSON to save bandwidth. Rely heavily on client-side prediction to hide latency.

---

> **Next Module →** [13 — Microservices](../13-microservices/notes.md)
> **Previous Module ←** [11 — Message Queues](../11-message-queues/notes.md)
