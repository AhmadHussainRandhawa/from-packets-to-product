# Phase 2 — Sockets & Event-Driven Systems

**Goal:** Understand how high-performance servers are actually built.

This phase answers: *"How do Nginx, Node, and Redis handle millions of concurrent connections?"*

The approach here is deliberate: build the **same chat server four times**, each time with a different concurrency model, then **measure** the difference instead of taking it on faith.

---

## What's here

| # | Project | Concurrency Model |
|---|---|---|
| [1 — Blocking Chat Server](1-blocking-chat-server/) | `server.py` + `load_test.py` | Thread-per-connection (blocking sockets) |
| [2 — Event Loop Server](2-event-loop-server/) | `event_server.py` | Single-threaded, `selectors`-based (epoll/kqueue under the hood) |
| [3 — Tiny Event Loop](3-tiny-event-loop/) | `tiny_event_loop.py` + `chat_server.py` | A minimal event loop **built from scratch** on top of `select`, then a chat server built on top of *that* |
| [4 — Load Testing](4-load-testing/) | `load_test.py` (threaded) + `async_load_testing.py` (`asyncio`) | Benchmarking harnesses used to compare all three servers above |

---

## Concepts covered

- **Blocking vs. non-blocking sockets** — and why blocking I/O forces a thread-per-client model
- **`select` / `selectors`** — how an OS-level readiness API lets one thread monitor thousands of sockets
- **Event loops** — first using Python's built-in `selectors` module, then implementing the dispatch loop manually (`TinyEventLoop`) to remove the last bit of abstraction
- **Backpressure** — the event-loop server explicitly buffers outgoing data (`out_buffer`) and only re-registers a socket for `EVENT_WRITE` when there's unsent data, rather than assuming `send()` always completes
- **Thread-per-connection vs. event-driven trade-offs** — measured directly, not just asserted

## How to run each project

Each sub-project's server listens on `127.0.0.1:5000` by default — run one server at a time.

**1. Blocking chat server (thread-per-connection):**
```bash
python 1-blocking-chat-server/server.py
python 1-blocking-chat-server/load_test.py   # in a second terminal: 100 clients, 500 msgs each
```

**2. Event-loop server (`selectors`):**
```bash
python 2-event-loop-server/event_server.py
```

**3. Hand-rolled tiny event loop:**
```bash
cd 3-tiny-event-loop
python chat_server.py
```
`tiny_event_loop.py` implements `TinyEventLoop` — a minimal read/write callback dispatcher built directly on `select.select()`, with no dependency on Python's `selectors` module. `chat_server.py` runs the same broadcast-chat logic as project 2, but built entirely on this from-scratch loop.

**4. Load testing / benchmarking:**
```bash
python 4-load-testing/load_test.py            # threaded client, measures messages/sec
python 4-load-testing/async_load_testing.py   # asyncio client, scales to 10,000 concurrent connections
```
Point either harness at any of the three servers above (adjust `HOST`/`PORT` if needed) to compare throughput and behavior under load between the blocking and event-driven models.

---

## 📊 Benchmarks

The blocking server (project 1) and the `selectors` event-loop server (project 2) were run under identical load — same broadcast logic, same message size, same client counts — to see whether the theoretical concurrency argument actually holds up.

| Concurrent Clients | Msgs/Client | Total Messages | Blocking (thread-per-connection) | Event-Loop (`selectors`) |
|---|---|---|---|---|
| 100 | 200 | 20,000 | 398,113 msgs/sec | 513,491 msgs/sec |
| 500 | 200 | 100,000 | 465,076 msgs/sec | 557,659 msgs/sec |
| **2,000** | 200 | 400,000 | **218,818 msgs/sec** | **606,346 msgs/sec** |

**What's happening:** at low concurrency, both models perform comparably — the OS handles a few hundred threads without much strain. By 2,000 concurrent clients, the blocking server has spawned **~2,000 OS threads** (one per connection) and its throughput *drops* as the OS spends more time context-switching between them. The event-loop server handles the same 2,000 clients on a **single thread**, reacting to readiness events instead of blocking per-connection — and its throughput *increases*, because there's no thread-scheduling overhead eating into it.

This is a local-loopback (`127.0.0.1`) benchmark, so absolute numbers won't match a real network — but the *relative* behavior (threaded model degrading under connection count, event-loop model scaling past it) is exactly the trade-off that motivates event-driven servers like Nginx, Redis, and Node.js in production.

**To reproduce:** run `1-blocking-chat-server/server.py` and `1-blocking-chat-server/load_test.py` together, then repeat with `2-event-loop-server/event_server.py`, adjusting `NUM_CLIENTS` in the load test script. For higher concurrency (1,000+), an `asyncio`-based client (see `4-load-testing/async_load_testing.py`) is more reliable than a purely thread-based load generator, since spawning thousands of client-side threads adds its own overhead to the measurement.

---

## What this proves

The blocking server works fine at low concurrency, but every connected client costs a thread — that stops scaling well past a few thousand clients, as the benchmark above shows directly. The event-loop versions handle the same broadcast logic with a single thread by reacting to OS-level readiness events instead of blocking on each connection. Building the event loop by hand (`3-tiny-event-loop/`) — rather than only using Python's `selectors` — was the point: it turns "epoll-based servers are fast" from a fact into something demonstrably understood and measured.

**Previous:** [← Phase 1 — TCP/IP Core](../phase-1-tcp-ip/)
**Next:** Phase 3 — HTTP & TLS *(upcoming)*