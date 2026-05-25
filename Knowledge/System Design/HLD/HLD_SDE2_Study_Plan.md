---
title: SDE-2 High-Level Design (HLD) Master Plan
tags:
  - interview-prep
  - HLD
  - SDE2
  - system-design
  - architecture
---

# HLD Study, Architecture & Practice Plan

This comprehensive study plan is curated for mastering High-Level Design (HLD) and Distributed Systems architecture for SDE-2 roles. It transitions from core foundational blocks to specific distributed primitives, a curated list of 22 practice problems, and essential engineering tech blogs to stay updated with production-grade engineering.

---

## 📚 Phase 1: Core System Design Pillars

### ⚖️ 1. Scalability & Performance Metrics
- [ ] **Horizontal vs Vertical Scaling:** Stateless vs stateful scaling, scaling limits, and cost trade-offs.
- [ ] **Latency, Throughput, & Bandwidth:** Understanding constraints, calculation formulas (QPS, storage, bandwidth), and Little's Law.
- [ ] **Availability & Reliability:** High Availability (HA) design patterns, Active-Passive vs Active-Active configurations, calculating Nines of availability (e.g., 99.99%).

### 🌐 2. Networking Protocols & Content Delivery
- [ ] **Transport & Application Layers:** HTTP/1.1 vs HTTP/2 vs HTTP/3, WebSockets for long-lived duplex connections, gRPC/Protobuf vs REST/JSON for internal RPCs.
- [ ] **Load Balancers:** Layer 4 (TCP) vs Layer 7 (Application) routing, algorithms (Round Robin, Consistent Hashing, Least Connections), and hardware vs software LBs (Nginx, HAProxy, AWS ALB).
- [ ] **CDNs (Content Delivery Networks):** Edge servers, push vs pull models, cache invalidation strategies, and static vs dynamic content acceleration.

### 🗄️ 3. Data Storage Layer (Crucial for SDE-2)
- [ ] **Relational Databases (RDBMS):** ACID properties, indexing mechanisms (B-Trees, B+ Trees), execution plans, replication topologies (Single-Leader, Multi-Leader, Leaderless).
- [ ] **NoSQL Databases:** Key-Value (Redis, Dynamo), Document (MongoDB), Column-Family (Cassandra, ScyllaDB), Graph (Neo4j). Understand LSM-Trees vs B-Trees for write-heavy workloads.
- [ ] **Scaling Databases:** Database sharding (horizontal partitioning), partition keys, hot spotting, routing layers, and cross-shard joins/transactions.
- [ ] **Distributed Theorems:** CAP Theorem (Consistency, Availability, Partition Tolerance) and the PACELC theorem extension.

### ⚡ 4. Caching Topologies
- [ ] **Caching Strategies:** Cache-Aside, Write-Through, Write-Around, Write-Back.
- [ ] **Cache Eviction Policies:** LRU, LFU, FIFO, TTL-based eviction.
- [ ] **Distributed Caching:** Redis Clusters, Memcached, cache invalidation at scale, handling **Cache Stampede**, **Cache Penetration**, and **Cache Avalanche**.

### 📨 5. Asynchronous Messaging & Event-Driven Architecture
- [ ] **Message Queues vs Event Streams:** Point-to-point queues (RabbitMQ) vs Distributed commit logs (Apache Kafka, Pulsar).
- [ ] **Key Concepts:** Consumer groups, partitions, offsets, message delivery guarantees (At-least-once, At-most-once, Exactly-once processing via idempotency).
- [ ] **Patterns:** Event Sourcing, CQRS (Command Query Responsibility Segregation), and Saga Pattern (Orchestration vs Choreography for distributed transactions).

---

## ⚙️ Phase 2: Distributed Systems Primitives

- [ ] **Consistent Hashing:** Ring topology, virtual nodes, rehashing mitigation during node failures (essential for distributed caching and storage routing).
- [ ] **Distributed ID Generation:** Ticket servers, UUIDs, and Twitter Snowflake algorithm (time-ordered, scalable, collision-free).
- [ ] **Distributed Consensus:** Basics of Paxos and Raft protocols; how distributed coordination services like Apache ZooKeeper or etcd handle leader election and configuration management.
- [ ] **Distributed Locking:** Multi-node synchronization using Redis (Redlock algorithm), ZooKeeper recipes, or database-backed locks.

---

## 💻 Phase 3: 22 Practice Problems (Categorized)

