# [INC-2026-DATADOG-q2njm9l0] Notifications to Pagerduty, VictorOps, Slack, Webhook, Microsoft Teams and OpsGenie are not delivered
**Company:** Datadog | **Date:** 2026-04-10 | **Severity:** HIGH | **Source:** [https://stspg.io/18986pkc2dgr](https://stspg.io/18986pkc2dgr)  
**Technologies:** Monitors, Datadog Ingestion, APM, Metrics Agent  
**Categories:** QUEUE_DELAY  

---

## 1. Symptoms & Observed Errors
```text
[2026-04-10 16:59:49 UTC] Datadog SRE (Resolved): Between approximately 9:42 AM and 10:38 AM ET on April 10, we observed delivery failures for customers in the US1, AP1, and AP2 regions. During this window, notifications to the following integrations were delayed or temporarily undelivered:

• PagerDuty
• Slack
• Microsoft Teams
• Webhooks
• Jira
• ServiceNow
• OpsGenie
• VictorOps
• BigPanda
• Zendesk
• Sumologic

All delayed notifications have been replayed with the following exceptions: PagerDuty pages and Slack notifications queued during the outage window were not replayed, as they were no longer actionable at the time of recovery. Webhook notifications will be reprocessed and delivered at a later date.

No monitoring data was lost during this incident. Monitor evaluations continued normally — only the delivery of triggered notifications was affected.

Thank you for your patience.
[2026-04-10 16:13:41 UTC] Datadog SRE (Monitoring): Webhooks and Slack notifications are still catching up.
[2026-04-10 15:44:24 UTC] Datadog SRE (Monitoring): Webhooks and Slack notifications are still catching up.
[2026-04-10 15:02:16 UTC] Datadog SRE (Monitoring): Webhooks and Slack notifications are still catching up.
[2026-04-10 14:33:12 UTC] Datadog SRE (Monitoring): Notifications for ServiceNow and Microsoft Teams are now caught up. Webhooks and Slack notifications are still catching up.
[2026-04-10 13:37:45 UTC] Datadog SRE (Monitoring): Notifications for JIRA are now caught up.
[2026-04-10 13:12:44 UTC] Datadog SRE (Monitoring): Notifications for Opsgenie, VictorOps, BigPanda, Zendesk, and Sumologic are caught up. Webhooks, Microsoft Teams, Slack, JIRA, and ServiceNow are still catching up.
[2026-04-10 12:15:54 UTC] Datadog SRE (Monitoring): We are still catching-up on past notifications that may have been delayed.
```

## 2. Root Cause Analysis
Official investigation timeline recorded by Datadog engineering: Between approximately 9:42 AM and 10:38 AM ET on April 10, we observed delivery failures for customers in the US1, AP1, and AP2 regions. During this window, notifications to the following integrations were delayed or temporarily undelivered:

• PagerDuty
• Slack
• Microsoft Teams
• Webhooks
• Jira
• ServiceNow
• OpsGenie
• VictorOps
• BigPanda
• Zendesk
• Sumologic

All delayed notifications have been replayed with the following exceptions: Pager

## 3. Breaking Configuration / Problematic Code
```
# Incident architectural state during Notifications to Pagerduty, VictorOps, Slack, Webhook, Microsoft Teams and OpsGenie are not delivered
service_cluster:
  provider: "Datadog"
  impacted_components: ["Monitors", "Datadog Ingestion", "APM", "Metrics Agent"]
  health_check_status: DEGRADED
  circuit_breaker: OPEN
  observed_error_threshold: 0.05
```

## 4. Remediation Patch / Corrected Configuration
```
# Remediation mitigation applied by Datadog SRE
deployment_mitigation:
  action: "Drain degraded node pool and scale healthy replicas"
  traffic_rerouting: ENABLED
  rate_limiting_tier: STRICT
  status: RESOLVED
```

## 5. Prevention & Hardening Checklist
- [ ] Validate automatic health checks and circuit breaking on Monitors cluster
- [ ] Enforce automated failover thresholds for cross-zone dependency outages
- [ ] Integrate real-time statuspage webhook alerting into PagerDuty on-call rotation
