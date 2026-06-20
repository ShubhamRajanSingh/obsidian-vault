---
title: HLD / System Design Study Map (SDE-2)
tags: [system-design, hld, interview-prep]
created: 2026-06-16
status: in-progress
---

# HLD / System Design — Complete Study Map (SDE-2)

> Goal: enough coverage to design almost any real-world system from scratch and reason confidently about tradeoffs in interviews.

---

## 📌 Dependency Graph (learn in this order)

```
Networking + Storage Engines (base layer)
        ↓
Replication & Consistency ──→ Consensus
        ↓
Partitioning/Sharding ──→ Caching ──→ Queues
        ↓                              ↓
Scaling Patterns              Idempotency/Dedup
        ↓                              ↓
Distributed Transactions ←────────────┘
        ↓
   [Now ready for domain systems]
        ↓
CDN / Geo / Search / Chat / Payments / Recommendations (parallel — pick by target company)
        ↓
Cross-cutting concerns (security, reliability, observability) — layer onto every design
        ↓
Multi-region + Infra/platform topics (senior differentiation)
```

---

## 🎯 High-ROI Topics for Interviews
- [ ] Caching strategies (cache-aside, write-through, stampede prevention)
- [ ] Consistency tradeoffs (CAP / PACELC)
- [ ] Sharding / partitioning strategies
- [ ] Queues + idempotency together
- [ ] Rate limiting
- [ ] CDN fundamentals
- [ ] At least 2 deep domain verticals matched to target company (geo for Uber/Lyft, video for streaming cos, payments for fintech)

## ⚠️ Commonly Missed (but expected at SDE-2+)
- Idempotency under client retries
- Failure-mode design — not just the happy path
- Analytics/observability designed in, not bolted on after
- Data deletion / compliance impact on architecture (GDPR-style erasure)
- Clock/ordering issues in distributed writes
- Backpressure when a downstream dependency is slow

## 🧠 Edge Topics (separate strong candidates from average)
- WebRTC SFU/TURN/ICE mechanics
- CRDTs for multi-region conflict resolution
- H3 / geohash tradeoffs for geospatial indexing
- Content-addressable storage & dedup (Dropbox-style)
- Saga pattern vs 2PC tradeoffs
- Vector clocks for causality
- Approximate counting algorithms (HyperLogLog, Count-Min Sketch) at scale

---

## 1. Foundational Topics

### Networking Fundamentals
- **Why it matters:** Every design conversation touches latency/protocol choice at some point.
- **Core concepts:** TCP vs UDP, HTTP/1.1 vs HTTP/2 vs HTTP/3 (QUIC), DNS resolution, TLS handshake, connection pooling, keep-alive, WebSockets vs long polling vs SSE.
- **Interview questions:** Why pick UDP for video calls but TCP for payments? How does HTTP/2 multiplexing help?
- **Prerequisites:** None.
- **Depth:** Intermediate.
- **Used in:** Every system.

### Storage Engine Internals
- **Why it matters:** "Which database" is meaningless without knowing how they store data.
- **Core concepts:** B-trees vs LSM-trees, WAL (write-ahead log), page cache, row vs column storage, indexing strategies, compaction.
- **Interview questions:** Why is Cassandra fast for writes? How does an index slow down writes?
- **Prerequisites:** None.
- **Depth:** Deep.
- **Used in:** Postgres, RocksDB, Cassandra, HBase.

### Replication & Consistency Models
- **Why it matters:** Core of every tradeoff discussion.
- **Core concepts:** Leader-follower, multi-leader, leaderless (quorum), sync vs async replication, CAP, PACELC, linearizability, causal consistency, eventual consistency, read-your-writes.
- **Interview questions:** Design a system that's AP but still useful. How do you handle replica lag?
- **Prerequisites:** Networking basics.
- **Depth:** Deep.
- **Used in:** Every distributed datastore.

### Partitioning / Sharding
- **Why it matters:** Required to scale beyond one machine.
- **Core concepts:** Range vs hash partitioning, consistent hashing, hot partition mitigation, resharding strategies, virtual nodes.
- **Interview questions:** Design sharding for a multi-tenant SaaS. How do you reshard without downtime?
- **Prerequisites:** Replication.
- **Depth:** Deep.

### Consensus Algorithms
- **Why it matters:** Underlies leader election, distributed locks, config stores.
- **Core concepts:** Paxos (conceptual), Raft, quorum reads/writes, split-brain.
- **Interview questions:** How does etcd/Zookeeper achieve consensus? Why an odd number of nodes?
- **Prerequisites:** Replication.
- **Depth:** Intermediate (know the what/why, not formal proofs).

