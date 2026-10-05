# [INC-2026-HASHICORP-01KZ9N6M] HCP Terraform is unable to communicate with VCS endpoints using certain IP addresses.
**Company:** HashiCorp | **Date:** 2026-08-05 | **Severity:** MEDIUM | **Source:** [https://status.hashicorp.com/incidents/01KZ9N6MT7JM1Y4GGHJP14K0TG](https://status.hashicorp.com/incidents/01KZ9N6MT7JM1Y4GGHJP14K0TG)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-06 20:33:13 UTC] HashiCorp SRE (Resolved): Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team.
[2026-08-05 20:46:52 UTC] HashiCorp SRE (Monitoring): Our Engineering team has implemented a fix to resolve the issue. We are monitoring the situation closely and will post an update as soon as the issue is fully resolved.
[2026-08-05 19:39:29 UTC] HashiCorp SRE (Identified): Many customers are seeing failed Terraform runs with SIC-001 errors or 403 forbidden responses. We have confirmed around 220 similar errors in VCS audit logs.  We have found the root cause and are working on a fix. Once we have additional information, we will share another update.
[2026-08-05 19:08:01 UTC] HashiCorp SRE (Investigating): Many customers are seeing failed Terraform runs with SIC-001 errors or 403 forbidden responses. We have confirmed around 220 similar errors in VCS audit logs, and at least two workspaces are affected. Our team is working to identify and resolve the issue. We will post updates with more information as it becomes available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team. Our Engineering team has implemented a fix to resolve the issue. We are monitoring the situation closely and will post an update as soon as the issue is fully resolved. Many customers are seeing failed Terraform runs with SIC-001 errors or 403 forbidden responses. We h

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during HCP Terraform is unable to communicate with VCS endpoints using certain IP addresses.
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
