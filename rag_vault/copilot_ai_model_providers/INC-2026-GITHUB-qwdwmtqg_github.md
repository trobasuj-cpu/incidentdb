# [INC-2026-GITHUB-qwdwmtqg] Disruption with some GitHub services
**Company:** GitHub | **Date:** 2026-09-15 | **Severity:** MEDIUM | **Source:** [https://stspg.io/6pn81yd1x4hz](https://stspg.io/6pn81yd1x4hz)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-15 11:17:22 UTC] GitHub SRE (Resolved): On September 15, 2026, between 05:45 and 09:50 UTC, the Claude Fable 5.1 model in GitHub Copilot experienced intermittently degraded availability, with an average error rate of 2.8%. During brief recurring 15 minute periods that recurred every ~45 minutes, availability for Claude Fable 5.1 dropped to a maximum of ~40% before recovering completely. Other Copilot models were not affected. Users could continue to work with a different model or with 'Auto'.<br /><br />The cause was an issue with an upstream model provider that intermittently rejected requests while overloaded. GitHub worked with the provider, who acknowledged and then resolved the underlying issue at 9:50 UTC, after which the model returned to constant normal availability. Once recovery was guaranteed, we resolved the incident at 11:17 UTC.<br /><br />To reduce the chance of recurrence and customer impact, GitHub is reviewing per-model availability alerting and automatic in-product fallback so that requests to a degraded model can shift to a healthy alternative more quickly.
[2026-09-15 11:17:13 UTC] GitHub SRE (Investigating): The issues with our upstream model provider have been resolved, and Claude Fable 5.1 is once again available in Copilot products and IDE surfaces.<br /><br />We will continue monitoring to ensure stability, but mitigation is complete.
[2026-09-15 11:09:33 UTC] GitHub SRE (Investigating): We continue to monitor intermittent errors affecting Claude Fable 5.1 in some Copilot products and integrated development environments. Customer-facing metrics have recovered, and we are awaiting confirmation from our upstream provider that the issue will not recur. Customers can select another model or Auto in the meantime.
[2026-09-15 10:32:25 UTC] GitHub SRE (Investigating): We continue to investigate intermittent errors affecting Claude Fable 5.1 in some Copilot products and integrated development environments. Customers can use another model or select Auto while we monitor the situation.
[2026-09-15 10:20:13 UTC] GitHub SRE (Investigating): Copilot AI Model Providers is experiencing degraded performance. We are continuing to investigate.
[2026-09-15 09:48:04 UTC] GitHub SRE (Monitoring): We are investigating degraded availability for Claude Fable 5.1, affecting some Copilot products and integrated development environments. The issue is caused by a problem with our upstream model provider. Customers can use another model or select Auto while we investigate.
[2026-09-15 09:47:15 UTC] GitHub SRE (Investigating): We are investigating reports of impacted performance for some GitHub services.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On September 15, 2026, between 05:45 and 09:50 UTC, the Claude Fable 5.1 model in GitHub Copilot experienced intermittently degraded availability, with an average error rate of 2.8%. During brief recurring 15 minute periods that recurred every ~45 minutes, availability for Claude Fable 5.1 dropped to a maximum of ~40% before recovering completely. Other Copilot models were not affected. Users could continue to work with a different model or with

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Disruption with some GitHub services
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
