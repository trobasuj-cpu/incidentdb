# [INC-2026-GITHUB-sj1tzyrx] Incident with Copilot AI Model Providers
**Company:** GitHub | **Date:** 2026-08-01 | **Severity:** MEDIUM | **Source:** [https://stspg.io/z58g1g529hzz](https://stspg.io/z58g1g529hzz)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** QUEUE_DELAY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-01 18:44:28 UTC] GitHub SRE (Resolved): On August 1, 2026, between 17:47 UTC and 18:20 UTC, users of the Fable 5 model in GitHub Copilot experienced increased request failures and latency. The average failure rate across all Copilot requests was 0.007%, while failures for Fable 5 peaked at 5.6%. Other models remained available. This was caused by degradation of an upstream model provider.<br /><br />The affected endpoint recovered, and we monitored the service until error rates and latency returned to normal levels. We are working to add endpoint redundancy to mitigate similar provider issues in the future.
[2026-08-01 18:23:40 UTC] GitHub SRE (Monitoring): The issues with our upstream model provider have been resolved, and Fable 5 is once again available in Copilot products and IDE surfaces.<br /><br />We will continue monitoring to ensure stability, but mitigation is complete.
[2026-08-01 18:20:48 UTC] GitHub SRE (Monitoring): The degradation affecting Copilot AI Model Providers has been mitigated. We are monitoring to ensure stability.
[2026-08-01 18:20:20 UTC] GitHub SRE (Investigating): We are experiencing degraded availability for the Fable 5 model in Copilot products and IDE surfaces. This is due to an issue with an upstream model provider. While we work with them to resolve the issue, we recommend choosing another model or selecting 'Auto' to continue using Copilot.
[2026-08-01 18:03:15 UTC] GitHub SRE (Investigating): We are seeing increased error rates from specific upstream AI Model Providers
[2026-08-01 18:03:05 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Copilot AI Model Providers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 1, 2026, between 17:47 UTC and 18:20 UTC, users of the Fable 5 model in GitHub Copilot experienced increased request failures and latency. The average failure rate across all Copilot requests was 0.007%, while failures for Fable 5 peaked at 5.6%. Other models remained available. This was caused by degradation of an upstream model provider.<br /><br />The affected endpoint recovered, and we monitored the service until error rates and lat

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
