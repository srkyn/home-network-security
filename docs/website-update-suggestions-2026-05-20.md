# Website Update Suggestions: 2026-05-20

Status update on 2026-05-21: the live portfolio site has been updated with the recommended direction. The Network project is now presented as **Home Network Security Control Plane** and the copy references Homepage Operations Dashboard, NetBox, Uptime Kuma, Victoria, NetAlertX, OpenCanary, Trivy/Syft, backup/freshness tracking, recovery status, and known gaps without publishing private operational details.

## Current Website Assessment

The live portfolio site currently presents the network project as "OPNsense + Proxmox Security Control Plane." The card accurately emphasizes OPNsense, DNSSEC, Quad9 DNS-over-TLS, CrowdSec, VictoriaLogs, NetAlertX, OpenCanary, and Proxmox as a visibility/control-plane layer.

It does not yet reflect the 2026-05-20 modernization sprint, Homepage Operations Dashboard migration, live operations status feed, Security Snapshot, Recovery Snapshot, NetBox source-of-truth role, Trivy/Syft visibility, or backup/freshness tracking.

## What Is Outdated

| Current wording | Issue | Replacement wording |
|---|---|---|
| "OPNsense + Proxmox Security Control Plane" | Still accurate, but misses the new dashboard/recovery operating model. | "Home Network Security Control Plane" |
| "logs, canary alerts, asset awareness, and safe on-demand checks" | Omits Homepage, status feeds, NetBox, Trivy/Syft, and recovery tracking. | "logs, metrics, inventory, canary alerts, vulnerability/SBOM visibility, and recovery-status tracking" |
| "controls: OPNsense, DNSSEC, Quad9 DoT, CrowdSec, VictoriaLogs, NetAlertX, OpenCanary" | Missing current tools. | "controls: OPNsense, DNSSEC, Quad9 DoT, CrowdSec, Homepage, NetBox, Uptime Kuma, Victoria, NetAlertX, OpenCanary, Trivy/Syft" |
| "Next action Keep enforcement on OPNsense, use Proxmox for visibility, and run scanners only on demand." | Good but incomplete after the sprint. | "Next action Add off-host backup and restore validation, then continue read-only widgets, one-service updates, and staged segmentation." |

## What Should Be Added

- Homepage-based HomeNet Operations Dashboard.
- Operations status feed and Prometheus-style status metrics.
- Mission, Security, and Recovery snapshots.
- NetBox as source of truth.
- Uptime Kuma monitor/status view.
- Grafana/VictoriaMetrics/VictoriaLogs stack.
- NetAlertX, OpenCanary, Trivy, and Syft roles.
- Backup/freshness scripts and restore-test gate.
- Glance retired after the Homepage migration.
- Explicit safety posture: no WAN exposure, no raw Docker socket, no privileged admin iframes.
- Known gaps: off-host backup/restore, remote access decision, endpoint pilot, VLAN migration.

## Revised Project Card Copy

Title: Home Network Security Control Plane

Description:

A production-style home network security build using OPNsense as the enforcement layer and Proxmox as a lightweight visibility/control plane. The project now includes a Homepage-based operations dashboard, NetBox inventory, uptime checks, logs/metrics, canary alerts, Trivy/Syft visibility, and recovery-status tracking.

Tags:

Network, OPNsense, Proxmox, Homepage, NetBox, Uptime Kuma, Victoria, OpenCanary, Trivy/Syft

Read note:

Problem: Home networks become difficult to trust when firewall policy, DNS behavior, asset awareness, logs, backups, and dashboard state are scattered.

Checked: OPNsense firewall/DNS path, CrowdSec, Proxmox service health, NetBox inventory, Uptime Kuma monitors, Victoria logs/metrics, NetAlertX device awareness, OpenCanary signal, Trivy/Syft output, and backup/freshness status.

Decision: Keep OPNsense as enforcement. Use Proxmox for visibility. Use Homepage as the operations dashboard. Stage risky changes like VLANs, remote access, endpoint agents, and API tokens instead of rushing them.

Next action: Add off-host backup and restore validation, then continue with read-only widgets, one-service container updates, and staged segmentation.

Limit: Public notes avoid raw configs, secrets, exact inventories, public IPs, and sensitive screenshots.

