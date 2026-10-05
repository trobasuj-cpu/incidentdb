# [INC-2023-REDIS-01] Real-time Redis Stall via Background AOF fsync Blocking Main Event Thread
**Company:** Cloud Storage Global | **Date:** 2023-08-11 | **Severity:** HIGH  
**Technologies:** Redis, Linux, AWS EBS, C  
**Categories:** CACHE_SUBSYSTEM, FSYNC_STALL, DISK_IO  

---

## 1. Symptoms & Observed Errors
```text
[1401] 11 Aug 14:10:02.192 * Asynchronous AOF fsync is taking too long (disk is busy?). Writing the AOF buffer without fsync will slow down Redis, please consider setting no-appendfsync-on-rewrite to yes.
[1401] 11 Aug 14:10:04.195 # Slow query detected: SET order:1942:lock took 2003ms
Client write latency spiked from 0.8ms to 2.1s across application cluster.
```

## 2. Root Cause Analysis
A Redis instance configured with 'appendfsync everysec' encountered an I/O bottleneck on an underlying AWS gp3 EBS volume. When a background fsync takes longer than 2 seconds, Redis's main thread intentionally blocks incoming write commands to prevent unbounded memory buffer growth. Because the main thread is single-threaded, all incoming read and write requests froze for several seconds.

## 3. Breaking Configuration / Problematic Code
```
# redis.conf with appendfsync everysec under slow disk I/O:
appendonly yes
appendfsync everysec
no-appendfsync-on-rewrite no  # Main thread blocks if background rewrite is writing to disk
```

## 4. Remediation Patch / Corrected Configuration
```
# Tuned redis.conf preventing main thread blockage during disk flushes
appendonly yes
appendfsync everysec
no-appendfsync-on-rewrite yes # Prevents fsync() calls while BGSAVE or BGREWRITEAOF are active
# Upgraded EBS volume from baseline 3000 IOPS / 125 MB/s to 6000 IOPS / 500 MB/s throughput
```

## 5. Prevention & Hardening Checklist
- [ ] Set no-appendfsync-on-rewrite yes on write-intensive Redis instances to insulate event loop from I/O pauses
- [ ] Provision dedicated high-throughput NVMe instance storage for AOF persistence logs instead of network-attached EBS
- [ ] Alert on Redis INFO persistence metric aof_delayed_fsync incrementing above zero
