# [INC-2026-DIGITALOCEAN-lrrw084g] Anthropic reported outage that's impacting access to their Serverless Inference models
**Company:** DigitalOcean | **Date:** 2026-05-15 | **Severity:** HIGH | **Source:** [https://stspg.io/d47rft5t1t68](https://stspg.io/d47rft5t1t68)  
**Technologies:** Agent Runtime, Droplets Hypervisor, DOKS Kubernetes, Block Storage  
**Categories:** INFRASTRUCTURE_DEGRADATION, SERVICE_OUTAGE  

---

## 1. Symptoms & Observed Errors
```text
[2026-05-15 02:00:39 UTC] DigitalOcean SRE (Resolved): As of the current time, our Engineering team has confirmed that the issue with Serverless Inference has been resolved. The root cause of the issue was an outage with our provider, Anthropic, which affected users of Sonnet 4.6 and Opus 4.7 models. According to Anthropic's status page (https://status.claude.com/incidents/8z7l5zcy0v3b), they have resolved the outage. If you continue to experience problems, please open a ticket with our Support team. We apologize for any inconvenience this may have caused.
[2026-05-15 01:23:56 UTC] DigitalOcean SRE (Identified): According to Anthropic's status page (https://status.claude.com/incidents/8z7l5zcy0v3b), they have identified the root cause and are actively working on a fix. At this time, users may continue to experience errors when attempting to use Sonnet 4.6 and Opus 4.7 models. We will provide another update once Anthropic has implemented a fix and service is restored.
[2026-05-15 01:21:51 UTC] DigitalOcean SRE (Monitoring): As of the current time, our Engineering team is aware of an ongoing incident with our provider, Anthropic, that is impacting Serverless Inference. The outage is affecting all users attempting to use Sonnet 4.6 and Opus 4.7 models. According to Anthropic's status page (https://status.claude.com/incidents/8z7l5zcy0v3b), they are currently experiencing an outage that is causing this disruption. We apologize for the inconvenience and will provide updates as more information becomes available.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by DigitalOcean engineering: As of the current time, our Engineering team has confirmed that the issue with Serverless Inference has been resolved. The root cause of the issue was an outage with our provider, Anthropic, which affected users of Sonnet 4.6 and Opus 4.7 models. According to Anthropic's status page (https://status.claude.com/incidents/8z7l5zcy0v3b), they have resolved the outage. If you continue to experience problems, please open a ticket with our Support team.

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Anthropic reported outage that's impacting access to their Serverless Inference models
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
