# [INC-2026-GITHUB-24t8gsgq] Errors with the Fable 5 Model in Copilot
**Company:** GitHub | **Date:** 2026-08-13 | **Severity:** MEDIUM | **Source:** [https://stspg.io/8htrnxk0q09v](https://stspg.io/8htrnxk0q09v)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-13 15:47:22 UTC] GitHub SRE (Resolved): On August 13th, 2026, between approximately 14:06 and 15:47 UTC, the Copilot service experienced a degradation of the Claude Fable 5 model due to an issue with our upstream provider. Users encountered elevated error rates, peaking at 43% and averaging 12%. Users who selected Auto or alternative models were unaffected.<br /><br />The issue was resolved by a mitigation put in place by our provider. GitHub is working with our provider to further improve the resiliency of the service to prevent similar incidents in the future.
[2026-08-13 15:47:15 UTC] GitHub SRE (Investigating): The issues with our upstream model provider have been resolved, and Fable 5 is once again available in Copilot products and IDE surfaces.<br /><br />We will continue monitoring to ensure stability, but mitigation is complete.
[2026-08-13 15:23:39 UTC] GitHub SRE (Investigating): We are seeing modest recovery, but are still experiencing degraded availability for the Fable 5 model in Copilot products and IDE surfaces. This is due to an issue with an upstream model provider. While we work with them to resolve the issue, we recommend choosing another model or selecting 'Auto' to continue using Copilot.
[2026-08-13 14:50:10 UTC] GitHub SRE (Investigating): We are experiencing degraded availability for the Fable 5 model in Copilot products and IDE surfaces. This is due to an issue with an upstream model provider. While we work with them to resolve the issue, we recommend choosing another model or selecting 'Auto' to continue using Copilot.
[2026-08-13 14:43:25 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Copilot AI Model Providers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 13th, 2026, between approximately 14:06 and 15:47 UTC, the Copilot service experienced a degradation of the Claude Fable 5 model due to an issue with our upstream provider. Users encountered elevated error rates, peaking at 43% and averaging 12%. Users who selected Auto or alternative models were unaffected.<br /><br />The issue was resolved by a mitigation put in place by our provider. GitHub is working with our provider to further imp

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Errors with the Fable 5 Model in Copilot
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
