# Project Update: 2026-05-20

## Short GitHub Project Update

Documented the 2026-05-20 modernization sprint for my daily home network security project. OPNsense remains the enforcement point, Proxmox remains the visibility/control-plane layer, and Homepage is now the internal HomeNet Operations Dashboard.

The update covers NetBox inventory, Uptime Kuma checks, Victoria logs/metrics, OpenCanary signal, Trivy/Syft visibility, backup/freshness tracking, and the controls intentionally deferred: VLAN migration, remote access, endpoint agents, aggressive scanners, auto-remediation, WAN exposure, and raw Docker socket access.

## Medium LinkedIn Post

I updated my public home network security project to reflect a recent modernization sprint.

This is my daily network, so I treated it less like a disposable lab and more like a small production environment: audit first, back up before changing, make one risky change at a time, and preserve rollback paths.

What changed:

- OPNsense stays the enforcement point.
- Proxmox stays the visibility/control-plane layer.
- Homepage replaced Glance as the HomeNet Operations Dashboard.
- NetBox now acts as the source of truth.
- Uptime Kuma, VictoriaMetrics, VictoriaLogs, NetAlertX, and OpenCanary support operations and security review.
- Trivy/Syft add vulnerability and SBOM visibility without automatic remediation.
- Backup/freshness checks now feed the recovery view.

What I intentionally did not do: expose dashboards to the WAN, mount the raw Docker socket, embed privileged admin UIs, rush VLAN migration, deploy endpoint agents broadly, or run aggressive scanners in production.

The useful part was not adding tools for the sake of it. It was improving visibility, recovery, and documentation while keeping the network stable.

## Longer Case-Study Intro

This case study documents a production-style home network security build. The starting point was a practical OPNsense firewall, a small Proxmox service node, a FreshTomato access point layer, and a flat LAN that supports normal household use. That daily-use constraint shaped the work: stability, rollback, latency, and avoiding lockouts mattered as much as adding controls.

The 2026-05-20 sprint focused on production-safe modernization. UPnP/NAT-PMP was disabled, an unnecessary rpcbind service was removed after dependency checks, DHCP reservations and documentation were cleaned up, Grafana memory was adjusted, NetBox was added as a source of truth, OpenCanary was tested, canary lures were added, Trivy/Syft reports were introduced, and backup/freshness scripts were created.

The biggest shift was the dashboard migration. Glance had been useful as a lightweight launchpad, but Homepage became the better fit for live operations. The HomeNet Operations Dashboard now centers Mission Status, Security Snapshot, and Recovery Snapshot while linking deeper tools like Uptime Kuma, NetBox, Grafana, VictoriaLogs, NetAlertX, and OpenCanary.

The project remains intentionally conservative. VLAN migration, remote access, endpoint telemetry, aggressive scanners, privileged API widgets, and container updates are staged behind testing and recovery gates.

## README Blurb

Production-style home network security documentation for OPNsense enforcement, Proxmox visibility/control-plane services, Homepage operations dashboard, logs/metrics, NetBox inventory, Uptime Kuma monitoring, OpenCanary deception, Trivy/Syft visibility, and backup/freshness tracking.

## Resume Bullet Version

- Modernized a daily-use home network security environment with OPNsense enforcement, Proxmox-hosted visibility services, Homepage operations dashboard, NetBox inventory, Uptime Kuma monitoring, Victoria logs/metrics, NetAlertX device awareness, OpenCanary deception, Trivy/Syft reporting, and backup/freshness tracking.
- Applied production-style change discipline by backing up before changes, disabling unnecessary exposure, validating service impact, preserving rollback paths, and deferring risky work such as VLAN migration, remote access, endpoint agents, broad scanners, and auto-remediation.
