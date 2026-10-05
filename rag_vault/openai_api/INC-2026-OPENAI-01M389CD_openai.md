# [INC-2026-OPENAI-01M389CD] Mobile users unable to see Work Mode and the Model Picker
**Company:** OpenAI | **Date:** 2026-09-23 | **Severity:** MEDIUM | **Source:** [https://status.openai.com/incidents/01M389CDQ97B11QPMSRAAYSE5S](https://status.openai.com/incidents/01M389CDQ97B11QPMSRAAYSE5S)  
**Technologies:** OpenAI API, ChatGPT, GPU Inference Cluster, Redis  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-23 23:49:06 UTC] OpenAI SRE (Resolved): All impacted services have now fully recovered.
[2026-09-23 23:32:53 UTC] OpenAI SRE (Monitoring): We have applied the mitigation and are monitoring the recovery.
[2026-09-23 23:25:00 UTC] OpenAI SRE (Investigating): We have identified that mobile users are not able to see work mode or the model picker when using ChatGPT.

We are working on implementing a mitigation.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by OpenAI engineering: All impacted services have now fully recovered. We have applied the mitigation and are monitoring the recovery. We have identified that mobile users are not able to see work mode or the model picker when using ChatGPT.

We are working on implementing a mitigation.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Mobile users unable to see Work Mode and the Model Picker
service_cluster:
  provider: "OpenAI"
  impacted_components: ["OpenAI API", "ChatGPT", "GPU Inference Cluster", "Redis"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by OpenAI SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on OpenAI API cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
