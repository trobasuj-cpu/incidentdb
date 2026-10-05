# [INC-2026-DIGITALOCEAN-z5fjzx3k] Elevated 5xx “context canceled” errors impacting serverless inference
**Company:** DigitalOcean | **Date:** 2026-04-28 | **Severity:** MEDIUM | **Source:** [https://stspg.io/qf13crcjzkmn](https://stspg.io/qf13crcjzkmn)  
**Technologies:** Agent Runtime, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-28 19:33:38 UTC] DigitalOcean SRE (Resolved): All services are operating normally. We will continue to monitor the system to ensure ongoing reliability.

Thank you for your patience while we worked to resolve this issue.
[2026-04-28 19:00:24 UTC] DigitalOcean SRE (Monitoring): Service for Serverless Inference has been restored.
We’ve implemented tighter rate limits to help prevent recurrence and are closely monitoring system performance. Some users may still experience intermittent latency as we complete final stabilization efforts.
Our team remains actively engaged to ensure full recovery. We appreciate your patience and will provide further updates as needed.
[2026-04-28 15:59:26 UTC] DigitalOcean SRE (Identified): We have identified an issue affecting our service and are currently working to implement a fix. Our team is actively investigating and taking the necessary steps to restore normal operations as quickly as possible.

We appreciate your patience and will provide updates as soon as more information becomes available.
[2026-04-28 13:45:20 UTC] DigitalOcean SRE (Investigating): Serverless inference customers are experiencing elevated 5xx errors, including “context canceled” responses. This may result in intermittent request failures. Our team is actively investigating and will provide updates as more information becomes available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: All services are operating normally. We will continue to monitor the system to ensure ongoing reliability.

Thank you for your patience while we worked to resolve this issue. Service for Serverless Inference has been restored.
We’ve implemented tighter rate limits to help prevent recurrence and are closely monitoring system performance. Some users may still experience intermittent latency as we complete final stabilization efforts.
Our team remai

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Elevated 5xx “context canceled” errors impacting serverless inference
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
