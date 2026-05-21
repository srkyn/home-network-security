# Modernization Sprint: 2026-05-20

This document is a sanitized narrative of the production home-network modernization sprint. It intentionally avoids secrets, raw configurations, exact host maps, public IP information, MAC addresses, tokens, and screenshots.

## Starting Point

The network began as a practical home security build around OPNsense and a small Proxmox security-services node. The early control plane focused on lightweight visibility: uptime checks, logs, asset awareness, canary services, and manual scanners.

The major issue was not lack of tools. It was operational maturity: backups, rollback, freshness checks, documented decisions, and a dashboard that reflected the real current state.

## Why The Project Shifted From Homelab Framing To Production Home-Network Framing

The network supports daily life: work, school, phones, streaming, gaming, monitoring, and security learning. That means breakage has real consequences.

The operating model changed accordingly:

- Backups before changes.
- One risky change at a time.
- Explicit stop points.
- No full VLAN migration during a casual session.
- No WAN exposure for dashboards or admin consoles.
- No broad endpoint-agent rollout.
- No auto-remediation of CVEs.

## Initial Audit Findings

The initial audit found a strong base:

- OPNsense was current and healthy.
- Proxmox was current and healthy.
- The access point was acting correctly as an AP.
- DNS and CrowdSec were already useful controls.

The main concerns were:

- UPnP/NAT-PMP exposure.
- Proxmox rpcbind running without an NFS need.
- A flat LAN with no segmentation yet.
- DHCP reservation cleanup.
- Grafana memory pressure.
- Lack of durable off-host backups.
- A dashboard stack that no longer matched the real target architecture.

## Same-Day Sprint

The first sprint focused on production-safe wins:

- Exported OPNsense configuration.
- Backed up Proxmox configuration.
- Backed up secops-core.
- Disabled UPnP/NAT-PMP.
- Disabled Proxmox rpcbind after checking NFS dependency.
- Added DHCP reservations for core services.
- Raised Grafana memory.
- Tested OpenCanary.
- Added canary lure files.
- Ran Trivy and Syft.
- Installed and minimally populated NetBox.

WireGuard, endpoint agents, and VLAN migration were skipped because they required more controlled planning.

## Post-Sprint Phase 2

Phase 2 turned the sprint into an operational program:

- Hashed local backups.
- Confirmed local backups were useful but not sufficient.
- Hardened NetBox credential-file permissions.
- Triaged Trivy findings.
- Confirmed no CISA KEV matches.
- Confirmed upstream double NAT/private WAN situation.
- Flagged Proxmox root SSH and Proxmox firewall policy as staged hardening items.
- Found repeated Proxmox failed logins from a local source.

## Control-Plane Hardening Phase 3

Phase 3 investigated the local failed-login source. It was identified as the user laptop, not an unknown intruder. The likely cause was a stale browser session, saved credential, or polling behavior.

Phase 3 also:

- Created a Proxmox failed-login watch helper.
- Created tested NetBox backups.
- Kept containment changes staged instead of blocking a known user device.

## Operational Resilience Phase 4

Phase 4 emphasized recovery:

- Created a NetBox manual backup script.
- Confirmed laptop-generated Proxmox failures stopped.
- Confirmed no durable off-host backup target existed.
- Created a blackbox-exporter update plan but did not execute it.

The update was intentionally gated behind backup and monitoring readiness.

## Monitoring/Backup Phase 5

Phase 5 created freshness checks and backup templates:

- Backup freshness scripts.
- Trivy/Syft freshness scripts.
- Proxmox failed-login watch scripts.
- restic backup and restore templates.

Container updates were deferred because the off-host backup and restore gate had not passed.

## HomeNet Operations Dashboard Migration

Homepage was tested as a replacement for the older Glance front door. It became the better fit because the goal had shifted from a simple launchpad to a dashboard with live operational state, recovery state, and security state.

The migration:

- Promoted Homepage to the primary internal dashboard.
- Retired Glance from active use.
- Preserved Glance backups and rollback notes.
- Added local dashboard feeds:
  - `/dashboard/status.json`
  - `/dashboard/home_network_status.prom`
  - `/dashboard/phase-notes.html`

## Live Widget Pack

The dashboard gained live status areas:

- Mission Status for Internet, DNS, Firewall, Proxmox, Backups, and Security.
- Security Snapshot for canary hits, known laptop failed-login watch, KEV matches, and Trivy severity counts.
- Recovery Snapshot for backup ages, report freshness, off-host copy state, and restore test state.
- Uptime Kuma status page integration.
- Read-only or least-purpose integrations where appropriate.

Raw Docker socket access was not added. Privileged admin UIs were not embedded.

## What Changed

- Dashboard front door changed from Glance to Homepage.
- UPnP/NAT-PMP was disabled.
- Proxmox rpcbind was disabled.
- NetBox became the source of truth.
- Backup and freshness checks were created.
- OpenCanary was tested and made more operationally visible.
- Trivy/Syft reports became part of the recurring workflow.
- Restore testing became a gate before broad updates.

## What Did Not Change

- No WAN dashboard exposure.
- No full VLAN migration.
- No Proxmox firewall lockdown.
- No SSH/root policy change without a staged plan.
- No broad endpoint-agent deployment.
- No public reverse proxy.
- No raw Docker socket in Homepage.
- No privileged admin iframes.

## Why Each Change Mattered

- Disabling UPnP/NAT-PMP reduced automatic inbound exposure.
- Disabling rpcbind reduced unnecessary listening services.
- NetBox created a source of truth before automation.
- Backups and restore tests made changes recoverable.
- OpenCanary created high-signal detection.
- Trivy/Syft added supply-chain visibility without panic updating.
- Homepage turned scattered tools into one daily dashboard.

## Risks Reduced

- Accidental exposure through automatic port mapping.
- Unneeded local service exposure.
- Undocumented drift.
- Fragile dashboard sprawl.
- Unreviewed CVE noise.
- Recovery uncertainty.
- Misclassification of a known laptop as an unknown attacker.

## Risks Remaining

- Durable off-host backup target still needs to be implemented.
- Segmentation still needs staged testing.
- Remote access still needs a controlled decision.
- Endpoint telemetry still needs a one-device pilot.
- Proxmox admin hardening is staged but not complete.
- Container updates must continue one service at a time.

## Lessons Learned

- Treating a home network like production changes the order of operations.
- Backup and rollback work is not optional overhead; it is what makes safe modernization possible.
- A dashboard should reduce decisions, not become another noisy system.
- High-signal controls are more valuable than heavy tools with unclear maintenance burden.
- Public documentation can show engineering discipline without publishing a target map.
