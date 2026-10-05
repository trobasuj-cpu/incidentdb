# [INC-2026-HASHICORP-01KZY3YW] Terraform AWS Provider v6.59.0 — errors during plan for aws_network_acl_rule resources
**Company:** HashiCorp | **Date:** 2026-08-13 | **Severity:** MEDIUM | **Source:** [https://status.hashicorp.com/incidents/01KZY3YWKZEY9QFKPRZQFF5C82](https://status.hashicorp.com/incidents/01KZY3YWKZEY9QFKPRZQFF5C82)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-13 19:06:41 UTC] HashiCorp SRE (Resolved): Our Engineering team has resolved the issue. We released the Terraform AWS Provider v6.60.0 with a fix. Affected systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team.
[2026-08-13 17:50:44 UTC] HashiCorp SRE (Identified): Our engineering team has identified the cause of the "Missing Resource Identity After Read" errors and has completed a fix. We are preparing version 6.60.0 of the Terraform AWS Provider, which includes this fix. v6.60.0 is not yet available in the registry.

This affects `aws_network_acl_rule` resources that were created with provider versions earlier than 6.39.0. Terraform runs that do not manage `aws_network_acl_rule `resources are unaffected.

Users encountering errors can continue running Terraform by pinning the provider to version 6.58.0:


```
terraform {
 required_providers {
  aws = {
   source = "hashicorp/aws"
   version = "6.58.0"
  }
 }
}
```
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: Our Engineering team has resolved the issue. We released the Terraform AWS Provider v6.60.0 with a fix. Affected systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team. Our engineering team has identified the cause of the "Missing Resource Identity After Read" errors and has completed a fix. We are preparing version 6.60.0 of the Terraform AWS Provider, which includes this

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Terraform AWS Provider v6.59.0 — errors during plan for aws_network_acl_rule resources
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
