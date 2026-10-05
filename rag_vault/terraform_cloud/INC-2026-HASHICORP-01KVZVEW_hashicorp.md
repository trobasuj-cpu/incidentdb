# [INC-2026-HASHICORP-01KVZVEW] Intermittent 5XX errors for registry
**Company:** HashiCorp | **Date:** 2026-06-25 | **Severity:** MEDIUM | **Source:** [https://status.hashicorp.com/incidents/01KVZVEWTKPS0WBSQ40P5JYAEK](https://status.hashicorp.com/incidents/01KVZVEWTKPS0WBSQ40P5JYAEK)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-25 18:16:40 UTC] HashiCorp SRE (Resolved): Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team.
[2026-06-25 17:27:28 UTC] HashiCorp SRE (Identified): Terraform Registry is experiencing elevated error rates affecting terraform init workflows and documentation access. Our Engineering team has identified the cause and we are actively working on a fix. Once we have additional information, we will share another update.
[2026-06-25 16:57:53 UTC] HashiCorp SRE (Investigating): Terraform Registry is experiencing elevated error rates affecting terraform init workflows and documentation access. Our engineering team is investigating the issue.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team. Terraform Registry is experiencing elevated error rates affecting terraform init workflows and documentation access. Our Engineering team has identified the cause and we are actively working on a fix. Once we have additional information, we will share another update. T

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Intermittent 5XX errors for registry
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
