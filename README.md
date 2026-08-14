<div align="center">

# From Packets to Product

**A first-principles systems networking journey** — from raw TCP handshakes to production-grade infrastructure, one measured phase at a time.

[![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=flat&logo=python&logoColor=white)](https://www.python.org/)
[![Status](https://img.shields.io/badge/Status-In%20Progress-orange)](ROADMAP.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/Contributions-Welcome-brightgreen.svg)](CONTRIBUTING.md)

[Roadmap](ROADMAP.md) · [Phases](#-phases) · [Benchmarks](#-proof-not-just-theory) · [Contributing](CONTRIBUTING.md)

</div>

---

## 🎯 Why this repo exists

Most engineers use TCP, HTTP, and load balancers without ever seeing what's actually happening underneath them. This repo closes that gap by **building the machinery itself** — raw sockets, hand-rolled event loops, custom HTTP servers, reverse proxies, and eventually a real product on top of all of it — then **measuring** whether the theory holds up.

It follows a structured 8-phase curriculum (full detail in [`ROADMAP.md`](ROADMAP.md)) moving through four layers:

```
Physics  →  Execution  →  Protocol  →  Systems
(bytes)     (programs)     (the web)    (scale & survival)
```

This repository is **actively growing** — each phase is built, benchmarked, documented, and committed before the next begins. Nothing here is theoretical; every performance claim below was produced by running the actual code in this repo.

---

## 📊 Proof, not just theory

Every "networking course" explains that event-driven servers outscale thread-per-connection servers. This repo **measures it**, using the servers in [`phase-2-sockets-event-driven/`](phase-2-sockets-event-driven/) under identical load:

| Concurrent Clients | Blocking (thread-per-connection) | Event-Loop (`selectors`) |
|---|---|---|
| 100 | 398,113 msgs/sec | 513,491 msgs/sec |
| 500 | 465,076 msgs/sec | 557,659 msgs/sec |
| **2,000** | **218,818 msgs/sec** ⬇️ | **606,346 msgs/sec** ⬆️ |

At 2,000 concurrent clients, the threaded server's throughput **collapses** — it's now juggling ~2,000 OS threads and losing time to context switching. The event-loop server, running the identical broadcast logic on a **single thread**, keeps scaling. This is the exact reason Nginx, Redis, and Node.js are built on event loops instead of thread-per-connection — and now it's demonstrated, not just cited.

Full methodology and how to reproduce these numbers yourself: [`phase-2-sockets-event-driven/README.md`](phase-2-sockets-event-driven/README.md#-benchmarks).

---

## 🗺 Phases

| Phase | Focus | Status | Key Projects |
|---|---|---|---|
| [01 · TCP/IP Core](phase-1-tcp-ip/) | Networking physics | ✅ Done | TCP client/server, packet capture & analysis, RTT measurement |
| [02 · Sockets & Event-Driven Systems](phase-2-sockets-event-driven/) | Concurrency models | ✅ Done | Threaded chat server, `selectors`-based event server, hand-rolled event loop, benchmarked load testing |
| 03 · HTTP & TLS | The protocol layer | ⏳ Upcoming | Custom HTTP server, TLS, keep-alive, HTTP load tester |
| 04 · Load, Failure & Reality | Systems under stress | ⏳ Upcoming | Reverse proxy, rate limiter, circuit breaker, failure simulation |
| 05 · Observability | Seeing inside systems | ⏳ Upcoming | Metrics, dashboards, distributed tracing |
| 06 · Security & Hardening | Production-grade safety | ⏳ Upcoming | Secured API gateway, TLS automation, attack simulation |
| 07 · Deployment & Cloud | Owning infrastructure | ⏳ Upcoming | Dockerized proxy, CI pipeline, failure drills |
| 08 · Productization | Turning skill into a product | ⏳ Upcoming | A real developer-facing product (spins into its own repo) |

Full detail, sub-goals, and tooling for every phase live in [`ROADMAP.md`](ROADMAP.md).

---

## 📂 How it's organized

```
from-packets-to-product/
├── README.md                        # you are here — the hub
├── ROADMAP.md                       # the full curriculum, with progress checkboxes
├── CONTRIBUTING.md                  # how to engage with this repo
├── LICENSE
├── phase-1-tcp-ip/
│   ├── README.md                    # what this phase covers, how to run it
│   ├── scripts/                     # TCP client/server implementations
│   └── captures/                    # .pcap files from Wireshark experiments
├── phase-2-sockets-event-driven/
│   ├── README.md                    # includes full benchmark results + methodology
│   ├── 1-blocking-chat-server/
│   ├── 2-event-loop-server/
│   ├── 3-tiny-event-loop/
│   └── 4-load-testing/
└── phase-3-... (upcoming)
```

**Convention:** each phase gets its own top-level folder (`phase-N-<name>`) with its own `README.md` explaining what's inside, why it exists, and how to run it. Sub-projects within a phase are numbered folders reflecting the order they were built in. This keeps the root README short even as the repo grows to 8 phases — it's an index, not a textbook.

---

## 🧰 Tools used across this repo

`tcpdump` · `wireshark` · `ss` · `netstat` · Python `socket` · Python `selectors` · `asyncio` · `strace` — with `curl`, `openssl`, `nginx`/`haproxy`, `wrk`, Docker, Prometheus, and Grafana entering in later phases.

---

## 🤝 Contributing

This is a personal learning journey, but it's built in the open on purpose. If you're learning networking too, see [`CONTRIBUTING.md`](CONTRIBUTING.md) for how to suggest corrections, propose better explanations, or fork this as a template for your own version of the curriculum.

---

## 📄 License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

---

<div align="center">

### Built by [Ahmad Hussain](https://github.com/AhmadHussainRandhawa) · <a href="https://www.linkedin.com/in/ahmad-hussain-randhawa/"><img src="https://icon.icepanel.io/Technology/svg/LinkedIn.svg" width="18" alt="LinkedIn" /></a>

⭐ **If you find this roadmap useful, consider giving the repository a star.**

</div>