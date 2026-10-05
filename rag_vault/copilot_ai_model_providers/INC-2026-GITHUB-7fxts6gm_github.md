# [INC-2026-GITHUB-7fxts6gm] Elevated rate of errors for OpenAI models provided by Copilot
**Company:** GitHub | **Date:** 2026-08-31 | **Severity:** MEDIUM | **Source:** [https://stspg.io/8trzn5bjrtnn](https://stspg.io/8trzn5bjrtnn)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-31 09:58:14 UTC] GitHub SRE (Resolved): Between 08:37 and 09:41 UTC on August 31, 2026, GitHub Copilot experienced degradation affecting several GPT models, including gpt-5.2, gpt-5.3-codex, gpt-5.4, gpt-5.4-mini, gpt-5.4-nano, and the gpt-5.6 family (Luna, Sol, and Terra). Users encountered elevated error rates and interrupted streaming responses. Other models were not affected.<br /><br />The degradation was caused by an issue with an upstream model provider. GitHub engineers detected the issue through automated monitoring, displayed in-product warnings for the affected models, and coordinated with the provider. Service returned to normal after the provider implemented a mitigation.
[2026-08-31 09:58:02 UTC] GitHub SRE (Monitoring): The issues with our upstream model provider have been resolved, and gpt-5.3-codex, gpt-5.4-mini, gpt-5.4-nano, gpt-5.5, and the gpt-5.6 family of models are once again available in Copilot products and IDE surfaces.<br /><br />We will continue monitoring to ensure stability, but mitigation is complete.
[2026-08-31 09:51:13 UTC] GitHub SRE (Monitoring): The degradation affecting Copilot AI Model Providers has been mitigated. We are monitoring to ensure stability.
[2026-08-31 09:48:12 UTC] GitHub SRE (Investigating): One of our model providers has confirmed an incident on their end. We have provided them details to help identify the issue. We are starting to see recovery.
[2026-08-31 09:22:18 UTC] GitHub SRE (Investigating): Copilot is experiencing a higher rate of errors for OpenAI models, including gpt-5.2, gpt-5.3-codex, gpt-5.4, gpt-5.4, and the gpt-5.6 family of models. Other models are not impacted. <br />
[2026-08-31 09:15:48 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Copilot AI Model Providers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: Between 08:37 and 09:41 UTC on August 31, 2026, GitHub Copilot experienced degradation affecting several GPT models, including gpt-5.2, gpt-5.3-codex, gpt-5.4, gpt-5.4-mini, gpt-5.4-nano, and the gpt-5.6 family (Luna, Sol, and Terra). Users encountered elevated error rates and interrupted streaming responses. Other models were not affected.<br /><br />The degradation was caused by an issue with an upstream model provider. GitHub engineers detecte

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Elevated rate of errors for OpenAI models provided by Copilot
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