### 🧱 Tier 1: Core Utilities & Microservices (Warm-up)
Focus on API definitions, data models, and simple distributed building blocks.
- [ ] **1. Design a URL Shortener (TinyURL / Bitly):** Focus on hash collisions, base62 encoding, distributed ID generation, and read caching.
- [ ] **2. Design a Distributed Unique ID Generator:** Focus on Snowflake algorithm, timestamp synchronization, and worker node coordination.
- [ ] **3. Design a Distributed Rate Limiter:** Focus on Token Bucket/Leaky Bucket algorithms, Redis cluster integration, and synchronization overhead.
- [ ] **4. Design a Notification System:** Focus on multi-channel routing (SMS, Email, Push), prioritizing queues, retry mechanisms, and idempotency keys to avoid duplicate notifications.
- [ ] **5. Design a Distributed Key-Value Store:** Focus on consistent hashing, replication factor, tunable consistency (Quorum writes/reads), and gossip protocols.

### 🚀 Tier 2: High-Scale Consumer Platforms (Standard SDE-2)
Focus on read-heavy vs write-heavy optimizations, push vs pull architectures, and data isolation.
- [ ] **6. Design Twitter / X (News Feed Service):** Focus on Fan-out-on-write vs Fan-out-on-read, handling celebrity profiles, hybrid timeline generation, and Redis-backed home feeds.
- [ ] **7. Design WhatsApp / Slack (Chat System):** Focus on persistent WebSocket connections, connection management clusters, gateway layers, message status tracking (sent, delivered, read), and group chats.
- [ ] **8. Design Netflix / YouTube (Video Streaming Platform):** Focus on video transcoding pipelines, object storage (S3), content delivery networks (CDNs), adaptive bitrate streaming (HLS/DASH), and metadata storage.
- [ ] **9. Design Uber / Lyft (Ride-Sharing Service):** Focus on geospatial indexing (Geohash, Google S2 geometry, Quadtrees), real-time driver tracking, matching algorithms, and dynamic surge pricing engines.
- [ ] **10. Design Ticketmaster / BookMyShow:** Focus on flash sales, handling extreme concurrency, distributed locking for seat reservations, transactional boundaries, and database connection pooling.
- [ ] **11. Design Airbnb / Hotel Booking System:** Focus on concurrency control (preventing double booking), inventory management, search filtering based on availability, and handling temporal data.
- [ ] **12. Design Amazon / E-Commerce System:** Focus on microservice boundaries, shopping cart management, payment workflows, checkout orchestrators, inventory deduction, and eventual consistency.
- [ ] **13. Design Instagram / Pinterest (Image Sharing Platform):** Focus on object storage architecture, image optimization/compression pipelines, graph databases for follower graphs, and async news feed processing.
- [ ] **14. Design Spotify / Apple Music (Audio Streaming):** Focus on audio chunking, metadata management, audio CDNs, offline sync capabilities, and playlists/activity streams.

### 🛰️ Tier 3: Advanced Infrastructure & Complex Orchestrations
Focus on stream processing, massive write-heavy systems, and specialized indexing.
- [ ] **15. Design Google Drive / Dropbox:** Focus on file chunking, block-level delta synchronization, metadata database syncing, and conflict resolution architectures.
- [ ] **16. Design Yelp / Google Maps Nearby Places:** Focus on Geohash precision, read-intensive scaling, updating business locations asynchronously, and spatial data querying.
- [ ] **17. Design Google Maps (Routing/Navigation System):** Focus on graph representations of road networks, routing algorithms (Dijkstra/A* variations at scale), and dynamic ETA calculations using real-time traffic segments.
- [ ] **18. Design a Distributed Web Crawler:** Focus on URL frontier management, politeness policies, HTML parsing, deduplication via MurmurHash/Bloom Filters, and distributed storage of crawled data.
- [ ] **19. Design a Metrics Monitoring & Alerting System (e.g., Datadog / Prometheus):** Focus on Time-Series Databases (TSDB), push vs pull metrics collection, data retention policies, aggregation pipelines, and anomaly detection.
- [ ] **20. Design an Ad Click Aggregator:** Focus on high-throughput event streaming, real-time map-reduce aggregation (using Flink/Spark streaming), deduplicating fraud clicks, and cold/warm storage separation.
- [ ] **21. Design a Distributed Task Scheduler / Cron Framework:** Focus on delayed queues, priority queues, worker nodes auto-scaling, misfire handling policies, and state persistence.
- [ ] **22. Design a Payment Gateway Orchestrator:** Focus on idempotency guarantees, state machines for transaction lifecycles, dual-entry ledger design, handling webhook timeouts, and circuit-breaker patterns for 3rd-party bank APIs.

