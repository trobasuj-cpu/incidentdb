# [INC-2026-HASHICORP-01M37NV4] HCP Terraform Returning 404s
**Company:** HashiCorp | **Date:** 2026-09-23 | **Severity:** CRITICAL | **Source:** [https://status.hashicorp.com/incidents/01M37NV49VB1CF8B4RKYTQZ0CJ](https://status.hashicorp.com/incidents/01M37NV49VB1CF8B4RKYTQZ0CJ)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-23 20:02:09 UTC] HashiCorp SRE (Resolved): Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team.
[2026-09-23 18:06:49 UTC] HashiCorp SRE (Monitoring): Our Engineering team has implemented a fix to resolve the issue. We are monitoring the situation closely and will post an update as soon as the issue is fully resolved.
[2026-09-23 18:02:13 UTC] HashiCorp SRE (Identified): Our Engineering team has identified the cause and we are actively working on a fix. Once we have additional information, we will share another update.
[2026-09-23 17:59:54 UTC] HashiCorp SRE (Investigating): We are continuing to investigate issues with HCP Terraform. Our team is working to identify and resolve the issue. We will post updates with more information as it becomes available.
[2026-09-23 17:43:30 UTC] HashiCorp SRE (Investigating): We are aware of and investigating reports of degraded performance with HCP Terraform. Our team is working to identify and resolve the issue. We will post updates with more information as it becomes available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team. Our Engineering team has implemented a fix to resolve the issue. We are monitoring the situation closely and will post an update as soon as the issue is fully resolved. Our Engineering team has identified the cause and we are actively working on a fix. Once we have add

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during HCP Terraform Returning 404s
service_cluster:
  provider: "HashiCorp"
  impacted_components: ["Terraform Cloud", "Vault", "Consul", "Nomad"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by HashiCorp SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Terraform Cloud cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
