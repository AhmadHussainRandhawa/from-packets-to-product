# The Roadmap

This repository follows a deliberate 8-phase curriculum, not a random collection of scripts. Each phase builds directly on the one before it — physics, then execution, then protocol, then systems — ending in a real product.

> Networking is a 4-layer machine: **how bytes move** → **how programs handle connections** → **how the web works** → **how real systems survive at scale**. This roadmap climbs all four, then adds security, deployment, and productization on top.

Progress is tracked with checkboxes below and updated as each project is completed. See the [root README](README.md) for the current phase-by-phase status at a glance.

---

## Phase 1 — Networking Physics (TCP/IP Core) ✅

**Goal:** Understand how data physically moves between machines.
**Answers:** *"What really happens when I send data over the internet?"*

Core concepts: IP (routing, TTL, MTU) · TCP handshake · sequence numbers & ACKs · retransmissions · flow control · congestion control (AIMD) · RTT, latency, throughput · `TIME_WAIT` & keep-alives · packet loss behavior

Tools: `tcpdump` · `wireshark` · `ss` · `netstat` · `tc`

- [x] TCP client/server in Python
- [x] Capture packets and analyze the handshake
- [x] Simulate packet loss and latency
- [x] Measure throughput vs. RTT

**Outcome:** Seeing the internet at packet level — no abstraction, no magic.

---

## Phase 2 — Sockets & Event-Driven Systems ✅

**Goal:** Understand how high-performance servers are built.
**Answers:** *"How do Nginx, Node, Redis handle millions of connections?"*

Core concepts: blocking vs. non-blocking sockets · `select` / `poll` / `epoll` · event loops · async I/O · backpressure · thread-per-connection vs. event-driven

Tools: Python `selectors` · `asyncio` · `strace` · `lsof`

- [x] Multi-client chat server (blocking, thread-per-connection)
- [x] Same server rebuilt on an event loop (`selectors`)
- [x] Hand-rolled minimal event loop from scratch
- [x] Load test both models and compare

**Outcome:** Real understanding of concurrency at the network level — the ability to build real servers.

---

## Phase 3 — HTTP & TLS (The Money Layer) ⏳

**Goal:** Master the protocol that runs the world.
**Answers:** *"How does SaaS, APIs, and microservices actually work?"*

Core concepts: HTTP/1.1 internals · keep-alive · chunked transfer · caching semantics · headers · HTTP/2 multiplexing · HTTP/3 / QUIC (conceptual) · TLS handshake · certificates

Tools: `curl -v` · `openssl s_client` · `nghttp`

- [ ] Write a custom HTTP server
- [ ] Add TLS to it
- [ ] Implement keep-alive
- [ ] Build an HTTP load tester

**Outcome:** Understanding the bloodstream of the internet — building APIs from first principles.

---

## Phase 4 — Load, Failure & Reality ⏳

**Goal:** Learn how systems behave under stress.
**Answers:** *"Why do real systems crash, and how do you prevent it?"*

Core concepts: load balancing strategies · timeouts · retries · idempotency · circuit breakers · rate limiting · thundering herd · backpressure

Tools: `wrk` / `hey` · `nginx` / `haproxy` · `locust`

- [ ] Build a reverse proxy
- [ ] Add a rate limiter
- [ ] Add retry logic + a circuit breaker
- [ ] Simulate failures

**Outcome:** Designing systems that don't die under load — real infrastructure thinking.

---

## Phase 5 — Observability (God Mode) ⏳

**Goal:** Gain x-ray vision into systems.
**Answers:** *"How do I see inside distributed systems?"*

Core concepts: metrics (p95, p99) · logs · traces · profiling · distributed tracing

Tools: Prometheus · Grafana · OpenTelemetry

- [ ] Instrument the reverse proxy from Phase 4
- [ ] Build a latency dashboard
- [ ] Add distributed tracing
- [ ] Find and fix a real performance bottleneck

**Outcome:** Seeing problems before others know they exist — the same practice used daily at Netflix, Stripe, and AWS.

---

## Phase 6 — Security & Hardening ⏳

**Goal:** Make systems production-grade.
**Answers:** *"How do I protect real systems?"*

Core concepts: TLS in practice · authentication gateways · OAuth/JWT basics · rate limiting for security · DDoS concepts

- [ ] Secure API gateway
- [ ] Automated TLS certificate handling
- [ ] Basic attack simulation

**Outcome:** The ability to ship real public systems safely.

---

## Phase 7 — Deployment & Cloud ⏳

**Goal:** Control infrastructure.
**Answers:** *"How do I run this in the real world?"*

Core concepts: Docker · CI/CD · reverse proxies · health checks · basic Kubernetes · cloud primitives

- [ ] Dockerize the reverse proxy
- [ ] Add a CI pipeline
- [ ] Simulate node failure

**Outcome:** Owning the full stack — from packet to product.

---

## Phase 8 — Productization (The Founder Phase) ⏳

**Goal:** Convert knowledge into a real product.

Build **one** of:
- Observability SaaS
- Performance analyzer
- API gateway
- Load testing platform
- Chaos engineering tool

**Product rules:** solves real engineering pain · uses the networking skills built above · hard to replicate · developer-focused.

> ⚠️ **Note:** once this phase begins, the resulting product will move into its **own dedicated repository** rather than living inside this learning-journey repo. This repo will link to it once it exists.

**Final outcome:** No longer "learning networking" — building infrastructure that other companies depend on.

---

## The identity this builds toward

Not a network engineer. Not a backend dev. Not a system admin.

A **systems-level infrastructure builder** who understands the internet at the machine level and builds products on top of it — from packets to product.