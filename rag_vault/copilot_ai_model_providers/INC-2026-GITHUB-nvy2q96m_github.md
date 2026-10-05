# [INC-2026-GITHUB-nvy2q96m] Elevated rate of errors for OpenAI models provided by Copilot
**Company:** GitHub | **Date:** 2026-09-17 | **Severity:** MEDIUM | **Source:** [https://stspg.io/t938cy7649sn](https://stspg.io/t938cy7649sn)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-17 21:49:01 UTC] GitHub SRE (Resolved): Between 20:26 and 21:17 UTC on September 17, 2026, GitHub Copilot experienced degradation affecting several GPT models, including GPT-5.6 Luna, GPT-5.6 Terra, GPT-5.6 Sol, GPT-5.3-Codex, and GPT-6 Astra. Users encountered elevated error rates when using these models.<br /><br />The degradation was caused by an issue with an upstream model provider. GitHub engineers detected the issue through automated monitoring and coordinated with the provider. Our automated model-warning system activated in-product warnings for affected models during the incident. Service returned to normal after the provider implemented a mitigation.
[2026-09-17 21:39:28 UTC] GitHub SRE (Monitoring): We are experiencing degraded availability for GPT-5.6 Luna, GPT-5.6 Terra, GPT-5.6 Sol, GPT-6 Astra, GPT-5.3-Codex in Copilot products and IDE surfaces. This is due to an issue with an upstream model provider. The provider is working to mitigate the problem and we are monitoring recovery. We recommend choosing another model or selecting 'Auto' to continue using Copilot.
[2026-09-17 20:59:02 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Copilot AI Model Providers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: Between 20:26 and 21:17 UTC on September 17, 2026, GitHub Copilot experienced degradation affecting several GPT models, including GPT-5.6 Luna, GPT-5.6 Terra, GPT-5.6 Sol, GPT-5.3-Codex, and GPT-6 Astra. Users encountered elevated error rates when using these models.<br /><br />The degradation was caused by an issue with an upstream model provider. GitHub engineers detected the issue through automated monitoring and coordinated with the provider.

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
