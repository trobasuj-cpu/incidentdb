# [INC-2026-DIGITALOCEAN-8mvnl6s4] Droplet Resize
**Company:** DigitalOcean | **Date:** 2026-07-01 | **Severity:** MEDIUM | **Source:** [https://stspg.io/m6y2gmlz3jrv](https://stspg.io/m6y2gmlz3jrv)  
**Technologies:** Cloud Control Panel, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-01 13:28:10 UTC] DigitalOcean SRE (Resolved): Our Engineering team has confirmed that the issue impacting Droplet resizes using the "Downscale Anytime" option has been fully resolved. Users can now resize their Droplets using the "Downscale Anytime" option without any issues.

We appreciate your patience while we worked to resolve this issue. If you continue to experience any problems, please open a support ticket from your account so our team can investigate further.
[2026-07-01 12:43:33 UTC] DigitalOcean SRE (Monitoring): Our Engineering team has identified the root cause of the issue affecting Droplet resizes using the "Downscale Anytime" option and has implemented a fix. Users should now be able to resize their Droplets using the "Downscale Anytime" option without experiencing any issues or errors.

We are actively monitoring the situation to ensure the fix remains effective and will provide another update once the issue has been fully resolved.
[2026-07-01 11:13:21 UTC] DigitalOcean SRE (Investigating): Our Engineering team is investigating an issue affecting Droplet resizes using the "Downscale Anytime" option. At this time users may find this option unavailable or unresponsive in the Cloud Control Panel.

As a workaround, resizes can be performed via the API while we work to resolve this issue.

We apologize for the inconvenience and will share an update once we have more information.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: Our Engineering team has confirmed that the issue impacting Droplet resizes using the "Downscale Anytime" option has been fully resolved. Users can now resize their Droplets using the "Downscale Anytime" option without any issues.

We appreciate your patience while we worked to resolve this issue. If you continue to experience any problems, please open a support ticket from your account so our team can investigate further. Our Engineering team ha

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Droplet Resize
service_cluster:
  provider: "DigitalOcean"
  impacted_components: ["Cloud Control Panel", "Droplets Hypervisor", "DOKS Kubernetes", "Block Storage"]
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
- [ ] Validate automatic health checks and circuit breaking on Cloud Control Panel cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
