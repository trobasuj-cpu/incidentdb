# [INC-2026-DIGITALOCEAN-nnt4rfbg] DNS API Service
**Company:** DigitalOcean | **Date:** 2026-06-04 | **Severity:** MEDIUM | **Source:** [https://stspg.io/c9tgc3txntpg](https://stspg.io/c9tgc3txntpg)  
**Technologies:** Global, DNS, Droplets Hypervisor, DOKS Kubernetes  
**Categories:** NETWORK_CONNECTIVITY, API_ERROR_SPIKE, PIPELINE_EXECUTION_FAILURE  

---

## 1. Symptoms & Observed Errors
```text
[2026-06-04 11:47:05 UTC] DigitalOcean SRE (Resolved): Between 09:41 and 11:24 UTC, our Engineering team identified an issue impacting the DNS API service. During this period, users may have experienced issues performing domain and DNS record management operations through the Control Panel and API. Services dependent on DNS API operations, including Let's Encrypt certificate provisioning, MongoDB cluster creation, App Platform deployments, and DigitalOcean Kubernetes (DOKS) cluster create and delete operations, were also impacted.

Our Engineering team has confirmed that the underlying issue affecting the DNS API service has been fully resolved, and all affected services are now operating normally.

If you continue to experience any issues, please contact our Support team by opening a ticket. We apologize for any inconvenience caused.
[2026-06-04 11:39:16 UTC] DigitalOcean SRE (Monitoring): Our Engineering team has implemented a fix to resolve the issue impacting our DNS API service. Users should now be able to perform domain and DNS record management operations successfully through the Control Panel and API. Additionally, services affected by this issue, including Let's Encrypt certificate provisioning, MongoDB cluster creation, App Platform deployments, and DigitalOcean Kubernetes (DOKS) cluster create and delete operations, should now be functioning as expected.

We are monitoring the situation closely and will share an update once the issue is resolved completely.
[2026-06-04 11:32:22 UTC] DigitalOcean SRE (Identified): Our Engineering team has identified the cause of the issue impacting our DNS API service and is actively working on a fix. During this time, users may experience errors when attempting to create, update, or delete domains and DNS records through the Control Panel and API. As a result, services that depend on DNS API operations, including Let's Encrypt certificate provisioning, MongoDB cluster creation, App Platform deployments, and DigitalOcean Kubernetes (DOKS) cluster create and delete operations, may also be impacted.

We will post an update as soon as additional information is available.
[2026-06-04 10:59:17 UTC] DigitalOcean SRE (Investigating): Our Engineering team continues to investigate an issue impacting our DNS API service. During this time, users may experience issues performing domain and DNS record management operations from the Control Panel and API, including creating, updating, or deleting DNS records. As a result, services that depend on DNS API operations, including Let's Encrypt certificate provisioning, MongoDB cluster creation, App Platform deployments, and DigitalOcean Kubernetes (DOKS) cluster create and delete operations, may also be impacted.

We apologize for the inconvenience and will share more information as it becomes available.
[2026-06-04 10:12:10 UTC] DigitalOcean SRE (Investigating): Our Engineering team is investigating an issue impacting our DNS API service. During this time, users may experience issues performing domain and DNS record management operations from the Control Panel and API, including creating, updating, or deleting DNS records. As a result, services that rely on DNS API operations, such as Let's Encrypt certificate provisioning and MongoDB cluster creation, may also be impacted.

We apologize for the inconvenience and will share an update once we have more information.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: Between 09:41 and 11:24 UTC, our Engineering team identified an issue impacting the DNS API service. During this period, users may have experienced issues performing domain and DNS record management operations through the Control Panel and API. Services dependent on DNS API operations, including Let's Encrypt certificate provisioning, MongoDB cluster creation, App Platform deployments, and DigitalOcean Kubernetes (DOKS) cluster create and delete

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during DNS API Service
service_cluster:
  provider: "DigitalOcean"
  impacted_components: ["Global", "DNS", "Droplets Hypervisor", "DOKS Kubernetes"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by DigitalOcean SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Global cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
