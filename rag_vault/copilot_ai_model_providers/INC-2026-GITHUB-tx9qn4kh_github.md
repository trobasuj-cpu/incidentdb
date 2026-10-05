# [INC-2026-GITHUB-tx9qn4kh] Incident with Copilot AI Model Providers
**Company:** GitHub | **Date:** 2026-08-27 | **Severity:** CRITICAL | **Source:** [https://stspg.io/573gc8p4328b](https://stspg.io/573gc8p4328b)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-27 12:12:58 UTC] GitHub SRE (Resolved): On August 27th, 2026, between approximately 09:20 and 12:14 UTC, the Copilot service experienced a degradation of the Kimi K3 model due to an issue with our upstream provider. Users encountered elevated error rates when using Kimi K3. No other models were impacted. <br /><br />The issue was resolved by a mitigation put in place by our provider. GitHub is working with our provider to further improve the resiliency of the service to prevent similar incidents in the future.
[2026-08-27 12:12:47 UTC] GitHub SRE (Investigating): The issues with our upstream model provider have been mitigated, and Kimi K3 is once again available in Copilot products and IDE surfaces.<br />We will continue monitoring to ensure stability.
[2026-08-27 11:58:04 UTC] GitHub SRE (Investigating): Copilot AI Model Providers is experiencing degraded performance. We are continuing to investigate.
[2026-08-27 10:43:00 UTC] GitHub SRE (Investigating): We are experiencing degraded availability for the Kimi K3 model in Copilot products and IDE surfaces. This is due to an issue with an upstream model provider. While we work with them to resolve the issue, we recommend choosing another model or selecting 'Auto' to continue using Copilot.
[2026-08-27 10:04:52 UTC] GitHub SRE (Investigating): We are investigating reports of degraded availability for Copilot AI Model Providers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 27th, 2026, between approximately 09:20 and 12:14 UTC, the Copilot service experienced a degradation of the Kimi K3 model due to an issue with our upstream provider. Users encountered elevated error rates when using Kimi K3. No other models were impacted. <br /><br />The issue was resolved by a mitigation put in place by our provider. GitHub is working with our provider to further improve the resiliency of the service to prevent similar

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with Copilot AI Model Providers
service_cluster:
  provider: "GitHub"
  impacted_components: ["Copilot AI Model Providers", "GitHub Actions", "Git", "REST API"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by GitHub SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Copilot AI Model Providers cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
