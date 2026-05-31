# Results

This page summarizes the strongest public-safe outcomes from the home network security modernization work.

## Key Outcomes

- OPNsense remains the enforcement point for routing, firewall policy, DNS, DHCP, and edge security.
- UPnP/NAT-PMP was disabled to reduce unnecessary automatic inbound exposure.
- Proxmox rpcbind was disabled after confirming NFS was not in use.
- Homepage replaced Glance as the internal HomeNet Operations Dashboard.
- NetBox was added as a source of truth for core inventory and planned segmentation.
- Uptime Kuma, Grafana, VictoriaMetrics, VictoriaLogs, OpenCanary, NetAlertX, Trivy, and Syft were organized into a practical operating model.
- CrowdSec was validated through OPNsense API checks, GUI verification, and shell `cscli` summaries.
- CrowdSec CAPI/community decisions were confirmed, with roughly 3.7k active ban decisions during the May 2026 check.
- Firewall telemetry was collected and published only as sanitized aggregate counts.
- GitHub Actions workflow integrity was checked and published as a sanitized CI audit.
- No dashboard, firewall admin UI, Proxmox UI, access point UI, or monitoring service was exposed to the public internet.
- No raw Docker socket was mounted into the dashboard.
- Raw configs, secrets, internal addresses, MAC addresses, offender values, and full topology details were kept out of public documentation.

## Remaining Gaps

- Durable off-host backup and restore validation remain the highest recovery priority.
- Remote access is still deferred pending a controlled WireGuard or mesh VPN test.
- Endpoint telemetry should start with one pilot endpoint, not a broad rollout.
- VLAN migration remains planned, but household Wi-Fi and management access should not be moved without a maintenance window.
- Future dashboard API widgets should use read-only, least-purpose credentials only when they add real operational value.

## Why It Matters

The project improved visibility and reduced unnecessary exposure without turning a daily home network into a fragile lab. The strongest result is the operating discipline: back up first, change one thing at a time, verify impact, document rollback, and publish only sanitized evidence.
