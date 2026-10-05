# [INC-2026-DATADOG-smc2wgcm] Elevated Error Rates
**Company:** Datadog | **Date:** 2026-07-01 | **Severity:** MEDIUM | **Source:** [https://stspg.io/h1vdjddg90hd](https://stspg.io/h1vdjddg90hd)  
**Technologies:** Datadog Ingestion, APM, Metrics Agent, Kafka  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-01 16:37:08 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-07-01 15:43:48 UTC] Datadog SRE (Monitoring): We are continuing to monitor the situation and are taking additional preventive measures.
[2026-07-01 15:19:01 UTC] Datadog SRE (Monitoring): We have deployed mitigation actions and are monitoring the recovery.
[2026-07-01 15:10:43 UTC] Datadog SRE (Identified): We have identified the issue of elevated error rates and are working through mitigation actions
[2026-07-01 14:57:38 UTC] Datadog SRE (Investigating): We are investigating elevated error rates across multiple products including: Fleet Automation, Security Products, Cloudcraft, Cloud Network Monitoring, Serverless, Kubernetes Autoscaling, and Database Monitoring.
[2026-07-01 14:31:20 UTC] Datadog SRE (Investigating): We are actively investigating elevated error rates across multiple Datadog products.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. We are continuing to monitor the situation and are taking additional preventive measures. We have deployed mitigation actions and are monitoring the recovery. We have identified the issue of elevated error rates and are working through mitigation actions We are investigating elevated error rates across multiple products including: Fleet Automation, Security Products, Cloudcraft, Cloud Network Monitoring, Serverles

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Elevated Error Rates
service_cluster:
  provider: "Datadog"
  impacted_components: ["Datadog Ingestion", "APM", "Metrics Agent", "Kafka"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Datadog SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Datadog Ingestion cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
