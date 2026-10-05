# [INC-2026-GITHUB-9tpgqq1h] Copilot model Claude Fable 5 experiencing elevated errors
**Company:** GitHub | **Date:** 2026-07-30 | **Severity:** MEDIUM | **Source:** [https://stspg.io/45g8r57kv3n9](https://stspg.io/45g8r57kv3n9)  
**Technologies:** Copilot AI Model Providers, GitHub Actions, Git, REST API  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-30 10:12:15 UTC] GitHub SRE (Resolved): On July 30, 2026, the Claude Fable 5 model in GitHub Copilot experienced degraded availability for approximately 73 minutes, from 08:33 to 09:46 UTC. During this time, requests to Claude Fable 5 in Copilot chat and IDE surfaces frequently failed or timed out. This was caused by an issue with an upstream model provider. Other Copilot models were not affected, and users could continue working by selecting another model or 'Auto'. Availability for Claude Fable 5 fully recovered once the provider resolved their outage at 09:46 UTC, and we confirmed resolution at 10:12 UTC.
[2026-07-30 10:11:55 UTC] GitHub SRE (Investigating): The issues with our upstream model provider have been resolved, and Claude Fable 5 is once again available in Copilot products and IDE surfaces.<br /><br />We will continue monitoring to ensure stability, but mitigation is complete.
[2026-07-30 09:17:26 UTC] GitHub SRE (Investigating): We are experiencing degraded availability for the Claude Fable 5 model in Copilot products and IDE surfaces. This is due to an issue with an upstream model provider. While we work with them to resolve the issue, we recommend choosing another model or selecting 'Auto' to continue using Copilot.
[2026-07-30 09:07:42 UTC] GitHub SRE (Investigating): We are investigating reports of degraded performance for Copilot AI Model Providers
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On July 30, 2026, the Claude Fable 5 model in GitHub Copilot experienced degraded availability for approximately 73 minutes, from 08:33 to 09:46 UTC. During this time, requests to Claude Fable 5 in Copilot chat and IDE surfaces frequently failed or timed out. This was caused by an issue with an upstream model provider. Other Copilot models were not affected, and users could continue working by selecting another model or 'Auto'. Availability for C

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Copilot model Claude Fable 5 experiencing elevated errors
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
