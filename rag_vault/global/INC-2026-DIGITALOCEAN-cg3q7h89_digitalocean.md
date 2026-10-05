# [INC-2026-DIGITALOCEAN-cg3q7h89] Functions on App Platform
**Company:** DigitalOcean | **Date:** 2026-08-06 | **Severity:** MEDIUM | **Source:** [https://stspg.io/7pm9l1jwpkr6](https://stspg.io/7pm9l1jwpkr6)  
**Technologies:** Global, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** API_ERROR_SPIKE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-06 21:05:05 UTC] DigitalOcean SRE (Resolved): Our Engineering team has confirmed that the issue affecting Functions on App Platform has been fully resolved. Functions builds, deploys, and requests to Functions endpoints are all working as expected. The fix implemented earlier was successful, and we are no longer seeing any problems affecting Functions.

However, if you continue to experience any issues, please don't hesitate to raise a support ticket for further investigation.
[2026-08-06 20:02:03 UTC] DigitalOcean SRE (Monitoring): Our Engineering team has implemented a fix for the issue affecting Functions on App Platform. Functions builds/deploys and requests to Functions endpoints should now be completing successfully. We are currently monitoring the situation to ensure the service has returned to normal operation and remains stable.

We apologize for the inconvenience and will provide a further update once the incident is confirmed resolved.
[2026-08-06 19:01:27 UTC] DigitalOcean SRE (Investigating): Our Engineering team is investigating an issue impacting Functions on App Platform. During this time, users may experience build/deploy failures for Functions components, and requests to Functions endpoints may fail with a 504 error, caused by an SSL error when App Platform attempts to forward requests to Functions. This issue currently appears to be isolated to Functions deployed within App Platform.

We apologize for the inconvenience and will share an update once we have more information.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: Our Engineering team has confirmed that the issue affecting Functions on App Platform has been fully resolved. Functions builds, deploys, and requests to Functions endpoints are all working as expected. The fix implemented earlier was successful, and we are no longer seeing any problems affecting Functions.

However, if you continue to experience any issues, please don't hesitate to raise a support ticket for further investigation. Our Engineerin

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Functions on App Platform
service_cluster:
  provider: "DigitalOcean"
  impacted_components: ["Global", "Droplets Hypervisor", "DOKS Kubernetes", "Block Storage"]
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
- [ ] Validate automatic health checks and circuit breaking on Global cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
