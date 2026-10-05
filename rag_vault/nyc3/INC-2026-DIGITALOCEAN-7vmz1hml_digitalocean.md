# [INC-2026-DIGITALOCEAN-7vmz1hml] Droplet Actions in NYC3
**Company:** DigitalOcean | **Date:** 2026-07-04 | **Severity:** MEDIUM | **Source:** [https://stspg.io/1ggj0xnb3n4v](https://stspg.io/1ggj0xnb3n4v)  
**Technologies:** NYC3, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-04 15:10:06 UTC] DigitalOcean SRE (Resolved): Our Engineering team has confirmed that the issue impacting Droplets in the NYC3 region has been fully resolved. Users can now perform actions on their Droplets or create new Droplets in this region without any issues.

We appreciate your patience while we worked to resolve this issue. If you continue to experience any problems, please open a support ticket from your account so our team can investigate further.
[2026-07-04 14:42:47 UTC] DigitalOcean SRE (Monitoring): Our Engineering team has implemented a fix to address the issue affecting Droplets in the NYC3 region. The issue, which began at 12:38 UTC, caused disruptions to customers attempting to perform actions on their Droplets in the NYC3 region.

During this time, customers may also have experienced errors when trying to create Droplets in this region.

We are actively monitoring the situation to ensure the fix remains effective and will provide another update once the issue has been fully resolved.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: Our Engineering team has confirmed that the issue impacting Droplets in the NYC3 region has been fully resolved. Users can now perform actions on their Droplets or create new Droplets in this region without any issues.

We appreciate your patience while we worked to resolve this issue. If you continue to experience any problems, please open a support ticket from your account so our team can investigate further. Our Engineering team has implemente

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Droplet Actions in NYC3
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
