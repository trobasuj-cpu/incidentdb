# [INC-2026-GITHUB-wt3hjqcr] Elevated errors on Fable 5 due to upstream provider
**Company:** GitHub | **Date:** 2026-08-24 | **Severity:** HIGH | **Source:** [https://stspg.io/4dz9rv14dz14](https://stspg.io/4dz9rv14dz14)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-24 07:58:41 UTC] GitHub SRE (Resolved): On August 24th, 2026, between approximately 06:35 and 07:25 UTC, the Copilot service experienced a degradation of the Claude Fable 5 model due to an issue with our upstream provider. Users encountered elevated error rates when using Claude Fable 5, with requests sometimes failing mid-response. No other models were impacted.<br /><br />The issue was resolved by a mitigation put in place by our provider. GitHub is working with our provider to further improve the resiliency of the service to prevent similar incidents in the future.
[2026-08-24 07:12:33 UTC] GitHub SRE (Investigating): We are experiencing degraded availability for the Fable model in Copilot products and IDE surfaces. This is due to an issue with the upstream model provider. While we work with them to resolve the issue, we recommend choosing another model or selecting 'Auto' to continue using Copilot.
[2026-08-24 07:12:05 UTC] GitHub SRE (Investigating): We are investigating reports of degraded availability for Copilot AI Model Providers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 24th, 2026, between approximately 06:35 and 07:25 UTC, the Copilot service experienced a degradation of the Claude Fable 5 model due to an issue with our upstream provider. Users encountered elevated error rates when using Claude Fable 5, with requests sometimes failing mid-response. No other models were impacted.<br /><br />The issue was resolved by a mitigation put in place by our provider. GitHub is working with our provider to furth

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Elevated errors on Fable 5 due to upstream provider
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
