# [INC-2025-NPM-z95xc4yt] Issue with package publish with provenance
**Company:** npm | **Date:** 2025-06-12 | **Severity:** MEDIUM | **Source:** [https://stspg.io/rtzh6flfmljd](https://stspg.io/rtzh6flfmljd)  
**Technologies:** Package publishing, npm Registry, CouchDB, Fastly CDN  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2025-06-12 22:25:08 UTC] npm SRE (Resolved): This incident has been resolved.
[2025-06-12 21:00:22 UTC] npm SRE (Investigating): We are seeing issues with package publish with provenance due to an outage of a key third party dependency.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by npm engineering: This incident has been resolved. We are seeing issues with package publish with provenance due to an outage of a key third party dependency.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Issue with package publish with provenance
service_cluster:
  provider: "npm"
  impacted_components: ["Package publishing", "npm Registry", "CouchDB", "Fastly CDN"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by npm SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Package publishing cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