## One-Line Website Summary

Production-style home network security project with OPNsense enforcement, Proxmox visibility, Homepage operations dashboard, inventory, monitoring, deception, vulnerability/SBOM visibility, and recovery tracking.

## Short Website Blurb

I maintain my daily home network like a small production environment: OPNsense enforces policy, Proxmox hosts visibility services, and Homepage provides a local operations dashboard for mission, security, and recovery state. The public writeup focuses on engineering decisions and redaction discipline rather than publishing raw configs or targetable details.

## Medium Website Case-Study Copy

This project documents a daily-use home network security build, not a disposable lab. OPNsense remains the enforcement point for firewalling, DNS, DHCP, and edge controls. A small Proxmox node hosts the visibility/control-plane layer: Homepage, Uptime Kuma, NetBox, Grafana, VictoriaMetrics, VictoriaLogs, NetAlertX, OpenCanary, Trivy, Syft, and backup/freshness scripts.

The 2026-05-20 modernization sprint focused on safe, recoverable improvements: UPnP/NAT-PMP was disabled, an unneeded rpcbind service was removed after NFS checks, NetBox was added as source of truth, OpenCanary was tested, Trivy/Syft reports were added, and Homepage replaced Glance as the active HomeNet Operations Dashboard.

The design intentionally avoids WAN dashboard exposure, raw Docker socket access, privileged admin iframes, broad endpoint agents, rushed VLAN migration, aggressive scanners, and automatic remediation. The next milestone is durable off-host backup with restore validation, followed by least-privilege read-only widgets and staged segmentation.

## Long Case-Study Section

Home networks become hard to reason about when firewall rules, DNS behavior, device awareness, logs, backups, and dashboard state live in separate places. This project is my attempt to make that environment observable and recoverable without overbuilding it.

OPNsense is the enforcement layer. Proxmox is the visibility/control-plane layer. Homepage is the daily HomeNet Operations Dashboard, showing Mission Status, Security Snapshot, and Recovery Snapshot before I open deeper tools. NetBox tracks core inventory and planned segmentation. Uptime Kuma watches availability. VictoriaMetrics and VictoriaLogs provide metrics and logs. NetAlertX watches unknown-device signals. OpenCanary creates a high-signal deception path. Trivy and Syft provide vulnerability and SBOM visibility without automatic remediation.

The interesting part is what I chose not to do. I did not expose dashboards to the WAN, mount the raw Docker socket, embed privileged admin consoles, roll out endpoint agents broadly, migrate VLANs casually, or run heavy lab stacks in production. The project is intentionally staged: backup before change, one change at a time, visibility before enforcement, and recovery before risky updates.

## Suggested Tags

- Network Security
- OPNsense
- Proxmox
- Homepage
- HomeNet Operations Dashboard
- NetBox
- Uptime Kuma
- Grafana
- VictoriaMetrics
- VictoriaLogs
- NetAlertX
- OpenCanary
- Trivy
- Syft
- Backup/Recovery

## Suggested Screenshot/Diagram Guidance

- Prefer a sanitized architecture diagram over live screenshots.
- If using a dashboard screenshot, crop browser chrome and blur internal labels, hostnames, IPs, usernames, tabs, bookmarks, and status details that create a target map.
- Show the dashboard structure rather than exact live data.
- Use role labels such as firewall, Proxmox node, access point, dashboard, inventory, metrics, logs, canary, and backup status.

## Sanitized Diagram Guidance

Use a simple flow: Internet -> OPNsense enforcement -> production LAN -> Proxmox visibility/control plane -> Homepage, NetBox, Uptime Kuma, Victoria stack, NetAlertX, OpenCanary, Trivy/Syft, backup/freshness scripts. Do not publish a full internal host inventory.

## Redaction Warnings

- Do not publish raw config exports, secrets, tokens, API keys, private keys, certificates, public WAN IPs, MAC addresses, serial numbers, or full internal inventories.
- Do not publish raw status feeds if they expose sensitive identifiers.
- Do not publish `.prom` labels if they reveal real hostnames or topology.
- Do not publish Uptime Kuma push URLs.
- Do not publish unredacted dashboard screenshots.
