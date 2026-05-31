# Home Network Security Live Threat Summary

**Date:** 2026-05-31
**Collection mode:** read-only, public-safe, partial authenticated telemetry
**Collection window:** point-in-time network checks, Proxmox API status, VictoriaLogs sample, and public GitHub workflow audit
**Environment:** OPNsense firewall, Proxmox control plane, security-service containers, Uptime Kuma, access point layer, canary host, VictoriaLogs, and public GitHub repositories

This summary documents live operational checks and a GitHub Actions workflow integrity audit. It is sanitized for public release. Internal addresses, hostnames, MAC addresses, secrets, raw configs, tokens, and credentials are not included.

No firewall rules, DNS settings, DHCP settings, Wi-Fi settings, VLANs, Proxmox settings, containers, or service configurations were modified during collection.

## Executive Summary

The core network path was functional during collection. Firewall reachability, Proxmox reachability, DNS resolution, external HTTPS, Uptime Kuma, the HomeNet Operations Dashboard, Grafana, and VictoriaLogs were reachable. Proxmox rpcbind was not reachable, matching the previous hardening goal.

Proxmox API authentication succeeded using a credential available locally, without printing or committing the credential. The Proxmox host had low CPU load, moderate memory use, healthy storage headroom, and four known containers. Three security-service containers were running and one speed-test container was stopped.

NetBox was not reachable on its usual service port during this point-in-time check. The access point answered DNS and SSH from the trusted admin side, which should be reviewed to confirm it is not competing with OPNsense for client DNS/DHCP authority.

The GitHub Actions integrity audit found no Megalodon indicators or suspicious workflow patterns in the requested public repositories.

## Data Source Status

| Phase | Data Source | Status | Records | Notes |
|---:|---|---|---:|---|
| 1 | OPNsense API | Not collected | 0 | A usable non-placeholder API secret was not available locally. |
| 1 | CrowdSec plugin/API | Not collected | 0 | No local API key was available. |
| 1 | DNS checks | Available | 3 | Firewall DNS, access point DNS, and external HTTPS checks passed. |
| 2 | Proxmox API | Available | 5 endpoints | Auth, node status, container inventory, storage, tasks, and version status collected. |
| 2 | Proxmox reachability | Available | 3 checks | HTTPS and SSH reachable; rpcbind closed. |
| 3 | VictoriaLogs | Available | 5,000 sampled events | Query path reachable; sample dominated by firewall/config service events. |
| 3 | OpenCanary reachability | Available | 3 checks | Fake service ports reachable as expected for a canary host. |
| 4 | GitHub Actions CI audit | Available | 7 workflow files | No suspicious patterns found. |
| 5 | Threat summary | Generated | 1 document | Public-safe summary only. |
| 6 | GitHub PR | Opened | 1 PR | Review before merge. |

## Network Health

Checks that passed:

- Firewall HTTPS reachability.
- Firewall DNS reachability.
- Proxmox management reachability.
- Proxmox SSH reachability from the trusted admin side.
- Uptime Kuma status page reachability.
- HomeNet Operations Dashboard reachability.
- Grafana reachability.
- VictoriaLogs reachability.
- DNS resolution through the firewall.
- DNS resolution through the access point.
- External HTTPS reachability.
- Canary fake service reachability.

Checks that failed or need review:

- NetBox service port was not reachable during this collection.
- Access point HTTPS was not reachable.
- Access point DNS was reachable and should be reviewed for AP-mode correctness.
- Firewall HTTP and SSH were reachable from the trusted admin side; acceptable for simple LAN administration, but still a future management-plane hardening item.

## Proxmox Control Plane

Proxmox API collection succeeded.

Host status at collection time:

- CPU usage: low single digits.
- Memory use: about 2.6 GB used of about 7.8 GB available.
- Uptime: collected successfully.
- Storage headroom: healthy.

Container status:

| Container Role | Status | Memory Use | Notes |
|---|---|---:|---|
| Uptime monitoring | Running | Low | Healthy. |
| Speed-test service | Stopped | None | Stopped service, likely intentional to save resources. |
| secops-core | Running | Moderate | Hosts dashboard, logs, metrics, and related services. |
| Canary host | Running | Low | Fake service surface available. |

Storage status:

| Storage Role | Used | Notes |
|---|---:|---|
| Thin-provisioned VM/container storage | About 6% | Strong headroom. |
| Local directory storage | About 29% | Healthy. |

Recent task history had non-OK task entries, mostly historical container locks, backup/shutdown warnings, and console proxy failures. These should be reviewed, but they do not by themselves prove an active compromise. The most important recurring pattern is previous container lock noise around the secops-core and canary containers.

## VictoriaLogs Sample

VictoriaLogs was reachable and returned a 5,000-event sample.

Summary of sampled events:

