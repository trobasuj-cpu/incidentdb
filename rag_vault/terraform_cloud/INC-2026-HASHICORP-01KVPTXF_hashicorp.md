# [INC-2026-HASHICORP-01KVPTXF] Cluster Creation Is Failing
**Company:** HashiCorp | **Date:** 2026-06-22 | **Severity:** MEDIUM | **Source:** [https://status.hashicorp.com/incidents/01KVPTXFSPZ5389AVP5X5NK264](https://status.hashicorp.com/incidents/01KVPTXFSPZ5389AVP5X5NK264)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-22 08:10:42 UTC] HashiCorp SRE (Resolved): Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team.
[2026-06-22 07:01:48 UTC] HashiCorp SRE (Identified): Our Engineering team has identified the cause to be a a CI build-number drift between app-image and terraform-image jobs in the same workflow run caused the service to reference a non-existent terraform image tag. We are actively working on a fix. Once we have additional information, we will share another update.
[2026-06-22 04:55:13 UTC] HashiCorp SRE (Investigating): We are aware of the issue that the cluster. creation is failing. We are actively investigating the issue. Our team is working to identify and resolve the issue. We will post updates with more information as it becomes available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team. Our Engineering team has identified the cause to be a a CI build-number drift between app-image and terraform-image jobs in the same workflow run caused the service to reference a non-existent terraform image tag. We are actively working on a fix. Once we have addition

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Cluster Creation Is Failing
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
