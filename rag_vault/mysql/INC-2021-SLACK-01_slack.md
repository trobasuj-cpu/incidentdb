# [INC-2021-SLACK-01] Global Workspace Connection Blackout via Webapp Rolling Deploy Connection Surge
**Company:** Slack | **Date:** 2021-01-04 | **Severity:** CRITICAL  
**Technologies:** MySQL, Vitess, PHP, Envoy, Redis  
**Categories:** CONNECTION_EXHAUSTION, DEPLOYMENT_FAILURE, DATABASE  

---

## 1. Symptoms & Observed Errors
```text
vttablet: out of connections to database backend (pool size: 500, waiting: 1892)
[CRITICAL] HTTP 504 Gateway Timeout on /api/conversations.history
WebSocket gateway disconnected 1.2M active user sessions
MySQL error: Too many connections (max_connections=4096 exceeded).
```

## 2. Root Cause Analysis
Following the winter holiday freeze, Slack executed a rolling deployment of its primary web application tier. The deployment orchestration spun up 1,200 new application container pods before draining existing pods. The simultaneous overlapping pods overwhelmed the Vitess database proxy connection pools and backend MySQL instances, causing query queues to fill completely, dropping message delivery and user presence globally.

## 3. Breaking Configuration / Problematic Code
```
# Kubernetes deployment manifest with excessive maxSurge:
spec:
  strategy:
    rollingUpdate:
      maxSurge: 100%       # Doubled application pool count simultaneously
      maxUnavailable: 10%
```

## 4. Remediation Patch / Corrected Configuration
```
# Conservative rolling update strategy coupled with client-side connection pooling
spec:
  strategy:
    rollingUpdate:
      maxSurge: 15%        # Strictly limited connection growth margin
      maxUnavailable: 10%
# Vitess vttablet query throttling and transaction pool limits
queryserver-config-pool-size: 300
queryserver-config-max-wait: 2000
queryserver-config-query-timeout: 5000
```

## 5. Prevention & Hardening Checklist
- [ ] Bound Kubernetes deployment maxSurge parameters to prevent instantaneous doubling of backend connection footprints
- [ ] Place database proxies (Vitess / PgBouncer) in front of all relational instances with strict server connection ceilings
- [ ] Run continuous load-testing against rolling deploy pipelines to verify connection headroom