### Caching
- **Why it matters:** Highest ROI lever for latency and cost.
- **Core concepts:** Cache-aside, write-through, write-back, TTL/eviction (LRU/LFU), cache stampede, thundering herd, negative caching, multi-tier caching (CDN → app → DB).
- **Interview questions:** Design cache invalidation for a feed. How do you prevent cache stampede?
- **Prerequisites:** None.
- **Depth:** Deep.

### Message Queues & Event Streaming
- **Why it matters:** Decoupling, async processing, backpressure.
- **Core concepts:** Pub-sub vs point-to-point, at-most/at-least/exactly-once delivery, idempotency, dead-letter queues, consumer groups, partitioned topics, backpressure.
- **Interview questions:** How do you guarantee exactly-once processing? Design a retry strategy.
- **Prerequisites:** Networking.
- **Depth:** Deep.

### Scaling Patterns
- **Why it matters:** Literal core of HLD.
- **Core concepts:** Vertical vs horizontal scaling, stateless services, load balancing algorithms (round robin, least connections, consistent hashing), connection draining, autoscaling triggers.
- **Prerequisites:** Networking, partitioning.
- **Depth:** Intermediate.

### Idempotency & Deduplication
- **Why it matters:** Chronically under-discussed; appears in every payments/messaging design. **Commonly missed.**
- **Core concepts:** Idempotency keys, dedup windows, exactly-once illusion via idempotent writes.
- **Interview questions:** Client retries a payment request — what happens?
- **Prerequisites:** Queues.
- **Depth:** Intermediate.

### Distributed Transactions
- **Why it matters:** Needed when a single DB can't hold all state.
- **Core concepts:** 2PC, Saga pattern (orchestration vs choreography), compensating transactions, outbox pattern.
- **Interview questions:** Design order placement across inventory + payment services.
- **Prerequisites:** Consensus, queues.
- **Depth:** Deep for senior roles.

### Rate Limiting & Backpressure
- **Core concepts:** Token bucket, leaky bucket, sliding window, distributed rate limiting (Redis-based).
- **Interview questions:** Design a rate limiter for an API gateway across multiple nodes.
- **Prerequisites:** Caching.
- **Depth:** Intermediate.

### Time, Ordering, Clocks
- **Why it matters:** Edge topic that separates strong candidates. **Commonly missed.**
- **Core concepts:** Logical clocks (Lamport, vector clocks), clock skew, NTP, monotonic vs wall clock, snowflake-style ID generation.
- **Interview questions:** Generate globally unique IDs without coordination.
- **Prerequisites:** Consensus.
- **Depth:** Intermediate.

---

## 2. System-Level Domains

### Video / Media Streaming
- **Why it matters:** Huge interview category (YouTube/Netflix-style questions).
- **Core concepts:** Transcoding pipelines, adaptive bitrate (HLS/DASH), chunking, manifest files, CDN edge delivery, thumbnail generation, DRM basics.
- **Interview questions:** Design YouTube. How does adaptive bitrate switching work?
- **Prerequisites:** CDN, storage, queues (for async transcoding).
- **Depth:** Deep.
- **Real systems:** YouTube, Netflix, Twitch VOD.

### Large File Upload / Download
- **Core concepts:** Chunked/multipart upload, resumable uploads, checksum verification, presigned URLs, client-side chunk hashing for resume.
- **Interview questions:** Design Dropbox upload for 10GB files over a flaky network.
- **Prerequisites:** Storage internals, object storage.
- **Depth:** Intermediate.
- **Real systems:** Dropbox, Google Drive, S3 multipart upload.

### Image / Media Processing & Delivery
- **Core concepts:** On-the-fly vs pre-computed resizing, image pyramids, format conversion (WebP/AVIF), perceptual hashing for dedup, lazy transformation + CDN caching.
- **Interview questions:** Design Instagram's image pipeline. How do you serve 5 sizes of the same image efficiently?
- **Prerequisites:** CDN, caching, storage.
- **Depth:** Intermediate.
- **Real systems:** Instagram, Imgix, Cloudinary.

### Geospatial Systems
- **Why it matters:** High ROI; a distinct skill most candidates lack.
- **Core concepts:** Geohashing, quad trees, R-trees, H3 hexagonal grids, proximity search, map tile pyramids (z/x/y tiles), routing graphs (Dijkstra/A* at scale), ETA prediction.
- **Interview questions:** Design Uber driver matching. Design "restaurants near me."
- **Prerequisites:** Partitioning, caching.
- **Depth:** Deep.
- **Real systems:** Uber, Google Maps, DoorDash.

