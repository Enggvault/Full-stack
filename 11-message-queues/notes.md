title: "Message Queues & Asynchronous Processing"
subtitle: "Scaling Background Work with Kafka, RabbitMQ, and Redis"
author: "Principal Systems Engineer"
version: "1.0"
date: "2026"

# Message Queues & Asynchronous Processing
## Engineering Reference

> **Prerequisites:** [10 — Caching ←](../10-caching/notes.md) · **Next:** [12 — WebSockets & Real-Time →](../12-websockets-realtime/notes.md)

---

## 1. Fundamentals

- **Synchronous Processing:** The client sends a request and waits. The server performs all required work (e.g., resizing an image) before returning an HTTP response. If the work takes 10 seconds, the user stares at a loading spinner.
- **Asynchronous Processing:** The server immediately returns an HTTP `202 Accepted` response to the client, but pushes the heavy work to a background process to be completed later.
- **Message:** A packet of data containing instructions (e.g., `{"job": "send_email", "to": "user@example.com"}`).
- **Producer:** The application (usually the web server) that creates and sends the message.
- **Consumer (Worker):** A separate background server that constantly listens for new messages and processes them.
- **Queue / Broker:** The infrastructure layer (like RabbitMQ or Kafka) that sits between the Producer and Consumer, storing messages safely until they are processed.

## 2. Why Queues Exist

```text
HTTP Request → Web Server (Producer) → Queue → Worker (Consumer) → Database / 3rd Party
```

- **Performance:** Keeps HTTP response times fast (under 100ms) by deferring slow tasks.
- **Reliability:** If the 3rd-party Email Service goes down, the web server doesn't crash. The Queue holds the messages until the Email Service comes back online.
- **Decoupling:** The web server doesn't need to know *how* to resize an image; it just drops a message saying "resize this."

## 3. Queue Concepts

- **Topic / Exchange:** A routing mechanism. Producers publish to a Topic (e.g., `user_events`), and the broker routes it to the appropriate queue.
- **Partition:** Used in Kafka. A single Topic is split into multiple Partitions to allow parallel processing across multiple servers.
- **Offset:** A marker indicating which messages a Consumer has already read. 
- **Acknowledgment (ACK):** The Consumer tells the Broker, "I successfully processed this message. You can delete it." If the Consumer crashes before sending an ACK, the Broker gives the message to another worker.

## 4. Message Delivery

Message brokers offer different guarantees for edge cases (like network partitions or server crashes):

- **At-most-once (Fire and Forget):** Messages are delivered once, but if a crash happens, the message is lost. Fast, but unreliable.
- **At-least-once (The Standard):** Guarantees the message will be processed. However, if a Consumer crashes right *after* processing but *before* sending the ACK, the Broker will retry it, leading to a duplicate message.
- **Exactly-once:** Highly complex and computationally expensive. Usually requires the consumer to implement idempotency.

*Engineering Consideration:* Always assume **At-least-once** delivery. Your consumers must be designed to safely process duplicate messages without corrupting data.

## 5. Reliability

- **Retry & Exponential Backoff:** If a worker fails to send an email, it shouldn't retry instantly 1,000 times (which could DDoS the email provider). It should retry in 1 second, then 2, then 4, then 8 seconds.
- **Dead-Letter Queue (DLQ):** If a message fails completely after all retries (e.g., malformed JSON), it is moved to a DLQ. Engineers can inspect the DLQ manually to fix the bug and replay the messages later.
- **Poison Message:** A message that consistently crashes the consumer whenever attempted. DLQs are the primary defense against this.
- **Idempotency:** A function is idempotent if running it multiple times produces the same result as running it once (e.g., `UPDATE users SET verified=true`). Consumers must be idempotent to handle duplicate messages safely.

## 6. Technologies

| Technology | Core Model | Best Use Case |
| :--- | :--- | :--- |
| **RabbitMQ** | Smart Broker, Dumb Consumer | Traditional task queues, complex routing (e.g., Celery). |
| **Apache Kafka** | Dumb Broker (Append Log), Smart Consumer | Massive event streaming, big data pipelines, event sourcing. |
| **Redis Streams / BullMQ**| In-memory list/streams | Lightweight background jobs for Node.js apps. |
| **AWS SQS** | Cloud-native queue | Simple decoupling in AWS without managing infrastructure. |

## 7. Apache Kafka

- **Architecture:** Kafka is an immutable, append-only log. Messages are NOT deleted when a consumer reads them; they are retained for a set period (e.g., 7 days).
- **Consumer Groups:** Multiple workers act as a team. Kafka ensures each partition in a topic is read by exactly one worker in the group, allowing massive horizontal scaling.
- **Use Case:** Tracking billions of website clicks, distributed microservice communication.

## 8. RabbitMQ

- **Architecture:** Messages are pushed to an **Exchange**, which uses **Routing Keys** to distribute messages to specific **Queues**.
- **Ephemeral Messages:** Once a message is ACKed by a consumer, RabbitMQ deletes it from memory/disk immediately.
- **Use Case:** Standard job processing (e.g., encoding video, sending emails).

## 9. Background Jobs in Node.js

A concise example using `bullmq` (Redis-backed queue in Node.js):

```typescript
import { Queue, Worker } from 'bullmq';

// 1. Setup the Queue (Producer side)
const emailQueue = new Queue('Emails');

// Inside your Express controller:
app.post('/register', async (req, res) => {
  await db.users.create(req.body);
  // Offload email sending to the queue
  await emailQueue.add('welcome-email', { email: req.body.email });
  res.status(201).send("Registered!"); 
});

// 2. Setup the Worker (Consumer side, usually runs in a separate process)
const worker = new Worker('Emails', async (job) => {
  console.log(`Sending email to ${job.data.email}`);
  await emailService.send(job.data.email); // If this throws, BullMQ automatically retries
});
```

## 10. Production

- **Monitoring:** Always monitor queue depth (how many messages are waiting). If depth grows continuously, you need to spin up more Consumer servers.
- **Backpressure:** If the queue grows too fast, the Broker might run out of RAM/Disk. Systems must apply backpressure to Producers (telling them to slow down) to prevent total failure.
- **Consumer Scaling:** Consumers are usually stateless and easy to scale horizontally via Kubernetes/Auto-Scaling Groups.

---

> **Next Module →** [12 — WebSockets & Real-Time](../12-websockets-realtime/notes.md)
> **Previous Module ←** [10 — Caching](../10-caching/notes.md)
