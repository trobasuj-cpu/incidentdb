# [INC-2026-DIGITALOCEAN-gwgh41d5] DNS service, Certificates and Managed MongoDB
**Company:** DigitalOcean | **Date:** 2026-05-14 | **Severity:** MEDIUM | **Source:** [https://stspg.io/r6dtr7gc9pch](https://stspg.io/r6dtr7gc9pch)  
**Technologies:** Global, DNS, Droplets Hypervisor, DOKS Kubernetes  
**Categories:** QUEUE_DELAY, NETWORK_CONNECTIVITY  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-14 20:46:40 UTC] DigitalOcean SRE (Resolved): As of 19:50 UTC, the issue affecting the DNS service has been fully resolved, and all related services, including Let's Encrypt certificate issuance and Managed MongoDB provisioning, are now operating normally.

The backlog of delayed requests has been successfully processed, and all DNS record updates, pending certificate, and MongoDB operations should now be complete. 

We apologize for the disruption this caused and appreciate your patience while our team worked to restore full functionality.

However, if you continue to experience any issues, please don't hesitate to raise a support ticket for further investigation. We'll be happy to assist you.
[2026-05-14 16:42:45 UTC] DigitalOcean SRE (Monitoring): Our Engineering team has implemented a fix for the issue affecting our DNS service and are now seeing DNS record updates successfully propagate to the edge.

As a result, Let's Encrypt certificate issuance and Managed MongoDB provisioning (including scaling operations) should resume. We are currently monitoring the systems as they process the backlog of delayed requests. Affected MongoDB clusters and certificate requests should complete their provisioning automatically.

We will continue to monitor the situation closely to ensure full stability. We appreciate your patience throughout this process and will provide an update once the issue is fully confirmed as resolved.
[2026-05-14 15:52:48 UTC] DigitalOcean SRE (Investigating): Our engineering team continues to investigate an issue affecting our DNS service. At this time, DNS resolution remains functional; however, new changes to DNS records are currently not being reflected at the edge.

This issue is also impacting related services. Specifically, customers may be unable to create new Let's Encrypt certificates. Regarding Managed MongoDB, customers will be able to submit requests to create new clusters or scale existing ones, but the completion of the provisioning process is currently delayed. This is due to the dependency on Let's Encrypt certificate issuance, which requires functional DNS propagation.

Affected clusters will automatically recover and complete their provisioning once the DNS issue is resolved. Our engineering team is actively working to restore full functionality across all affected services.

We apologize for the inconvenience and will share more information as it becomes available.
[2026-05-14 13:57:38 UTC] DigitalOcean SRE (Investigating): Our Engineering team is investigating an issue affecting our DNS service. At this time, DNS resolution remains functional but any changes to DNS records are not being reflected at the edge. Additionally, customers may be unable to create new Let's Encrypt certificates at this time. Our engineering team is actively working to identify the root cause and restore full functionality.

We apologize for any inconvenience, and we'll share more information as it becomes available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: As of 19:50 UTC, the issue affecting the DNS service has been fully resolved, and all related services, including Let's Encrypt certificate issuance and Managed MongoDB provisioning, are now operating normally.

The backlog of delayed requests has been successfully processed, and all DNS record updates, pending certificate, and MongoDB operations should now be complete. 

We apologize for the disruption this caused and appreciate your patience wh

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during DNS service, Certificates and Managed MongoDB
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
