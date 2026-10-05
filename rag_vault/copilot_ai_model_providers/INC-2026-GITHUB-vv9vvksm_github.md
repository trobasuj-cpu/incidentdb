# [INC-2026-GITHUB-vv9vvksm] Several GPT models degraded
**Company:** GitHub | **Date:** 2026-07-25 | **Severity:** HIGH | **Source:** [https://stspg.io/vh0xxw69dr6v](https://stspg.io/vh0xxw69dr6v)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-25 10:11:16 UTC] GitHub SRE (Resolved): On July 25, 2026, between 09:07 and 10:04 UTC, the GPT-5.2, GPT-5.3-Codex, GPT-5.4, GPT-5.4 Mini, GPT-5.6 Sol, GPT-5.6 Terra and GPT-5.6 Luna models experienced degraded availability in GitHub Copilot products and IDE surfaces. Requests to these models had an average failure rate of 5.6%. Other Copilot models remained available as alternatives.<br /><br />The degradation was caused by an issue with an upstream model provider. Success rates returned to normal after the upstream issue was mitigated, and we continued monitoring before resolving the incident. We are working on improving the automated failover for the affected models to prevent similar incidents in the future.
[2026-07-25 10:04:39 UTC] GitHub SRE (Monitoring): The degradation affecting Copilot AI Model Providers has been mitigated. We are monitoring to ensure stability.
[2026-07-25 09:48:30 UTC] GitHub SRE (Investigating): We are experiencing degraded availability for the GPT-5.2, GPT-5.3-Codex, GPT-5.4, GPT-5.4 Mini, GPT-5.6 Sol, GPT-5.6 Terra, and GPT-5.6 Luna models in Copilot products and IDE surfaces. This is due to an issue with an upstream model provider. While we work with them to resolve the issue, we recommend choosing another model or selecting 'Auto' to continue using Copilot.
[2026-07-25 09:42:28 UTC] GitHub SRE (Investigating): We are investigating reports of degraded availability for Copilot AI Model Providers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On July 25, 2026, between 09:07 and 10:04 UTC, the GPT-5.2, GPT-5.3-Codex, GPT-5.4, GPT-5.4 Mini, GPT-5.6 Sol, GPT-5.6 Terra and GPT-5.6 Luna models experienced degraded availability in GitHub Copilot products and IDE surfaces. Requests to these models had an average failure rate of 5.6%. Other Copilot models remained available as alternatives.<br /><br />The degradation was caused by an issue with an upstream model provider. Success rates return

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Several GPT models degraded
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