---

## 📑 Phase 4: Estimation & Back-of-the-Envelope Calculations
Mastering estimations is critical for SDE-2 interviews to justify technical choices (e.g., choosing Redis vs RAM, calculating database cluster sizes).

- **Data Size Reference Rules:**
  * Char = 1 byte | Int = 4 bytes | Long/Double = 8 bytes.
  * IPv4 address = 4 bytes | Unique UUID = 36 bytes.
- **Capacity Conversions:**
  * 1 Million daily active users (DAU) $\times$ 100 actions/day $\approx$ 1160 QPS.
  * 100 Million requests per day $\approx$ 1160 QPS (approx 1200 QPS).
  * Storage: 1 QPS of 1KB writes $\approx$ 86.4 MB/day $\approx$ 31.5 GB/year.

---

## 🌐 Phase 5: Production-Grade Tech Blogs to Read

To build deep intuition for real-world distributed architectures, replace generic resources with engineering blogs authored by platforms handling real-world scale:

### 🚀 Tier-1 System Engineering Blogs
1. **[The Netflix Tech Blog](https://netflixtechblog.com/)**
   * *Why:* Pioneers of microservices, chaos engineering, resilience (Hystrix/Circuit breakers), and hybrid cloud databases.
   * *Must-Reads:* Architecture of video encoding, Eureka/Service discovery, and caching with EVCache.
2. **[The Uber Engineering Blog](https://www.uber.com/blog/engineering/)**
   * *Why:* Unrivaled resource for real-time geospatial processing, ride matching, and handling stateful microservices.
   * *Must-Reads:* Evolution of Uber's geospatial index (from Geohash to H3/S2), and how they built their distributed database (Schemaless).
3. **[The Discord Engineering Blog](https://discord.com/blog/categories/engineering)**
   * *Why:* Incredible deep-dives on concurrency, performance tuning, and database migrations.
   * *Must-Reads:* "How Discord Stores Billions of Messages" (Migrating from MongoDB to Cassandra, then to ScyllaDB), and how they optimize WebSockets.
4. **[The Slack Engineering Blog](https://slack.engineering/)**
   * *Why:* Great for real-time state synchronization, message delivery state machines, and scaling caching systems.
   * *Must-Reads:* Architecture of Slack’s real-time messaging fabric, and how they implement local caching strategies.
5. **[The Stripe Engineering Blog](https://stripe.com/blog/engineering)**
   * *Why:* The gold standard for financial reliability, consistency patterns, API design, and **idempotency**.
   * *Must-Reads:* Designing distributed systems for zero double-charges, and how Stripe handles transaction ledgers with total consistency.

### 📚 Curated Deep-Dive Resources
* **[Designing Data-Intensive Applications (DDIA) by Martin Kleppmann](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/)**: The ultimate book for understanding distributed primitives (Replication, Sharding, Consensus).
* **[The System Design Primer (GitHub Repository)](https://github.com/donnemartin/system-design-primer)**: Exceptional cheat sheets and comprehensive collection of visual architectures.
* **[ByteByteGo (Alex Xu Newsletter)](https://blog.bytebytego.com/)**: Excellent visual Breakdowns of modern real-world production architectures.

---

## 📋 The SDE-2 HLD Interview Blueprint
Use this mental framework to drive the 45-minute HLD discussion:

1. **Scope the System (5-7 mins):**
   * Clarify functional requirements (e.g., "Do we need real-time comments, or is eventual consistency okay?").
   * Define non-functional requirements (Availability vs Consistency, Scale: DAU, Read-Write ratio).
   * Do quick back-of-the-envelope calculations for QPS and Storage.
2. **High-Level Architecture (10 mins):**
   * Draw the end-to-end data flow block diagram (Client $\rightarrow$ DNS/CDN $\rightarrow$ API Gateway $\rightarrow$ App Servers $\rightarrow$ Databases/Queues).
   * Define the core APIs (e.g., `POST /v1/trips/book`).
3. **Deep Dive Component Design (20 mins):**
   * This is where you shine. Zoom into the hardest problem (e.g., how the newsfeed engine merges timeline arrays, or how the geo-index fetches drivers within 2km via Geohash/Redis).
   * Discuss database schemas, partitioning strategies, caching layers, and race conditions/concurrency blockers.
4. **Resilience & Bottlenecks (5 mins):**
   * Introduce Single Point of Failure (SPOF) remediations, rate limiting, circuit breakers, and data replication catch-ups.
