# [INC-2021-ROBLOX-01] 73-Hour Global Service Outage via HashiCorp Consul epoll Starvation and KV Replication Deadlock
**Company:** Roblox | **Date:** 2021-10-28 | **Severity:** CRITICAL  
**Technologies:** HashiCorp Consul, Nomad, Envoy, Go  
**Categories:** CONSUL_DEADLOCK, SERVICE_DISCOVERY, EPOLL_STARVATION  

---

## 1. Symptoms & Observed Errors
```text
consul.raft: failed to heartbeat to 10.20.14.9: context deadline exceeded
consul: streaming backend connection pool full; blocking new subscribers
nomad.client: failed to resolve upstream service addresses: connection refused
Roblox platform unavailable worldwide for 73 consecutive hours.
```

## 2. Root Cause Analysis
Roblox enabled a new feature in HashiCorp Consul (Streaming) designed to reduce network traffic from service polling. Under elevated backend traffic, a subtle lock contention bug in the Go runtime interacting with the Linux epoll subsystem caused worker threads to block while holding internal lock mutexes. All Consul servers locked up simultaneously, paralyzing service discovery, internal routing, and Nomad workload orchestrators for three days.

## 3. Breaking Configuration / Problematic Code
```
# Consul configuration enabling experimental streaming:
{
  "streaming": {
    "enabled": true
  },
  "performance": {
    "raft_multiplier": 1
  }
}
```

## 4. Remediation Patch / Corrected Configuration
```
# Reverted streaming and configured tuned raft parameters with circular buffer tuning
{
  "streaming": {
    "enabled": false
  },
  "performance": {
    "raft_multiplier": 3
  },
  "limits": {
    "rpc_max_conns_per_client": 100
  }
}
```

## 5. Prevention & Hardening Checklist
- [ ] Test critical infrastructure control plane upgrades under 3x peak load in fully isolated benchmark topologies
- [ ] Ensure core service discovery systems have cached fallback routing tables to permit degraded operations
- [ ] Maintain decoupled independent bootstrap orchestrators that do not circular-depend on the service discovery layer
