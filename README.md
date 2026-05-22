# Home Network Security

<p align="center">
  <img src="./docs/assets/opnsense-home-network-security.svg" alt="OPNsense Home Network Security project banner" width="100%">
</p>

Sanitized documentation for a production-style home network security modernization project: OPNsense as the enforcement layer, Proxmox as the visibility/control-plane layer, and Homepage as the internal HomeNet Operations Dashboard. The project brings logs, metrics, inventory, deception, uptime checks, vulnerability/SBOM visibility, and recovery tracking into one practical operating model.

The network supports normal daily use: work, school, gaming, streaming, phones, access points, monitoring, and security learning. That changed the engineering standard. This is not treated as a disposable homelab. Stability, rollback, backup validation, low latency, and no lockouts are first-class requirements.

This public repository does not contain secrets, raw configuration exports, public IP addresses, MAC addresses, serial numbers, full host inventory, browser screenshots, API keys, passwords, tokens, private hostnames, or sensitive screenshots.

## At A Glance

- OPNsense remains the enforcement point for routing, firewall policy, DNS, DHCP, and edge security.
- Proxmox runs the lightweight visibility and control-plane services without sitting inline with network traffic.
- Homepage is the primary internal HomeNet Operations Dashboard.
- Homepage serves an operations status feed, Prometheus-style status metrics, and phase notes for internal widgets.
- Glance was tested and used earlier, then retired from active use after the Homepage dashboard migration.
- Grafana remains the deep metrics and dashboarding layer.
- Uptime Kuma remains the uptime and alerting view.
- NetBox is the source of truth for core inventory and planned segmentation.
- VictoriaMetrics and VictoriaLogs provide metrics and log backends.
- OpenCanary provides high-signal deception.
- NetAlertX provides local unknown-device awareness.
- Trivy and Syft provide vulnerability and SBOM visibility without automatic remediation.
- UPnP/NAT-PMP was disabled to reduce unnecessary exposure.
- Proxmox rpcbind was disabled after confirming NFS was not in use.
- Off-host backup/restore validation, remote access choice, endpoint pilot, and VLAN segmentation remain known gaps.
- WireGuard/Tailscale, endpoint agents, and VLAN migration were deferred until controlled testing paths are ready.
- No dashboard or admin console is exposed to the public internet.
- No raw Docker socket is mounted into the dashboard.
- OPNsense, Proxmox, and access point admin UIs are linked, not embedded.

## Current Architecture

```mermaid
flowchart LR
    Internet["Internet"] --> Gateway["ISP Gateway / Modem"]
    Gateway --> Firewall["OPNsense Firewall"]
    Firewall --> LAN["Trusted Production LAN"]
    LAN --> Proxmox["Proxmox Security Node"]
    LAN --> AP["Access Point Layer"]
    LAN --> Clients["Household Clients"]

    Proxmox --> Core["secops-core"]
    Proxmox --> Canary["canary host"]
    Proxmox --> Kuma["Uptime Kuma"]

    Core --> Homepage["HomeNet Operations Dashboard"]
    Core --> Grafana["Grafana"]
    Core --> Metrics["VictoriaMetrics"]
    Core --> Logs["VictoriaLogs"]
    Core --> NetBox["NetBox"]
    Core --> Discovery["NetAlertX"]
    Core --> Reports["Trivy / Syft Reports"]
    Core --> StatusFeed["Operations Status Feed"]
    Core --> MetricsFeed["Prometheus-style Status Metrics"]

    Firewall -- "firewall events" --> Logs
    Canary -- "canary events" --> Logs
    StatusFeed --> Homepage
    MetricsFeed --> Homepage
    MetricsFeed --> Grafana
```

## HomeNet Operations Dashboard

Homepage is now the daily front door. It is internal-only and designed to answer the first operational questions quickly:

- Is the network healthy?
- Is DNS working?
- Is the firewall reachable?
- Is Proxmox healthy?
- Are backups fresh?
- Is security quiet?

The dashboard serves sanitized local status, metrics, and notes feeds for widgets and operations review. Public docs describe the feed purpose rather than publishing live internal paths.

The dashboard includes Mission Status, Security Snapshot, Recovery Snapshot, observability links, inventory links, and documentation links. Privileged admin interfaces are not embedded in iframes.

See [docs/homepage-operations-dashboard.md](docs/homepage-operations-dashboard.md).

## Current Status

Active controls include OPNsense firewall/DNS enforcement, CrowdSec, Homepage, Uptime Kuma, NetBox, VictoriaMetrics, VictoriaLogs, NetAlertX, OpenCanary, Trivy/Syft reporting, and backup/freshness checks.

Inactive or deferred controls include full VLAN migration, AP VLAN trunking, remote access, broad endpoint telemetry, automatic remediation, broad vulnerability scanning, and heavy packet-capture/SIEM lab stacks in production.

