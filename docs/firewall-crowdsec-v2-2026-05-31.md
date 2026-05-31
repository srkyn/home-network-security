# Firewall and CrowdSec Telemetry v2

**Date:** 2026-05-31
**Collection mode:** read-only API probes plus temporary shell collection for CrowdSec CLI summaries
**Scope:** sanitized production telemetry

This document records which OPNsense and CrowdSec data paths returned usable data. Raw logs, offender addresses, private addresses, MAC addresses, credentials, and host identifiers are not published.

## Credential Handling

- OPNsense API credentials were present with key length 80 and secret length 80.
- Credential source used by the collector: local key file.
- CrowdSec LAPI key present in environment: no.
- Credential values were not printed or committed.
- A temporary SSH public key was used only for shell `cscli` collection and removed afterward; the key no longer authenticated after cleanup.

## Endpoint Probe Results

| Area | Method | Endpoint | HTTP | Shape | Records | Result |
|---|---|---|---:|---|---:|---|
| Firewall | GET | `/api/diagnostics/firewall/log` | 200 | list[1000] | 1000 | worked |
| Firewall | GET | `/api/diagnostics/firewall/log?limit=500` | 200 | list[500] | 500 | worked |
| Firewall | POST | `/api/diagnostics/firewall/log` with empty body | 200 | non-json | 0 | no data/error |
| Firewall | POST | `/api/diagnostics/firewall/log` with rowCount body | 200 | non-json | 0 | no data/error |
| Firewall | GET | `/api/diagnostics/firewall/stats` | 200 | list[2] | 2 | worked |
| Firewall | GET | `/api/diagnostics/firewall/pf_states` | 200 | dict | 0 | worked |
| Firewall | GET | `/api/diagnostics/traffic/interface` | 200 | dict | 0 | worked |
| CrowdSec | GET | `/api/crowdsec/service/decisions` | 404 | dict | 0 | no data/error |
| CrowdSec | GET | `/api/crowdsec/service/alerts` | 404 | dict | 0 | no data/error |
| CrowdSec | GET | `/api/crowdsec/service/stats` | 404 | dict | 0 | no data/error |
| CrowdSec | GET | `/api/crowdsec/service/status` | 200 | dict | 0 | worked |
| CrowdSec | GET | `/api/crowdsec/service/metrics` | 404 | dict | 0 | no data/error |
| CrowdSec | GET | `/api/crowdsec/service/listBouncers` | 404 | dict | 0 | no data/error |
| CrowdSec | GET | `/api/crowdsec/service/listMachines` | 404 | dict | 0 | no data/error |
| CrowdSec | GET | `/api/crowdsec/service/listScenarios` | 404 | dict | 0 | no data/error |
| CrowdSec | POST | `/api/crowdsec/service/run` | 404 | dict | 0 | no data/error |
| CrowdSec | GET | `/api/crowdsec/decisions/search` | 200 | dict.rows[0] | 0 | worked |
| CrowdSec | POST | `/api/crowdsec/decisions/search` | 200 | dict.rows[0] | 0 | worked |
| CrowdSec | GET | `/api/crowdsec/alerts/search` | 200 | dict.rows[0] | 0 | worked |
| CrowdSec | POST | `/api/crowdsec/alerts/search` | 200 | dict.rows[0] | 0 | worked |
| CrowdSec | GET | `/api/crowdsec/bouncers/search` | 200 | dict.rows[1] | 1 | worked |
| CrowdSec | POST | `/api/crowdsec/bouncers/search` | 200 | dict.rows[1] | 1 | worked |
| CrowdSec | GET | `/api/crowdsec/machines/search` | 200 | dict.rows[1] | 1 | worked |
| CrowdSec | POST | `/api/crowdsec/machines/search` | 200 | dict.rows[1] | 1 | worked |
| CrowdSec LAPI | GET | `/v1/decisions` | error | non-json | 0 | no data/error |
| CrowdSec LAPI | GET | `/v1/alerts` | error | non-json | 0 | no data/error |

## Firewall Numbers

- Recent firewall log sample: 500 entries.

| Action | Count |
|---|---:|
| pass | 449 |
| block | 51 |

| Protocol | Count |
|---|---:|
| tcp | 357 |
| icmp | 54 |
| igmp | 50 |
| udp | 39 |

