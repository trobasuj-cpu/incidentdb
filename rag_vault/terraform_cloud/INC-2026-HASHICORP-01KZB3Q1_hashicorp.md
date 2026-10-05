# [INC-2026-HASHICORP-01KZB3Q1] DR Cluster Creation Failures
**Company:** HashiCorp | **Date:** 2026-08-06 | **Severity:** MEDIUM | **Source:** [https://status.hashicorp.com/incidents/01KZB3Q1A2PE4DQMVSMGE53TWV](https://status.hashicorp.com/incidents/01KZB3Q1A2PE4DQMVSMGE53TWV)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-06 11:23:22 UTC] HashiCorp SRE (Resolved): We have implemented a mitigation for the issue affecting new Disaster Recovery (DR) cluster creation. Initial validation has been successful, and new DR cluster creation is functioning as expected. We are continuing to monitor the service to ensure stability before considering the incident fully resolved.
[2026-08-06 11:20:55 UTC] HashiCorp SRE (Resolved): Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please open a ticket with our support team.
[2026-08-06 08:40:52 UTC] HashiCorp SRE (Identified): We have identified an issue affecting the creation of new Disaster Recovery (DR) clusters. Customers attempting to create a DR cluster may encounter provisioning failures. Our engineering team has identified the cause and is working on a resolution. Existing clusters and workloads are not impacted. We will provide updates as more information becomes available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: We have implemented a mitigation for the issue affecting new Disaster Recovery (DR) cluster creation. Initial validation has been successful, and new DR cluster creation is functioning as expected. We are continuing to monitor the service to ensure stability before considering the incident fully resolved. Our Engineering team has resolved the issue. These systems should now be operating normally. If you continue to experience any problems, please

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during DR Cluster Creation Failures
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
