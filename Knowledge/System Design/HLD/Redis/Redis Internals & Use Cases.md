# Redis Internals & Use Cases

## How Redis Works Internally
- Single-threaded event loop (fast operations)
- Uses in-memory data structures
- Optional persistence:
  - RDB (snapshot based)
  - AOF (append-only file)

## Data Structures
- String
- List
- Set
- Hash
- Sorted Set (ZSet)

## Scaling Redis
- Replication (Master-Slave)
- Redis Sentinel (failover)
- Redis Cluster (sharding)

## Common Design Patterns

### 1. Caching Pattern
- Store frequently accessed data
- TTL (time-to-live) for expiry

### 2. Rate Limiting
- Use counters or sliding window

### 3. Leaderboard System
- Use Sorted Sets (ZSet)

### 4. Distributed Lock
- Use SETNX or Redlock algorithm

## When to Use Redis
- Need ultra-fast reads/writes
- Real-time analytics
- High concurrency systems

## When NOT to Use Redis
- Large persistent datasets
- Complex relational queries
