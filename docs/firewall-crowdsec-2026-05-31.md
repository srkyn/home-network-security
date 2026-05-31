# OPNsense Firewall and CrowdSec Telemetry

**Date:** 2026-05-31  
**Collection mode:** read-only API checks  
**Scope:** sanitized production home-network telemetry

This note records a small live telemetry sample from the firewall and security stack. It intentionally omits raw logs, internal addresses, MAC addresses, hostnames, credentials, public IP details, serial numbers, and full topology data.

## Summary

Recent firewall telemetry was reachable through the OPNsense diagnostics API. The sample showed normal stateful firewall activity with a smaller set of blocked inbound events. The blocked events were primarily WAN-side private-network traffic being rejected by the firewall's private-network block rule, plus one default-deny/state-violation event.

CrowdSec status, alert, decision, bouncer, and machine table endpoints were reachable through the OPNsense plugin API. At collection time, the alert and decision tables returned zero local rows. The bouncer and machine tables each returned one registered, valid, recently seen integration. The OPNsense GUI confirmed the same result on the CrowdSec Alerts and Decisions pages.

The shell-only CrowdSec CLI view showed the larger picture: the firewall bouncer had thousands of active CAPI/community blocklist decisions. Those decisions do not appear in the normal OPNsense Decisions table.

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
| OPNsense CrowdSec GUI alerts page | Reachable, no rows |
| OPNsense CrowdSec GUI decisions page | Reachable, no rows |
| CrowdSec community/CAPI decisions | Present through shell `cscli decisions list -a` |

The important operational finding is that CrowdSec was not merely skipped by the collector. The plugin exposed the local decision and alert tables, and those tables were empty during the sample window. The CAPI/community blocklist view, however, was active and visible through `cscli`.

The OPNsense Decisions page includes an important note: decisions coming from CrowdSec CAPI/community blocklists do not appear in that GUI table. The command to validate those from the firewall shell is:

```sh
cscli decisions list -a -o json
```

Shell access was used temporarily to collect the CAPI/community decision summary. A temporary SSH public key was added to the root account for collection and then removed after collection. The temporary key no longer authenticated after cleanup.

Detailed row values were not published because CrowdSec bouncer and machine rows can include operational identifiers, internal addressing, and timestamps. Public documentation should continue to publish counts and state, not raw table payloads.

The next safe improvement is to add a dedicated, read-only local collection path for CrowdSec summary data in the internal operations dashboard. That should be done without publishing API keys, raw decision payloads, offender addresses, or internal firewall details.

## CrowdSec CLI Summary

The shell-only `cscli decisions list -a -o json` output was flattened and summarized without publishing offender values.

| Metric | Value |
|---|---:|
| CAPI decision update records | 79 |
| Flattened decision rows | 3,776 |
| Unique decision values | 3,776 |
| Active-duration decision rows | 3,722 |
| Expired or negative-duration rows | 54 |
| Local alert rows | 0 |

### Decision Origin, Scope, and Type

| Field | Value |
|---|---|
| Origin | CAPI |
| Scope | IP |
| Action type | ban |
| Simulated | false |

### Scenario Breakdown

| Scenario | Count |
|---|---:|
| http:bruteforce | 2,431 |
| ssh:bruteforce | 937 |
| generic:scan | 205 |
| tcp:scan | 194 |
| ssh:exploit | 9 |

### Metrics Cross-Check

The `cscli metrics` local API decision table showed the same shape, with roughly 3.72k active CAPI ban decisions:

| Reason | Origin | Action | Count |
|---|---|---|---:|
| http:bruteforce | CAPI | ban | 2,424 |
| ssh:bruteforce | CAPI | ban | 891 |
| generic:scan | CAPI | ban | 205 |
| tcp:scan | CAPI | ban | 193 |
| ssh:exploit | CAPI | ban | 9 |

The small difference between flattened rows and active metrics is expected because `decisions list -a` includes rows whose durations have already crossed into negative time, while the metrics table reflects currently active decisions.

## Operational Interpretation

The telemetry supports the current design direction:

- Keep OPNsense as the enforcement point.
- Keep UPnP/NAT-PMP disabled unless a specific exception is justified.
- Keep publishing only sanitized summaries, not raw firewall exports.
- Add richer CrowdSec visibility later through a least-privilege, local-only collection path.
- Continue using the operations dashboard for status visibility and Grafana/Victoria-backed views for deeper metrics.

The CrowdSec deployment does not need more noisy features to be useful. Its strongest current role is lightweight edge reputation and behavior-based blocking: OPNsense enforces, CrowdSec supplies decisions, and the bouncer applies them at the firewall.

For a plain-English explanation of the setup, see [CrowdSec in Plain English](crowdsec-plain-english.md).

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
