# [INC-2026-DIGITALOCEAN-qd8v4wdd] Major Service Interruption in MKC1
**Company:** DigitalOcean | **Date:** 2026-08-19 | **Severity:** CRITICAL | **Source:** [https://stspg.io/gdzqj0v18x0r](https://stspg.io/gdzqj0v18x0r)  
**Technologies:** MKC1, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** DATABASE_DEGRADATION, NETWORK_CONNECTIVITY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-19 19:34:54 UTC] DigitalOcean SRE (Resolved): Our Engineering team has confirmed the regional control plane is healthy and CPU Droplets, Managed databases, Load balancers, Block storage, Kubernetes (DOKS) and Spaces are operating normally.

GPU Droplets continue to be impacted and our teams are working to restore all nodes. We will communicate with GPU Droplet customers separately via Slack and email with more information and regular updates.

Thank you for your patience throughout this incident. If you continue to experience any issues, please open a support ticket from within your account.
[2026-08-19 18:59:03 UTC] DigitalOcean SRE (Monitoring): At this time, the regional control plane is fully healthy. CPU Droplets, Managed databases, Load balancers, Block storage and Spaces are operating normally. Droplet create, resize and other management operations are working. Kubernetes (DOKS) control planes are reachable.

Our teams are continuing to work with the facility to restore all equipment. GPU Droplets in MKC1 remain offline or unreachable. DOKS GPU worker nodes may remain NotReady, and GPU-backed inference endpoints in this region may be unavailable. 

We will monitor the regional control plane for a short time and then resolve this incident. GPU customers will receive personalized updates with more information in lieu of this status page. 

If you have questions about your affected resources, contact support and reference this incident.
[2026-08-19 15:28:37 UTC] DigitalOcean SRE (Identified): Our Engineering team continues to work on the issue affecting the MKC1 region. We are actively working to restore connectivity and bring the impacted nodes back online.
We will provide another update as soon as we have more information.
[2026-08-19 12:22:47 UTC] DigitalOcean SRE (Identified): We have identified the root cause of the issue in the MKC1 region affecting multiple racks and nodes. Our Engineering team is actively implementing remediation steps to restore connectivity and bring the impacted nodes back online.
During this time, customers may continue to experience disruptions to GPU workloads and Serverless Inference. Kubernetes (DOKS) worker nodes may also remain in a NotReady state, and customers may be unable to reach Kubernetes API endpoints or perform cluster-management operations in affected clusters. We will provide another update as soon as we have more information.
[2026-08-19 11:10:06 UTC] DigitalOcean SRE (Investigating): Our Engineering team is currently investigating an issue in the MKC1 region affecting multiple racks and nodes.

During this time, customers may experience disruptions to GPU workloads, and Kubernetes (DOKS) worker nodes may enter a NotReady state.

Our team is actively working to restore connectivity and bring impacted nodes back online. If you continue to experience issues, please open a support ticket from within your account.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: Our Engineering team has confirmed the regional control plane is healthy and CPU Droplets, Managed databases, Load balancers, Block storage, Kubernetes (DOKS) and Spaces are operating normally.

GPU Droplets continue to be impacted and our teams are working to restore all nodes. We will communicate with GPU Droplet customers separately via Slack and email with more information and regular updates.

Thank you for your patience throughout this inci

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Major Service Interruption in MKC1
service_cluster:
  provider: "DigitalOcean"
  impacted_components: ["MKC1", "Droplets Hypervisor", "DOKS Kubernetes", "Block Storage"]
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
- [ ] Validate automatic health checks and circuit breaking on MKC1 cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
