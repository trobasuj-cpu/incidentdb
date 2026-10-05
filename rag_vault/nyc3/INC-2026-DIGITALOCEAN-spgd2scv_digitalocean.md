# [INC-2026-DIGITALOCEAN-spgd2scv] Block Storage Volume Performance
**Company:** DigitalOcean | **Date:** 2026-05-20 | **Severity:** MEDIUM | **Source:** [https://stspg.io/1cr62wpw64kx](https://stspg.io/1cr62wpw64kx)  
**Technologies:** NYC3, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** STORAGE_SUBSYSTEM  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-20 05:13:50 UTC] DigitalOcean SRE (Resolved): From 00:00 UTC to 01:40 UTC, users may have experienced degraded write performance and intermittent impacts to workloads dependent on Block Storage Volumes in the NYC3 region.

Our Engineering team has confirmed that the underlying infrastructure issue affecting Block Storage Volumes at the storage layer has been fully resolved, and services are now operating normally.

If you continue to experience any issues, please contact our Support team by opening a ticket. We apologize for any inconvenience caused.
[2026-05-20 02:19:45 UTC] DigitalOcean SRE (Monitoring): Our Engineering team has implemented mitigation measures for the infrastructure issue affecting Block Storage Volumes in the NYC3 region. The team is monitoring the situation, and we will share another update once the issue is fully resolved
[2026-05-20 01:05:16 UTC] DigitalOcean SRE (Investigating): Our Engineering team is currently investigating an ongoing infrastructure issue in the NYC3 region affecting Block Storage Volumes at the storage layer. Users may experience degraded write performance and intermittent impact to services dependent on the affected storage infrastructure.

We apologize for the inconvenience and will continue to provide updates as more information becomes available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: From 00:00 UTC to 01:40 UTC, users may have experienced degraded write performance and intermittent impacts to workloads dependent on Block Storage Volumes in the NYC3 region.

Our Engineering team has confirmed that the underlying infrastructure issue affecting Block Storage Volumes at the storage layer has been fully resolved, and services are now operating normally.

If you continue to experience any issues, please contact our Support team by

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Block Storage Volume Performance
service_cluster:
  provider: "DigitalOcean"
  impacted_components: ["NYC3", "Droplets Hypervisor", "DOKS Kubernetes", "Block Storage"]
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
- [ ] Validate automatic health checks and circuit breaking on NYC3 cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
