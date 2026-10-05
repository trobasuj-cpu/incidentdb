# [INC-2026-GITHUB-zkxwbgr0] Incident with GitHub.com
**Company:** GitHub | **Date:** 2026-08-17 | **Severity:** CRITICAL | **Source:** [https://stspg.io/y1fl26l6wpzr](https://stspg.io/y1fl26l6wpzr)  
**Technologies:** Git Operations, Webhooks, API Requests, Issues  
**Categories:** QUEUE_DELAY, NETWORK_CONNECTIVITY, API_ERROR_SPIKE  

---

## 1. Symptoms & Observed Errors
```text
[2026-08-17 21:15:46 UTC] GitHub SRE (Resolved): On August 17, 2026, from 13:28–21:15 UTC (7h 47m), GitHub.com experienced elevated errors and latency across Issues, Pull Requests, APIs, Actions, and Copilot. At peak, web/API error rates were approximately 20%, while archive and raw-content downloads reached approximately 50%. SAML/OIDC authentication, SCIM, and Team Sync were also affected, as well as Actions workflows in GHEC with Data Residency that depend on public workflow step definitions hosted on GitHub.com. Most services recovered by 16:36 UTC as our Central US datacenter recovered; Actions was degraded until approximately 18:03 UTC; and Copilot Token Service fully recovered by 21:02. <br /><br />Some of the failing traffic was moved from Central US to Northern Virginia where it was served successfully until the network failure in Central US was debugged and resolved. Delayed replies to a single internal endpoint triggered a latent retry bug in VS Code that amplified traffic by approximately 10x and caused delayed recovery for the Copilot Token Service. <br /><br />The immediate cause of the failure was network saturation on load balancers in Central US due to a new peak in traffic. Originally this was caused by an Istio sidecar pod reaching its concurrency limits and failing to auto scale correctly because of a misconfigured policy that watched host service but not sidecar limits. One failure cascaded to more and eventually four HAProxy nodes exhausted their flow limits, degrading the gateway auth path and causing widespread authentication latency and failures. The problem was worsened by optimistic retry logic which overloaded internal load balancers. Pausing HAProxy on those nodes simultaneously produced immediate broad recovery. <br /><br />The retry storm in Northern VA was fixed by 1) temporarily reducing gateway retry logic with a PR and 2) blocking inbound Copilot Token Service token requests at the load balancers with a 403, and then gradually ramping back up traffic per-site to allow callers to succeed. <br /><br />Residual Copilot authentication failures continued because client retry behavior amplified load: a failed token operation could generate many extra requests and enter a retry loop. Copilot Token Service traffic increased from a normal 7–9K RPS to 70–100K RPS. Reducing gateway authentication retries and blocking retry-triggering responses stabilized Copilot Token Service and completed recovery. <br /><br />Complicating factors that impeded recovery included a number of scraping attacks on codeload endpoints. <br /><br />To prevent recurrence, our follow-up actions include: <br /><br />- Correcting autoscaling policies to account for service-mesh sidecar concurrency and capacity. <br /><br />- Auditing Istio request, concurrency, and scaling limits across affected services. <br /><br />- Reviewing retry limits and backoff behavior across gateways and clients. <br /><br />- Addressing the VS Code retry behavior that amplified Copilot token traffic. <br /><br />- Improving load-balancer capacity monitoring and regional failover safeguards.
[2026-08-17 20:45:28 UTC] GitHub SRE (Investigating): We are continuing to apply mitigations to address sporadic Copilot authentication failures in some applications. We expect full recovery within the next 30 minutes. Copilot usage via the GitHub CLI and GitHub App are unaffected.
[2026-08-17 20:22:28 UTC] GitHub SRE (Investigating): Issues is operating normally.
[2026-08-17 20:08:40 UTC] GitHub SRE (Investigating): We are continuing to investigate sporadic failures affecting Copilot authentication in some applications. Copilot usage via the GitHub CLI and GitHub App are unaffected.
[2026-08-17 19:13:20 UTC] GitHub SRE (Investigating): We are continuing to investigate sporadic authentication failures. We have partially disabled authentication token retries and have seen improvement, and we are monitoring impact before fully applying this mitigation.
[2026-08-17 19:01:45 UTC] GitHub SRE (Investigating): API Requests is operating normally.
[2026-08-17 18:48:19 UTC] GitHub SRE (Investigating): API Requests is experiencing degraded availability. We are continuing to investigate.
[2026-08-17 18:23:47 UTC] GitHub SRE (Investigating): The degradation affecting Git Operations has been mitigated. We are monitoring to ensure stability.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by GitHub engineering: On August 17, 2026, from 13:28–21:15 UTC (7h 47m), GitHub.com experienced elevated errors and latency across Issues, Pull Requests, APIs, Actions, and Copilot. At peak, web/API error rates were approximately 20%, while archive and raw-content downloads reached approximately 50%. SAML/OIDC authentication, SCIM, and Team Sync were also affected, as well as Actions workflows in GHEC with Data Residency that depend on public workflow step definitions

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Incident with GitHub.com
service_cluster:
  provider: "GitHub"
  impacted_components: ["Git Operations", "Webhooks", "API Requests", "Issues"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by GitHub SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Git Operations cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