- Total sampled events: 5,000.
- Dominant source: firewall/config service events.
- Severity mix: mostly notice-level, with a smaller number of warnings and errors.
- Keyword hits included warning, canary, error, and failure-related terms.
- No CrowdSec-specific events were found in this sample.

This confirms the log path is alive, but the sample appears heavily weighted toward firewall/config service logs rather than a balanced security-event feed. Future tuning should ensure the dashboard can clearly separate:

- firewall events,
- CrowdSec events,
- canary events,
- service health events,
- backup/freshness events.

## Firewall Activity

Firewall logs and live firewall statistics were not collected through the OPNsense API because a usable API secret was not available locally. A local OPNsense API file existed, but the available secret material did not resolve to a usable non-placeholder API secret for this run.

Recommended next collection, once credentials are supplied securely through local environment variables:

- Firmware status.
- Firewall block summary.
- Interface traffic stats.
- DNS resolver stats.
- CrowdSec plugin status.

## CrowdSec IPS

CrowdSec alert and decision telemetry was not collected because no local API key was available and OPNsense API collection did not proceed.

Recommended next collection:

- Total alerts.
- Active decisions.
- Top scenarios.
- Recent ban activity.
- Bouncer health.

## OpenCanary Deception

The canary host responded on expected fake service ports. That confirms the deception surface is available from the trusted admin side.

VictoriaLogs contained canary-related keyword hits in the sample, but this summary does not publish raw event lines or source details. Routine monitoring should avoid repeatedly polling fake canary services; use host-level reachability for uptime checks and reserve fake-service touches for intentional alert-path tests.

## secops-core And Dashboard Services

The visibility stack was partially healthy during this collection:

- HomeNet Operations Dashboard: reachable.
- Grafana: reachable.
- VictoriaLogs: reachable.
- NetBox: not reachable on its usual port.

NetBox should be checked next from Proxmox or the secops-core container to determine whether it is stopped, bound differently, unhealthy, or intentionally offline.

## GitHub Actions CI Integrity Audit

The workflow audit checked the requested public repositories for Megalodon-related and generic suspicious CI/CD patterns.

Result:

- Repositories requested: 8.
- Workflow files audited: 7.
- Suspicious findings: 0.
- Megalodon IOC check: clean.

Patterns checked included suspicious shell execution, direct pipe-to-shell behavior, reverse-shell terms, tunnel services, suspicious exfiltration patterns, and reported Megalodon-style CI bot identities.

The supporting machine-readable audit result is stored in `docs/ci-audit-2026-05-31.json`.

## Detection Architecture

| Layer | Tool | Purpose |
|---|---|---|
| Perimeter enforcement | OPNsense | Stateful firewall, DNS, DHCP, traffic policy |
| Behavioral blocking | CrowdSec | Scenario-based detection and decisions |
| DNS security | Unbound/OPNsense | Resolver control and validation |
| Deception | OpenCanary | High-confidence anomaly detection |
| Log aggregation | VictoriaLogs | Central log storage and query |
| Metrics | VictoriaMetrics/Grafana | Time-series metrics and dashboards |
| Inventory | NetBox | Asset tracking and planned segmentation |
| Uptime | Uptime Kuma | Service availability monitoring |
| Control plane | Proxmox | Hosts supporting security services |

## Known Gaps

- NetBox was not reachable during collection.
- API-backed OPNsense and CrowdSec telemetry was not collected.
- VictoriaLogs sample showed no CrowdSec-specific events.
- Access point DNS is reachable and should be reviewed for AP-mode correctness.
- VLAN segmentation is documented but not yet deployed.
- Endpoint telemetry agents are not broadly deployed.
- Remote access remains deferred pending controlled testing.
- Durable off-host backup remains the most important resilience improvement.

## Methodology

Data was collected with read-only network checks, HTTP status checks, Proxmox API GET requests, VictoriaLogs read queries, and public GitHub repository workflow inspection. Public reporting on the May 2026 Megalodon campaign was used only to justify the CI workflow audit scope; no unverified incident details are treated as local findings.

No configuration changes were made.

## Sources For CI Audit Context

- [Cloud Security Alliance research note on Megalodon GitHub Actions CI/CD supply-chain activity](https://labs.cloudsecurityalliance.org/research/csa-research-note-megalodon-github-actions-cicd-supply-chain/)
- [TechRadar report on Megalodon GitHub repository compromise](https://www.techradar.com/pro/security/github-hit-with-another-major-attack-megalodon-hits-over-5-000-repos-with-malware-laden-commits)

## Next Human Actions

1. Review this summary before merging.
2. Check NetBox service health.
3. Supply OPNsense and CrowdSec API credentials through local environment variables only if a deeper firewall/IPS telemetry run is needed.
4. Review access point DNS behavior and confirm OPNsense remains the intended resolver authority.
5. Re-run the threat summary after NetBox and API-backed OPNsense/CrowdSec telemetry are available.
