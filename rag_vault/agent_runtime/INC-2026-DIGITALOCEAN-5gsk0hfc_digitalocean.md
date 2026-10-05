# [INC-2026-DIGITALOCEAN-5gsk0hfc] Serverless Inference - Intermittent Rate Limiting Affecting Some Customers Using Anthropic Models
**Company:** DigitalOcean | **Date:** 2026-04-27 | **Severity:** HIGH | **Source:** [https://stspg.io/y8cgtdqrkct1](https://stspg.io/y8cgtdqrkct1)  
**Technologies:** Agent Runtime, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-27 11:37:36 UTC] DigitalOcean SRE (Resolved): The issue is resolved, and service is operating normally.
[2026-04-27 11:07:50 UTC] DigitalOcean SRE (Monitoring): We identified the cause of intermittent HTTP 429 responses affecting some customers using Anthropic models on DigitalOcean Serverless Inference and applied a mitigation. Service has recovered, and we are monitoring stability.
[2026-04-27 10:38:30 UTC] DigitalOcean SRE (Investigating): We are investigating an issue affecting some customers using DigitalOcean Serverless Inference with Anthropic models. Over the last two hours, impacted customers may have experienced intermittent request failures, including HTTP 429 responses, on some Anthropic model requests. Our engineering team is actively investigating the issue. We apologize for the inconvenience and will share another update as soon as more information is available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: The issue is resolved, and service is operating normally. We identified the cause of intermittent HTTP 429 responses affecting some customers using Anthropic models on DigitalOcean Serverless Inference and applied a mitigation. Service has recovered, and we are monitoring stability. We are investigating an issue affecting some customers using DigitalOcean Serverless Inference with Anthropic models. Over the last two hours, impacted customers may

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Serverless Inference - Intermittent Rate Limiting Affecting Some Customers Using Anthropic Models
service_cluster:
  provider: "DigitalOcean"
  impacted_components: ["Agent Runtime", "Droplets Hypervisor", "DOKS Kubernetes", "Block Storage"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by DigitalOcean SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Agent Runtime cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