### Chat / Messaging Systems
- **Core concepts:** WebSocket connection management at scale, presence systems, message ordering, fan-out on write vs read, offline message queuing, read receipts, E2E encryption basics (key exchange).
- **Interview questions:** Design WhatsApp. How do you handle a user with 1000 group chats?
- **Prerequisites:** Queues, consistency models, WebSockets.
- **Depth:** Deep.
- **Real systems:** WhatsApp, Slack, Discord.

### Real-Time / Live Streaming & WebRTC
- **Core concepts:** SFU vs MCU vs P2P mesh, ICE/STUN/TURN/NAT traversal, RTP/RTCP, jitter buffers, simulcast, low-latency HLS.
- **Interview questions:** Design a video calling app for 1000 participants. Why can't WebRTC scale P2P beyond ~6 people?
- **Prerequisites:** Networking, video streaming basics.
- **Depth:** Deep — interviewers love probing this; few candidates know it.
- **Real systems:** Zoom, Google Meet, Twitch live.

### Search & Autocomplete
- **Core concepts:** Inverted indexes, tokenization/stemming, ranking (TF-IDF, BM25), trie-based prefix matching, fuzzy matching, query-time vs index-time tradeoffs, typeahead caching.
- **Interview questions:** Design Google autocomplete. Design Elasticsearch-like full text search.
- **Prerequisites:** Caching, storage internals.
- **Depth:** Deep.
- **Real systems:** Elasticsearch, Google Search, Algolia.

### Recommendation Systems
- **Core concepts:** Collaborative filtering vs content-based, embedding-based retrieval, two-stage candidate generation + ranking, feature stores, batch vs real-time inference pipelines, cold-start problem.
- **Interview questions:** Design YouTube recommendations. Design news feed ranking.
- **Prerequisites:** Queues, analytics pipelines.
- **Depth:** Intermediate-Deep.
- **Real systems:** YouTube, TikTok, Netflix.

### Payments
- **Core concepts:** Idempotency keys, double-entry ledger design, two-phase settlement (auth/capture), reconciliation, webhook retries, PCI scope minimization (tokenization), saga for distributed payment flows.
- **Interview questions:** Design a payment gateway. How do you guarantee no double-charging?
- **Prerequisites:** Idempotency, distributed transactions.
- **Depth:** Deep.
- **Real systems:** Stripe, PayPal, UPI.

### Notifications & Email
- **Core concepts:** Fan-out architecture, multi-channel delivery (push/SMS/email), provider abstraction + failover, batching/digesting, delivery guarantees, unsubscribe/preference management, bounce handling.
- **Interview questions:** Design a notification system for 1B users.
- **Prerequisites:** Queues, rate limiting.
- **Depth:** Intermediate.
- **Real systems:** FCM/APNs, SendGrid.

### Analytics / Metrics Pipelines
- **Why it matters:** **Commonly missed entirely** by candidates.
- **Core concepts:** Batch (Hadoop/Spark) vs stream (Flink/Kafka Streams) processing, lambda/kappa architecture, approximate algorithms (HyperLogLog, Count-Min Sketch), star schema/OLAP cubes.
- **Interview questions:** Design a real-time analytics dashboard counting unique visitors.
- **Prerequisites:** Queues, storage internals.
- **Depth:** Intermediate.

### Feed / Timeline Generation
- **Core concepts:** Push (fan-out on write) vs pull (fan-out on read) vs hybrid, celebrity problem, ranking injection.
- **Interview questions:** Design Twitter timeline.
- **Prerequisites:** Queues, caching.
- **Depth:** Deep — distinct enough from "recommendation" to study separately.
- **Real systems:** Twitter/X, Facebook feed.

### E-commerce / Inventory Systems
- **Core concepts:** Inventory consistency under high contention, reservation patterns, overselling prevention, cart vs order state machines.
- **Interview questions:** Design ticket/seat booking avoiding double booking.
- **Prerequisites:** Distributed transactions, consistency.
- **Depth:** Intermediate.

---

## 3. Cross-Cutting Concerns

### Security
- **Core concepts:** AuthN (OAuth2/OIDC, JWT) vs authZ (RBAC/ABAC), API key management, encryption at rest/in transit, secrets management, DDoS mitigation at edge.
- **Interview questions:** Design auth for a multi-tenant API.
- **Depth:** Intermediate.

### Reliability / Resilience
- **Why it matters:** Interviewers actively probe failure modes. **Commonly missed: candidates design only the happy path.**
- **Core concepts:** Circuit breakers, retries with exponential backoff + jitter, bulkheads, graceful degradation, chaos engineering mindset, blast radius limiting.
- **Interview questions:** Design for partial failure when a downstream dependency is slow.
- **Depth:** Deep.

