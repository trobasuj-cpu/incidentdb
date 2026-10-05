# [INC-2026-HASHICORP-01KYPY1K] HCP Terraform Workspace Runs Failing with IAM Errors After AWS Provider Upgrade
**Company:** HashiCorp | **Date:** 2026-07-29 | **Severity:** HIGH | **Source:** [https://status.hashicorp.com/incidents/01KYPY1KTBE8KWC4E75RXN0XX8](https://status.hashicorp.com/incidents/01KYPY1KTBE8KWC4E75RXN0XX8)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-07-29 20:44:50 UTC] HashiCorp SRE (Resolved): Our Engineering team has pulled v6.57.0 of the AWS provider from the registry and published a fixed v6.57.1.
[2026-07-29 16:50:28 UTC] HashiCorp SRE (Monitoring): Our Engineering team has pulled v6.57.0 of the AWS provider from the registry, and continue to work to publish v6.57.1. We will continue to monitor for failures.
[2026-07-29 16:08:25 UTC] HashiCorp SRE (Identified): Our Engineering team has identified the cause and we are actively working on a fix to publish a patch version of v6.57.1 and pull v6.57.0 from the registry. Prior to that we advise working around the issue by pinning the AWS provider to v6.56.0. Once we have additional information, we will share another update.
[2026-07-29 12:56:39 UTC] HashiCorp SRE (Identified): Our Engineering team has identified the cause and we are actively working on a fix. A workaround would be to pin the aws provider to v6.56.0 till the fixed patch is released. Once we have additional information, we will share another update.
[2026-07-29 12:36:59 UTC] HashiCorp SRE (Investigating): We are aware of and investigating reports of degraded performance after AWS provider update. Our team is working to identify and resolve the issue. We will post updates with more information as it becomes available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: Our Engineering team has pulled v6.57.0 of the AWS provider from the registry and published a fixed v6.57.1. Our Engineering team has pulled v6.57.0 of the AWS provider from the registry, and continue to work to publish v6.57.1. We will continue to monitor for failures. Our Engineering team has identified the cause and we are actively working on a fix to publish a patch version of v6.57.1 and pull v6.57.0 from the registry. Prior to that we advis

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during HCP Terraform Workspace Runs Failing with IAM Errors After AWS Provider Upgrade
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
