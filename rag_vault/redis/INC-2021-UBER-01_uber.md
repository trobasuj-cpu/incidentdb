# [INC-2021-UBER-01] Microservice Cascade Collapse via Redis Epoll Starvation and Unbounded Connection Spikes
**Company:** Uber | **Date:** 2021-03-18 | **Severity:** HIGH  
**Technologies:** Redis, Go, Docker, Linux  
**Categories:** CONNECTION_EXHAUSTION, CASCADE_FAILURE, CACHE_SUBSYSTEM  

---

## 1. Symptoms & Observed Errors
```text
dial tcp 10.0.12.44:6379: i/o timeout
redis: connection pool timeout: timed out waiting for free connection from pool
ERR max number of clients reached (10000 clients active)
HTTP/1.1 500 Internal Server Error: Failed to fetch driver geolocation cache
Kernel: [29104.12] nf_conntrack: table full, dropping packet
```

## 2. Root Cause Analysis
A brief 200ms latency spike in the backend database caused downstream Go microservices to spawn additional goroutines. Each goroutine opened a new connection to Redis rather than reusing a bounded connection pool. Within 15 seconds, Redis reached its maxclients threshold (10,000), causing connection drops. The client services retried aggressively without exponential backoff or jitter, creating an unrecoverable thundering herd.

## 3. Breaking Configuration / Problematic Code
```
// Vulnerable client setup: Defaulting to unbounded pool and aggressive retries
var rdb = redis.NewClient(&redis.Options{
    Addr:         "redis-cluster.internal:6379",
    PoolSize:     0,  // Unbounded: creates connections on demand
    MinIdleConns: 50,
    MaxRetries:   10, // Aggressive tight retry loop
})
```

## 4. Remediation Patch / Corrected Configuration
```
// Hardened client configuration with strict connection pooling and jitter backoff
var rdb = redis.NewClient(&redis.Options{
    Addr:         "redis-cluster.internal:6379",
    PoolSize:     200, // Hard ceiling matching backend capacity
    MinIdleConns: 20,
    MaxRetries:   3,
    MinRetryBackoff: 50 * time.Millisecond,
    MaxRetryBackoff: 500 * time.Millisecond,
    DialTimeout:     1 * time.Second,
})
```

## 5. Prevention & Hardening Checklist
- [ ] Enforce strict client-side connection pooling caps with circuit breaker fallbacks on cache exhaustion
- [ ] Mandate full jitter exponential backoff on all network retries
- [ ] Configure kernel nf_conntrack_max and Redis maxclients headroom alerting at 70% threshold
