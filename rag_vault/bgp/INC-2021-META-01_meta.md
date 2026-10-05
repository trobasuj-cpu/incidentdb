# [INC-2021-META-01] Global Outage via Automated Network Maintenance Command Withdrawing All Backbone BGP Routes
**Company:** Meta | **Date:** 2021-10-04 | **Severity:** CRITICAL  
**Technologies:** BGP, DNS, Juniper, Cisco  
**Categories:** BGP_ROUTE_LEAK, DNS_RESOLUTION_FAILURE, NETWORK_ISOLATION  

---

## 1. Symptoms & Observed Errors
```text
BGP Error: Withdrawing prefix 129.134.0.0/16 from AS32934
BGP Error: Withdrawing prefix 157.240.0.0/16 from AS32934
DNS SERVFAIL: a.ns.facebook.com: connection refused
Host unreachable: internal-tools.facebook.com (ICMP Network Unreachable)
Physical badge readers offline across all corporate campus facilities.
```

## 2. Root Cause Analysis
During routine maintenance to assess global backbone capacity, a command was issued to evaluate backbone availability. The automated audit tool inadvertently tore down all physical connections in the backbone network. Facebook's authoritative DNS servers detected that their connection to the internal data centers was lost, and per design, deactivated their BGP route advertisements. Consequently, Facebook's DNS servers disappeared from the global internet routing tables, making WhatsApp, Instagram, and internal communication systems inaccessible.

## 3. Breaking Configuration / Problematic Code
```
# Automated backbone audit script issuing unhedged link termination:
audit_network_backbone --isolate-peering-links --scope GLOBAL_TIER
# Missing guardrail: Executed across all global transit links simultaneously without canary boundaries
```

## 4. Remediation Patch / Corrected Configuration
```
# Hardened peering audit tool with physical site limits and out-of-band serial consoles
def audit_network_backbone(scope: str, max_isolated_pct: float = 0.05) -> None:
    if scope == "GLOBAL_TIER":
        raise PermissionError("Global tier isolation prohibited; must audit single geographic region")
    ensure_out_of_band_access_verified()
```

## 5. Prevention & Hardening Checklist
- [ ] Ensure DNS servers continue advertising BGP routes over out-of-band management planes during internal backbone isolation
- [ ] Require independent multi-party dual authorization for any commands capable of modifying global peering routes
- [ ] Maintain physical hardware serial console access to data center routers decoupled from primary identity networks
