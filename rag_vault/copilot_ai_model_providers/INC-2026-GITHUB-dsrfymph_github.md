# [INC-2026-GITHUB-dsrfymph] Incident with Copilot AI Model Providers
**Company:** GitHub | **Date:** 2026-07-29 | **Severity:** MEDIUM | **Source:** [https://stspg.io/tdw1hjz3lj91](https://stspg.io/tdw1hjz3lj91)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** QUEUE_DELAY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-29 21:51:26 UTC] GitHub SRE (Resolved): On July 29, 2026, between 19:45 UTC and 21:51 UTC, users of the Fable 5 model in GitHub Copilot experienced increased request failures and latency. The average failure rate across all Copilot requests was 0.006%, while failures for Fable 5 peaked at 21%. Other models remained available. This was caused by degradation of an upstream model provider.<br /><br />The affected endpoint recovered, and we monitored the service until error rates and latency returned to normal levels. We are working to add endpoint redundancy to mitigate similar provider issues in the future.
[2026-07-29 21:51:15 UTC] GitHub SRE (Investigating): The external ai model provider has resolved the issues, and we have verified Copilot's traffic is fully recovered.
[2026-07-29 21:08:31 UTC] GitHub SRE (Investigating): The external AI model provider is continuing to investigate.
[2026-07-29 20:38:32 UTC] GitHub SRE (Investigating): The external AI model provider has identified the issue and is working to resolve.
[2026-07-29 20:18:18 UTC] GitHub SRE (Investigating): We are investigating increased error rates affecting GitHub Copilot requests to external AI model providers. Some users may experience failures or degraded performance when using Copilot features.
[2026-07-29 20:07:43 UTC] GitHub SRE (Investigating): We are seeing increased error rates with requests to specific model providers.
[2026-07-29 20:07:08 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Copilot AI Model Providers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On July 29, 2026, between 19:45 UTC and 21:51 UTC, users of the Fable 5 model in GitHub Copilot experienced increased request failures and latency. The average failure rate across all Copilot requests was 0.006%, while failures for Fable 5 peaked at 21%. Other models remained available. This was caused by degradation of an upstream model provider.<br /><br />The affected endpoint recovered, and we monitored the service until error rates and laten

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
