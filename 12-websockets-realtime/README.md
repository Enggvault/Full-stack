# WebSockets & Real-Time

This module covers real-time communication between the client and server. Standard HTTP is strictly request-response; the server cannot push data to the client unprompted. For applications like chat, live sports scores, or collaborative editing, you need a persistent connection.

You will learn the differences between Polling, Server-Sent Events (SSE), and WebSockets, and how to scale stateful WebSocket connections across multiple servers.

## What You Will Learn
* Polling vs. Long Polling vs. Server-Sent Events vs. WebSockets
* The WebSocket lifecycle and HTTP Upgrade handshake
* Real-Time Architecture (using Redis Pub/Sub to scale WebSockets)
* Connection security and authentication
* Node.js implementation example

## Prerequisites
* [05-http-json-fetch](../05-http-json-fetch/notes.md)
* [07-nodejs-express](../07-nodejs-express/notes.md)
* [11-message-queues](../11-message-queues/notes.md)

## Roadmap Position
[11 Message Queues](../11-message-queues/notes.md) → **12 WebSockets & Real-Time** → [13 Microservices](../13-microservices/notes.md)

## Contents
* [notes.md](./notes.md) - Full reference guide
