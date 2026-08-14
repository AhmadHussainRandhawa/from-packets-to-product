# Phase 1 — Networking Physics (TCP/IP Core)

**Goal:** Understand how data physically moves between machines — no abstractions, packet-level truth.

This phase answers: *"What really happens when I send data over the internet?"*

---

## What's here

```
phase-1-tcp-ip/
├── scripts/
│   ├── tcp_server.py     # Minimal echo server — one connection, blocking I/O
│   └── tcp_client.py     # Client that sends 5,000 messages and measures RTT per round-trip
└── captures/
    ├── tcp_handshake.pcap    # The 3-way handshake, captured in Wireshark
    ├── tcp_flow.pcap         # Normal request/response flow
    ├── tcp_loss.pcap         # Behavior under simulated packet loss
    └── tcp_experiments.pcap  # General experimentation capture
```

## Concepts covered

- The TCP 3-way handshake (`SYN` → `SYN-ACK` → `ACK`), observed directly in packet captures rather than just described
- Sequence numbers and ACK behavior across a live connection
- Round-trip time (RTT) measurement at the application layer
- Retransmission and packet loss behavior under simulated network conditions
- Connection teardown and `TIME_WAIT` state

## How to run it

**1. Start the echo server:**
```bash
python scripts/tcp_server.py
```

**2. In a second terminal, run the client:**
```bash
python scripts/tcp_client.py
```
The client sends 5,000 short messages to the server and prints the measured RTT for each round trip — a direct, hands-on way to see latency instead of just reading about it.

**Measured results** (5,000 round trips, localhost loopback):

| Metric | Value |
|---|---|
| Average RTT | 0.0083 ms |
| Minimum RTT | 0.0073 ms |
| P95 RTT | 0.0108 ms |
| Maximum RTT | 0.2244 ms |

The tight spread between average and P95 is expected on loopback — there's no real network path, so the RTT is almost entirely syscall and scheduling overhead. The occasional max spike (0.22ms vs. an average of 0.008ms) is a useful, visible example of OS scheduling jitter — the same kind of tail latency that matters far more once this is run over a real network in later phases.

**3. Capture and inspect traffic (optional, requires Wireshark/tcpdump):**
```bash
sudo tcpdump -i lo -w my_capture.pcap port 5000
```
Then open the `.pcap` files in this folder with Wireshark to see the handshake, sequence numbers, and ACKs frame by frame. `tcp_loss.pcap` was captured while simulating loss with `tc` (Linux traffic control) to observe retransmission behavior directly.

## What this proves

Everything above the network layer — HTTP, REST, WebSockets, gRPC — sits on top of exactly this: a byte stream that has to be assembled, acknowledged, and recovered from loss. Having watched it happen at the packet level makes every higher-level abstraction in later phases legible rather than magical.

**Next:** [Phase 2 — Sockets & Event-Driven Systems →](../phase-2-sockets-event-driven/)