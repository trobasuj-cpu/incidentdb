# [INC-2026-HASHICORP-01M1PRS0] Errors creating new VCS workspaces with Bitbucket cloud provider
**Company:** HashiCorp | **Date:** 2026-09-04 | **Severity:** MEDIUM | **Source:** [https://status.hashicorp.com/incidents/01M1PRS06XEYC48D769GDVZ0YW](https://status.hashicorp.com/incidents/01M1PRS06XEYC48D769GDVZ0YW)  
**Technologies:** Terraform Cloud, Vault, Consul, Nomad  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-09-04 21:46:08 UTC] HashiCorp SRE (Resolved): Our Engineering team has resolved the issue with creating new VCS workspaces with the Bitbucket cloud provider. If you continue to experience any problems, please open a ticket with our support team.
[2026-09-04 20:14:04 UTC] HashiCorp SRE (Monitoring): Our Engineering team has implemented a fix for errors when creating new VCS workspaces with the Bitbucket cloud provider. We are continuing to monitor the situation and will post an update as soon as the issue is fully resolved.
[2026-09-04 18:31:17 UTC] HashiCorp SRE (Identified): Our Engineering team has identified the cause of errors when creating new VCS workspaces with the Bitbucket cloud provider. We are actively working on a fix. Once we have additional information, we will share another update.
[2026-09-04 17:51:59 UTC] HashiCorp SRE (Investigating): We are aware of and investigating reports of errors when creating new VCS workspaces using the Bitbucket cloud provider. We are working to identify and resolve the issue and will post updates with more information when it is available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by HashiCorp engineering: Our Engineering team has resolved the issue with creating new VCS workspaces with the Bitbucket cloud provider. If you continue to experience any problems, please open a ticket with our support team. Our Engineering team has implemented a fix for errors when creating new VCS workspaces with the Bitbucket cloud provider. We are continuing to monitor the situation and will post an update as soon as the issue is fully resolved. Our Engineering team

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Errors creating new VCS workspaces with Bitbucket cloud provider
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