| Blocked rule label | Count |
|---|---:|
| Block private networks from WAN | 50 |
| Default deny / state violation rule | 1 |

## CrowdSec API and GUI-Backed Tables

| Table | Total | Rows returned |
|---|---:|---:|
| decisions | 0 | 0 |
| alerts | 0 | 0 |
| bouncers | 1 | 1 |
| machines | 1 | 1 |

## CrowdSec CLI Summary

- Active `cscli decisions list -o json` rows: 0.
- `cscli alerts list -o json` rows: 0.
- `cscli bouncers list -o json` rows: 1.
- `cscli decisions list -a -o json` alert/update records: 79.
- Flattened all-decision rows: 3776.
- Unique decision values: 3776.

| Scenario | Count |
|---|---:|
| http:bruteforce | 2431 |
| ssh:bruteforce | 937 |
| generic:scan | 205 |
| tcp:scan | 194 |
| ssh:exploit | 9 |

### Metrics Cross-Check

| Reason | Origin | Action | Count |
|---|---|---|---:|
| generic:scan | CAPI | ban | 205 |
| http:bruteforce | CAPI | ban | 2424 |
| ssh:bruteforce | CAPI | ban | 891 |
| ssh:exploit | CAPI | ban | 9 |
| tcp:scan | CAPI | ban | 193 |

## What Worked

- `GET /api/diagnostics/firewall/log` returned `list[1000]` with 1000 records.
- `GET /api/diagnostics/firewall/log?limit=500` returned `list[500]` with 500 records.
- `GET /api/diagnostics/firewall/stats` returned `list[2]` with 2 records.
- `GET /api/diagnostics/firewall/pf_states` returned `dict` with 0 records.
- `GET /api/diagnostics/traffic/interface` returned `dict` with 0 records.
- `GET /api/crowdsec/service/status` returned `dict` with 0 records.
- `GET /api/crowdsec/decisions/search` returned `dict.rows[0]` with 0 records.
- `POST /api/crowdsec/decisions/search` returned `dict.rows[0]` with 0 records.
- `GET /api/crowdsec/alerts/search` returned `dict.rows[0]` with 0 records.
- `POST /api/crowdsec/alerts/search` returned `dict.rows[0]` with 0 records.
- `GET /api/crowdsec/bouncers/search` returned `dict.rows[1]` with 1 records.
- `POST /api/crowdsec/bouncers/search` returned `dict.rows[1]` with 1 records.
- `GET /api/crowdsec/machines/search` returned `dict.rows[1]` with 1 records.
- `POST /api/crowdsec/machines/search` returned `dict.rows[1]` with 1 records.
- Shell `cscli` collection worked through temporary SSH key access and produced real CrowdSec decision, alert, metrics, and bouncer summaries.

## What Did Not Work

- `POST /api/diagnostics/firewall/log` with an empty body returned HTTP `200` with shape `non-json`.
- `POST /api/diagnostics/firewall/log` with a rowCount body returned HTTP `200` with shape `non-json`.
- `GET /api/crowdsec/service/decisions` returned HTTP `404` with shape `dict`.
- `GET /api/crowdsec/service/alerts` returned HTTP `404` with shape `dict`.
- `GET /api/crowdsec/service/stats` returned HTTP `404` with shape `dict`.
- `GET /api/crowdsec/service/metrics` returned HTTP `404` with shape `dict`.
- `GET /api/crowdsec/service/listBouncers` returned HTTP `404` with shape `dict`.
- `GET /api/crowdsec/service/listMachines` returned HTTP `404` with shape `dict`.
- `GET /api/crowdsec/service/listScenarios` returned HTTP `404` with shape `dict`.
- `POST /api/crowdsec/service/run` returned HTTP `404` with shape `dict`.
- Direct CrowdSec LAPI `/v1/decisions` returned `error`; LAPI key used: no.
- Direct CrowdSec LAPI `/v1/alerts` returned `error`; LAPI key used: no.

## Manual OPNsense Shell Commands

If shell collection must be repeated manually, run these on the OPNsense shell and sanitize the output before publishing:

```sh
cscli decisions list -o json
cscli decisions list -a -o json
cscli alerts list -o json
cscli metrics
cscli bouncers list -o json
```

## Redaction Rules

- No private addresses are published.
- No MAC addresses are published.
- No raw offender values are published.
- No credential values are published.
- Raw API and CLI artifacts remain local and are not committed.
