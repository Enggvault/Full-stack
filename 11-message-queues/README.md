# Message Queues

This module introduces asynchronous processing and background workers. When an HTTP request takes too long to process (e.g., sending an email, processing an image), the work should be offloaded to a message queue.

You will learn how to decouple systems, guarantee message delivery, and safely scale background workers without overwhelming your database.

## What You Will Learn
* Synchronous vs. Asynchronous processing
* The anatomy of a Message Queue (Producers, Consumers, Brokers)
* Message delivery guarantees (At-most-once, At-least-once, Exactly-once)
* Handling failures with Retries and Dead-Letter Queues (DLQ)
* Comparing Kafka, RabbitMQ, Redis Streams, and SQS
* Background jobs in production

## Prerequisites
* [07-nodejs-express](../07-nodejs-express/notes.md)
* [08-databases](../08-databases/notes.md)
* [10-caching](../10-caching/notes.md)

## Roadmap Position
[10 Caching](../10-caching/notes.md) → **11 Message Queues** → [12 WebSockets + Real-Time](../12-websockets-realtime/notes.md)

## Contents
* [notes.md](./notes.md) - Full reference guide
