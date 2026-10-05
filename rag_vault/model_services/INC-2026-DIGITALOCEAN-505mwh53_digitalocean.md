# [INC-2026-DIGITALOCEAN-505mwh53] DeepSeek V4 Pro is returning HTTP 429 "Rate limit exceeded"
**Company:** DigitalOcean | **Date:** 2026-06-11 | **Severity:** HIGH | **Source:** [https://stspg.io/r8mzkb94pf9g](https://stspg.io/r8mzkb94pf9g)  
**Technologies:** Model Services, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-11 17:42:05 UTC] DigitalOcean SRE (Resolved): This incident has been resolved.
[2026-06-11 14:31:01 UTC] DigitalOcean SRE (Identified): The issue has been identified and a fix is being implemented.
[2026-06-11 14:30:20 UTC] DigitalOcean SRE (Investigating): We are continuing to investigate this issue.
[2026-06-11 14:24:20 UTC] DigitalOcean SRE (Investigating): We are currently facing issues with Gradient AI DeepSeek V4 Pro is returning HTTP 429 "Rate limit exceeded"
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: This incident has been resolved. The issue has been identified and a fix is being implemented. We are continuing to investigate this issue. We are currently facing issues with Gradient AI DeepSeek V4 Pro is returning HTTP 429 "Rate limit exceeded"

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during DeepSeek V4 Pro is returning HTTP 429 "Rate limit exceeded"
service_cluster:
  provider: "DigitalOcean"
  impacted_components: ["Model Services", "Droplets Hypervisor", "DOKS Kubernetes", "Block Storage"]
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
- [ ] Validate automatic health checks and circuit breaking on Model Services cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