Known gaps are documented directly: durable off-host backup and restore validation, remote access decision, one endpoint pilot, staged segmentation, and safer future API/widget integrations.

Intentionally not exposed: public dashboards, WAN admin consoles, raw Docker socket access, privileged admin iframes, secrets, raw configs, full inventories, and sensitive screenshots.

## Control Areas

| Area | Current Implementation | Public Evidence |
|---|---|---|
| Perimeter firewalling | OPNsense remains the policy enforcement point | Sanitized architecture and rule intent |
| DNS security | Firewall-managed DNS path with resolver hardening | DNS flow and operating model |
| Exposure reduction | UPnP/NAT-PMP disabled; inbound exposure avoided | Decision log |
| Control plane | Proxmox hosts visibility services, not inline enforcement | Control-plane design |
| Dashboard | Homepage primary; Glance retired | Dashboard documentation |
| Deep metrics | Grafana backed by metrics collection | Operations workflow |
| Uptime | Uptime Kuma monitors core services | Daily review workflow |
| Source of truth | NetBox tracks core assets and planned segmentation | Current-state snapshot |
| Logs | VictoriaLogs stores firewall and canary evidence | Control-plane design |
| Metrics | VictoriaMetrics supports time-series visibility | Dashboard roadmap |
| Asset awareness | NetAlertX tracks unknown-device signals | Operations workflow |
| Deception | OpenCanary provides high-signal alerts | Security controls |
| Supply-chain visibility | Trivy and Syft produce reports and SBOMs | Sprint narrative |
| Backups | Local backups, temporary off-host copy, restore-test workflow | Recovery status and roadmap |
| Remote access | Deferred because double NAT and testing path require planning | Decision log |
| Segmentation | VLANs documented but not migrated | Roadmap |

## Why This Matters

Home networks become hard to reason about when firewall policy, DNS behavior, asset awareness, logs, backups, and dashboard state are scattered. This project treats stability as part of security: visibility before enforcement, recovery before risky updates, and staged changes before broad rollout.

Not every security tool belongs in a daily production network. The public story is intentionally about judgment, not maximum tool count.

## Design Principles

### Production First

Changes are made like a small production network: back up first, change one thing at a time, verify, and keep rollback simple.

### Visibility Without Packet-Path Risk

The Proxmox node provides monitoring, logs, inventory, reports, and deception. It does not route traffic and should not break daily internet if it is offline.

### No Marketing-Driven Tool Sprawl

Tools are added only when they provide measurable defensive value. Heavy IDS/SIEM/full-packet-capture stacks are deferred unless there is hardware, maintenance time, and a clear detection goal.

### Document Without Publishing A Target Map

This repo explains the work without publishing live secrets, exact host maps, or sensitive operational data.

## What Is Not Published

- Public IP addresses.
- Raw OPNsense, Proxmox, NetBox, or dashboard configuration exports.
- API keys, passwords, tokens, private keys, recovery codes, or certificates.
- Exact private host maps or full inventory.
- MAC addresses, serial numbers, browser screenshots, or private hostnames.
- Live dashboard screenshots unless cropped and sanitized.

## Repository Structure

- `README.md`: project overview and public-facing case study.
- `docs/current-state.md`: current sanitized architecture and control-plane snapshot.
- `docs/modernization-sprint-2026-05-20.md`: modernization sprint narrative.
- `docs/decision-log.md`: architecture decision records.
- `docs/homepage-operations-dashboard.md`: Homepage Operations Dashboard architecture and rules.
- `docs/website-update-suggestions-2026-05-20.md`: suggested public website copy.
- `docs/portfolio-website-integration-2026-05-21.md`: notes on the live portfolio integration and sanitized website presentation.
- `docs/project-update-2026-05-20.md`: launch/update copy for GitHub, LinkedIn, resume, and case-study use.
- `docs/audit-report-2026-05-20.md`: final repo and website audit report.
- `docs/evidence-screenshots.md`: sanitized proof screenshots.
- `docs/roadmap.md`: current, next, and later work.
- `docs/operations.md`: daily and weekly operating workflow.
- `docs/proxmox-security-control-plane.md`: lightweight Proxmox control-plane case study.
- `docs/redaction-guide.md`: rules for safely sharing network security work.
- `SECURITY.md`: guidance for reporting security concerns about the repository.

## Status

Live production home-network project. Documentation is sanitized and reflects the Homepage dashboard migration and modernization sprint completed on 2026-05-20.

The public portfolio site now presents this project as **Home Network Security Control Plane** and links back to this repository from `https://srkyn.com/`. The website copy intentionally stays role-based and does not expose private dashboards, internal service URLs, raw feeds, exact inventories, or screenshots that reveal operational detail.
