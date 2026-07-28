title: "Microservices & Distributed Architecture"
subtitle: "Building Scalable, Independent Services"
author: "Principal Systems Engineer"
version: "1.0"
date: "2026"

# Microservices & Distributed Architecture
## Engineering Reference

> **Prerequisites:** [12 — WebSockets & Real-Time ←](../12-websockets-realtime/notes.md) · **Next:** [14 — System Design →](../14-system-design/notes.md)

---

## 1. Fundamentals

- **Monolith:** The entire application (UI, business logic, data access) is packaged and deployed as a single unit. Easiest to develop, test, and deploy, but can become a tangled mess ("Big Ball of Mud") as teams grow.
- **Modular Monolith:** A single deployment unit, but internally organized into strict, decoupled modules (e.g., separating the `Users` module from the `Payments` module). Highly recommended before ever attempting microservices.
- **Microservices:** The application is broken into small, loosely coupled services that can be developed, deployed, and scaled entirely independently.
- **Service Boundaries:** The hardest part of microservices. Drawing lines incorrectly leads to a "Distributed Monolith" where services are so tightly coupled that if one fails, they all fail.

## 2. Service Design

- **Single Responsibility:** A microservice should do one thing well (e.g., the `PaymentService` only handles charging credit cards).
- **Bounded Context:** (From Domain-Driven Design). The definition of a "User" in the `ShippingService` (an address) is completely different from a "User" in the `AuthenticationService` (a hashed password). Don't try to create one giant, global `User` model. Keep them bounded.

## 3. Communication

How services talk to each other:
- **REST / HTTP (Synchronous):** Simple, standard. Problematic if service A needs data from B, C, and D, leading to massive latency.
- **gRPC (Synchronous):** High-performance RPC framework using binary Protocol Buffers instead of JSON. Excellent for internal service-to-service communication.
- **Messaging / Events (Asynchronous):** Using Kafka/RabbitMQ. Service A publishes "UserCreated", and Services B and C react to it. Extremely scalable and highly decoupled.

## 4. API Gateway

Clients (mobile apps, browsers) shouldn't memorize 50 different microservice IP addresses.
- **Routing:** The Gateway accepts all traffic at `api.app.com` and routes `/users` to the User Service and `/payments` to the Payment Service.
- **Offloading:** It handles cross-cutting concerns like **Authentication** (validating JWTs), **Rate Limiting**, and **SSL Termination**, keeping the internal microservices simple.

## 5. Service Discovery

If the `OrderService` needs to call the `PaymentService`, it needs an IP address. Since instances autoscale and change IPs constantly, we use:
- **Service Registry:** A database (like Consul or Eureka) tracking active IP addresses.
- **Kubernetes DNS:** In modern infrastructure, Kubernetes provides internal DNS. `http://payment-service.default.svc.cluster.local` automatically resolves to an active pod.

## 6. Distributed Data

**Golden Rule of Microservices:** A microservice must own its own data.
- **Database per Service:** The `OrderService` uses PostgreSQL, while the `AnalyticsService` uses MongoDB. The `OrderService` is the *only* entity allowed to touch the Order database. If the `UserService` wants an order, it must ask the `OrderService` via API.
- **Shared Database Problem:** If multiple services connect to the exact same database, a schema change by one team will instantly crash the other team's service.

## 7. Distributed Transactions

When a business process spans multiple services (e.g., Place Order → Reserve Inventory → Charge Card), standard ACID database transactions no longer work.

- **Two-Phase Commit (2PC):** A coordinator asks all services to prepare to commit, then tells them all to commit. Very slow, locks the databases, bad for scalability.
- **Saga Pattern:** A sequence of local transactions. If one step fails (e.g., Charge Card fails), the system triggers **Compensating Transactions** (e.g., Un-reserve Inventory, Cancel Order) to roll back the previous steps.
  - *Choreography:* Each service publishes an event, and the next service listens to it. (Decentralized).
  - *Orchestration:* A central "Saga Orchestrator" service coordinates the workflow. (Centralized).

## 8. Event-Driven Architecture (EDA)

Services communicate purely by emitting and listening to Events (e.g., "ItemAddedToCart").
- **Eventual Consistency:** In a distributed system, data is not instantly consistent everywhere. If a user updates their username, it might take 200ms for the `ReviewService` to receive the event and update the cached username on their past reviews.

## 9. Reliability

Because networks are unreliable, microservice calls *will* fail.

- **Timeout:** Never make an HTTP call without a timeout. If the downstream service freezes, your service will freeze waiting for it.
- **Retry:** Automatically retry failed requests (must use Exponential Backoff and ensure the endpoint is idempotent).
- **Circuit Breaker:** If a downstream service is failing 90% of the time, the Circuit Breaker trips and "opens." It instantly fails future requests without even trying the downstream service, giving the broken service time to recover.
- **Bulkhead Pattern:** Isolate resources. Don't let a crash in the `ReportGenerator` service use up all the CPU and take down the `Checkout` service.

## 10. Observability

When an error happens in a monolith, you check one log file. In microservices, the error spans 5 services.
- **Distributed Tracing:** When a request hits the API Gateway, it generates a `Correlation ID` (or Trace ID). This ID is passed in the HTTP headers to every subsequent service. Tools like Jaeger or Datadog stitch these logs together into a single trace.

## 11. Deployment

- **Containers (Docker):** Packages the service and its dependencies into a standard unit.
- **Kubernetes:** Orchestrates thousands of containers, handling load balancing, auto-scaling, and self-healing.
- **Rolling Deployment:** Slowly replaces old versions with new versions.
- **Blue-Green Deployment:** Run v1 (Blue) and v2 (Green) simultaneously. Flip the router from Blue to Green instantly.
- **Canary Deployment:** Send 5% of real user traffic to v2 to see if it crashes before rolling it out to 100%.

## 12. Security

- **mTLS (Mutual TLS):** Encrypts traffic *between* internal microservices and verifies identity (e.g., ensuring only the `OrderService` can call the `PaymentService`).
- **Secrets Management:** Use HashiCorp Vault or AWS Secrets Manager. Never hardcode passwords in containers.

## 13. Production Considerations

**Should you use Microservices?**
Probably not yet. Start with a Modular Monolith. Microservices solve organizational scaling problems (when you have 50+ engineers stepping on each other's toes). They introduce massive operational complexity (network latency, distributed tracing, sagas) that will slow down a small team drastically.

---

> **Next Module →** [14 — System Design](../14-system-design/notes.md)
> **Previous Module ←** [12 — WebSockets & Real-Time](../12-websockets-realtime/notes.md)