### Observability
- **Core concepts:** Metrics/logs/traces distinction, distributed tracing (trace context propagation), SLI/SLO/SLA, alerting design.
- **Interview questions:** How would you debug a latency spike across 20 microservices?
- **Depth:** Intermediate.

### Compliance / Data Governance
- **Why it matters:** **Commonly missed but increasingly asked.**
- **Core concepts:** Data residency, GDPR right-to-erasure implications on architecture, audit logging, PII handling/encryption, retention policies.
- **Interview questions:** Design a system that must delete user data within 30 days across all replicas/backups.
- **Depth:** Beginner-Intermediate.

### Deployment & Release
- **Core concepts:** Blue-green, canary, feature flags, schema migration strategies (backward compatibility), zero-downtime deploys.
- **Depth:** Intermediate.

---

## 4. Infrastructure / Platform Topics

### Containers & Orchestration
- **Core concepts:** Container isolation basics, Kubernetes pods/deployments/services, horizontal pod autoscaling, liveness/readiness probes.
- **Depth:** Intermediate (concepts, not kubectl syntax).

### Service Discovery & API Gateways
- **Core concepts:** Client-side vs server-side discovery, service mesh (sidecar pattern), gateway responsibilities (auth, rate limiting, routing).
- **Depth:** Intermediate.

### Multi-Region Architecture
- **Why it matters:** Edge topic; strong differentiator for senior roles.
- **Core concepts:** Active-active vs active-passive, data residency-driven sharding, cross-region replication lag, conflict resolution (CRDTs, last-write-wins), DNS-based failover.
- **Interview questions:** Design a system serving both EU and US with data residency constraints.
- **Depth:** Deep for senior roles.

### Object / Blob Storage Internals
- **Core concepts:** Durability via erasure coding/replication factor, metadata vs data separation, multipart upload mechanics.
- **Depth:** Intermediate.

---

## 5. "How X Works" Case Studies

| System | Key teaching points |
|---|---|
| YouTube / Netflix | Transcoding pipeline, ABR streaming, CDN strategy, recommendations |
| Uber | Geospatial indexing, real-time matching, surge pricing, ETA |
| WhatsApp | WebSocket scale, E2E encryption, offline delivery, fan-out |
| Google Maps | Tile pyramids, routing graphs, geocoding |
| Instagram | Image pipeline, feed fan-out, graph-based social data |
| Dropbox | Chunking, dedup (content-addressable storage), sync conflict resolution |
| Kafka | Log-structured storage, partition replication, consumer offset management |
| Redis | Single-threaded event loop, persistence (RDB/AOF), data structure internals |
| Postgres | MVCC, WAL, vacuum, index types |
| Kubernetes | Control loop reconciliation, scheduler, etcd as source of truth |

---

## 📂 Suggested Vault Structure
```
HLD/
├── 00-Study-Map.md  (this file)
├── Foundational/
│   ├── Networking.md
│   ├── Storage-Engines.md
│   ├── Replication-Consistency.md
│   ├── Partitioning-Sharding.md
│   ├── Consensus.md
│   ├── Caching.md
│   ├── Queues-Streaming.md
│   ├── Scaling-Patterns.md
│   ├── Idempotency.md
│   ├── Distributed-Transactions.md
│   ├── Rate-Limiting.md
│   └── Clocks-Ordering.md
├── Domains/
│   ├── Video-Streaming.md
│   ├── File-Upload-Download.md
│   ├── Image-Processing.md
│   ├── Geospatial.md
│   ├── Chat-Messaging.md
│   ├── WebRTC-Live-Streaming.md
│   ├── Search-Autocomplete.md
│   ├── Recommendations.md
│   ├── Payments.md
│   ├── Notifications.md
│   ├── Analytics-Pipelines.md
│   ├── Feed-Timeline.md
│   └── Ecommerce-Inventory.md
├── Cross-Cutting/
│   ├── Security.md
│   ├── Reliability.md
│   ├── Observability.md
│   ├── Compliance.md
│   └── Deployment.md
├── Infrastructure/
│   ├── Containers-Orchestration.md
│   ├── Service-Discovery-Gateways.md
│   ├── Multi-Region.md
│   └── Object-Storage.md
└── Case-Studies/
    ├── YouTube.md
    ├── Uber.md
    ├── WhatsApp.md
    ├── Google-Maps.md
    ├── Instagram.md
    ├── Dropbox.md
    ├── Kafka.md
    ├── Redis.md
    ├── Postgres.md
    └── Kubernetes.md
```
