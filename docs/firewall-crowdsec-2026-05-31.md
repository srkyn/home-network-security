# OPNsense Firewall and CrowdSec Telemetry

**Date:** 2026-05-31  
**Collection mode:** read-only API checks  
**Scope:** sanitized production home-network telemetry

This note records a small live telemetry sample from the firewall and security stack. It intentionally omits raw logs, internal addresses, MAC addresses, hostnames, credentials, public IP details, serial numbers, and full topology data.

## Summary

Recent firewall telemetry was reachable through the OPNsense diagnostics API. The sample showed normal stateful firewall activity with a smaller set of blocked inbound events. The blocked events were primarily WAN-side private-network traffic being rejected by the firewall's private-network block rule, plus one default-deny/state-violation event.

CrowdSec status, alert, decision, bouncer, and machine table endpoints were reachable through the OPNsense plugin API. At collection time, the alert and decision tables returned zero active rows. The bouncer and machine tables each returned one registered, valid, recently seen integration.

## Firewall Sample

| Metric | Value |
|---|---:|
| Recent log entries sampled | 500 |
| Passed entries | 458 |
| Blocked entries | 42 |
| Other entries | 0 |

## Action Distribution

| Action | Count | Share |
|---|---:|---:|
| Pass | 458 | 91.6% |
| Block | 42 | 8.4% |

## Protocol Distribution

| Protocol | Count |
|---|---:|
| TCP | 369 |
| ICMP | 47 |
| UDP | 43 |
| IGMP | 41 |

## Blocked-Traffic Notes

The blocked sample did not include destination port data because the blocked events were ICMP/IGMP-style entries rather than TCP/UDP service probes.

Observed blocked-rule categories:

| Category | Count |
|---|---:|
| WAN private-network block rule | 41 |
| Default deny / state violation | 1 |

This is a good sign: the firewall is enforcing expected perimeter behavior and rejecting traffic that should not arrive on the WAN side.

## CrowdSec API Notes

| Check | Result |
|---|---|
| CrowdSec plugin status endpoint | Reachable |
| CrowdSec alerts table endpoint | Reachable, 0 rows |
| CrowdSec decisions table endpoint | Reachable, 0 rows |
| CrowdSec bouncer table endpoint | Reachable, 1 registered bouncer |
| CrowdSec machine table endpoint | Reachable, 1 registered machine |
| CrowdSec metrics endpoint through plugin API | Not available on tested path |

The important operational finding is that CrowdSec was not merely skipped by the collector. The plugin exposed the decision and alert tables, and those tables were empty during the sample window. That is a useful quiet-state baseline.

Detailed row values were not published because CrowdSec bouncer and machine rows can include operational identifiers, internal addressing, and timestamps. Public documentation should continue to publish counts and state, not raw table payloads.

The next safe improvement is to add a dedicated, read-only local collection path for CrowdSec summary data in the internal operations dashboard. That should be done without publishing API keys, raw decision payloads, offender addresses, or internal firewall details.

## Operational Interpretation

The telemetry supports the current design direction:

- Keep OPNsense as the enforcement point.
- Keep UPnP/NAT-PMP disabled unless a specific exception is justified.
- Keep publishing only sanitized summaries, not raw firewall exports.
- Add richer CrowdSec visibility later through a least-privilege, local-only collection path.
- Continue using the operations dashboard for status visibility and Grafana/Victoria-backed views for deeper metrics.

## Public Redaction Rules Used

This document excludes:

- Internal IP addresses.
- Public WAN details.
- MAC addresses.
- Hostnames.
- API keys and tokens.
- Raw log rows.
- Exact interface mappings beyond sanitized role-level descriptions.
- Full inventory or topology details.

## Follow-Up

1. Add a safe CrowdSec summary collector that reports only counts, scenario names, and status.
2. Keep raw telemetry artifacts local and out of Git.
3. Add a dashboard card for sanitized firewall/CrowdSec status once the local feed is stable.
4. Re-run this check after the next firewall or CrowdSec plugin update.
