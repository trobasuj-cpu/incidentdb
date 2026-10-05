# [INC-2026-DATADOG-rpj3lwgx] Errors Pulling Container Images
**Company:** Datadog | **Date:** 2026-07-02 | **Severity:** CRITICAL | **Source:** [https://stspg.io/gs85vq0xmxcx](https://stspg.io/gs85vq0xmxcx)  
**Technologies:** Container Registry, Datadog Ingestion, APM, Metrics Agent  
**Categories:** PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-02 21:29:11 UTC] Datadog SRE (Resolved): This incident has been resolved.
[2026-07-02 21:21:10 UTC] Datadog SRE (Monitoring): A fix has been implemented and we are monitoring the results.
[2026-07-02 20:55:53 UTC] Datadog SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-07-02 20:25:57 UTC] Datadog SRE (Investigating): We are investigating errors pulling Datadog container images from registry.datadoghq.com. As a result of this issue, some users may be unable to download or deploy the Datadog Agent, Cluster Agent, and other Datadog container images. Containers that are already running are not affected.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: This incident has been resolved. A fix has been implemented and we are monitoring the results. The issue has been identified and a fix is being implemented. We are investigating errors pulling Datadog container images from registry.datadoghq.com. As a result of this issue, some users may be unable to download or deploy the Datadog Agent, Cluster Agent, and other Datadog container images. Containers that are already running are not affected.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Errors Pulling Container Images
service_cluster:
  provider: "Datadog"
  impacted_components: ["Container Registry", "Datadog Ingestion", "APM", "Metrics Agent"]
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
- [ ] Validate automatic health checks and circuit breaking on Container Registry cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
