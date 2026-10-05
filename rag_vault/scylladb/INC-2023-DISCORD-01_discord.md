# [INC-2023-DISCORD-01] Message History Latency Surges via ScyllaDB Hot-Partition Compaction Thread Contention
**Company:** Discord | **Date:** 2023-01-20 | **Severity:** HIGH  
**Technologies:** ScyllaDB, Rust, C++, NVMe  
**Categories:** STORAGE_SUBSYSTEM, DATABASE, HOT_PARTITION  

---

## 1. Symptoms & Observed Errors
```text
scylla: [shard 14] seastar - reactor stalled for 124ms
ERROR: Query on table 'messages_by_channel' timed out after 5000ms
Client p99 latency increased from 4.2ms to 2,800ms
Discord client UI: 'Failed to load messages. Try again later.'
```

## 2. Root Cause Analysis
A popular public Discord guild experienced a massive bot event resulting in over 100,000 messages in a single channel. In ScyllaDB's Seastar shared-nothing engine, a single large partition is assigned to a single CPU core/shard. Continuous major compaction on this single hot partition locked the shard's reactor thread, stalling query execution for other unrelated channels mapped to the same CPU core.

## 3. Breaking Configuration / Problematic Code
```
# Table schema with unbounded partition size:
CREATE TABLE messages_by_channel (
    channel_id bigint,
    message_id bigint,
    author_id bigint,
    content text,
    PRIMARY KEY (channel_id, message_id)
) WITH CLUSTERING ORDER BY (message_id DESC);
# Anti-pattern: channel_id partition key grows infinitely over time
```

## 4. Remediation Patch / Corrected Configuration
```
# Bucketed partition key bounding maximum partition size to 10 days of messages:
CREATE TABLE messages_by_channel_bucketed (
    channel_id bigint,
    bucket int, // Derived as unix_timestamp / 864000 (10-day epoch)
    message_id bigint,
    author_id bigint,
    content text,
    PRIMARY KEY ((channel_id, bucket), message_id)
) WITH CLUSTERING ORDER BY (message_id DESC);
```

## 5. Prevention & Hardening Checklist
- [ ] Design Cassandra/ScyllaDB schemas with compound partition keys (bucketing) to prevent unbounded partition growth
- [ ] Alert on partitions exceeding 100MB in size or 100,000 clustering rows
- [ ] Configure client-side rate limiting on third-party bot message dispatching
