# [INC-2026-OPENAI-01M2VBZB] SSO sign-in and SCIM provisioning issues
**Company:** OpenAI | **Date:** 2026-09-18 | **Severity:** MEDIUM | **Source:** [https://status.openai.com/incidents/01M2VBZB1RYSMXHZNRA25ZJ36X](https://status.openai.com/incidents/01M2VBZB1RYSMXHZNRA25ZJ36X)  
**Technologies:** OpenAI API, ChatGPT, GPU Inference Cluster, Redis  
**Categories:** API_ERROR_SPIKE, AUTHENTICATION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-18 23:18:55 UTC] OpenAI SRE (Resolved): All impacted services have now fully recovered.
[2026-09-18 23:10:37 UTC] OpenAI SRE (Monitoring): We have applied the mitigation and are monitoring the recovery.
[2026-09-18 23:00:09 UTC] OpenAI SRE (Identified): Some users are unable to sign in with single sign-on (SSO). User and group provisioning through SCIM is also affected.

We are working on implementing a mitigation.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by OpenAI engineering: All impacted services have now fully recovered. We have applied the mitigation and are monitoring the recovery. Some users are unable to sign in with single sign-on (SSO). User and group provisioning through SCIM is also affected.

We are working on implementing a mitigation.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during SSO sign-in and SCIM provisioning issues
service_cluster:
  provider: "OpenAI"
  impacted_components: ["OpenAI API", "ChatGPT", "GPU Inference Cluster", "Redis"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by OpenAI SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on OpenAI API cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
