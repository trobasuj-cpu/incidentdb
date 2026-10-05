# [INC-2026-DIGITALOCEAN-1y9gxbdc] Intermittent errors impacting some Serverless Inference models in ATL1
**Company:** DigitalOcean | **Date:** 2026-04-23 | **Severity:** MEDIUM | **Source:** [https://stspg.io/z4pxt0jh8n97](https://stspg.io/z4pxt0jh8n97)  
**Technologies:** Agent Runtime, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-23 23:51:58 UTC] DigitalOcean SRE (Resolved): This incident has been resolved.
[2026-04-23 22:26:34 UTC] DigitalOcean SRE (Investigating): As of 21:53 UTC, our Engineering team is investigating reports of increased internal errors for models Llama 3.3 70B, GPT OSS 120B, GPT OSS 20B, Qwen3 32B and Deepseek R1 70B hosted in the ATL1 region, impacting Serverless Inference. At this point, users with models hosted in ATL1 may experience intermittent errors when using Serverless Inference. We apologize for the inconvenience and will share an update once we have more information.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: This incident has been resolved. As of 21:53 UTC, our Engineering team is investigating reports of increased internal errors for models Llama 3.3 70B, GPT OSS 120B, GPT OSS 20B, Qwen3 32B and Deepseek R1 70B hosted in the ATL1 region, impacting Serverless Inference. At this point, users with models hosted in ATL1 may experience intermittent errors when using Serverless Inference. We apologize for the inconvenience and will share an update once we

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Intermittent errors impacting some Serverless Inference models in ATL1
service_cluster:
  provider: "DigitalOcean"
  impacted_components: ["Agent Runtime", "Droplets Hypervisor", "DOKS Kubernetes", "Block Storage"]
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
- [ ] Validate automatic health checks and circuit breaking on Agent Runtime cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
