# Home Network Security Threat Summary

**Generated:** 2026-05-31  
**Collection mode:** read-only, public-safe, limited telemetry  
**Environment:** OPNsense firewall, Proxmox control plane, Uptime Kuma, access point layer, canary host, and security-service containers

This summary was generated from safe reachability checks and public-safe service status checks. API-backed OPNsense and Proxmox telemetry was not collected because the required credentials were not present in the local environment at collection time. No configuration changes were made.

## Executive Summary

The core network path is functional: firewall reachability, DNS resolution, and external HTTPS all tested successfully. Proxmox management is reachable, and Uptime Kuma is reachable. The secops-core service stack was not reachable during this collection window, so the operations dashboard, Grafana, NetBox, and VictoriaLogs were unavailable from the admin machine.

The most important operational finding is not an attack signal. It is an availability/recovery signal: the visibility stack needs attention because the dashboard/logging/inventory services were unavailable while the network itself remained online.

## Data Source Status

| Data Source | Status | Records Collected | Notes |
|---|---:|---:|---|
| Firewall reachability | Available | 4 checks | HTTPS, HTTP, SSH, and DNS listeners were reachable from the admin LAN. |
| Proxmox reachability | Available | 3 checks | HTTPS and SSH were reachable; rpcbind was not reachable, which matches the hardening goal. |
| secops-core services | Unavailable | 4 checks | Operations dashboard, Grafana, NetBox, and VictoriaLogs were unreachable. |
| Uptime Kuma | Available | 1 HTTP status check | Internal status page responded successfully. |
| Access point layer | Available | 4 checks | HTTP, SSH, and DNS were reachable; HTTPS was not reachable. |
| Canary host | Available | 3 checks | Fake services were reachable as expected for a deception host. |
| OPNsense API | Not collected | 0 | Required environment variables were missing. |
| Proxmox API | Not collected | 0 | Required environment variables were missing. |
| VictoriaLogs | Not collected | 0 | secops-core was unreachable. |
| CrowdSec API | Not collected | 0 | Required local API key was missing. |

## Network Health

The following checks passed:

- DNS resolution through the firewall.
- DNS resolution through the access point.
- External HTTPS reachability.
- Firewall management-plane reachability from the trusted admin side.
- Proxmox management-plane reachability from the trusted admin side.
- Uptime Kuma internal status page reachability.

The following checks failed or were unavailable:

- secops-core operations dashboard.
- Grafana.
- NetBox.
- VictoriaLogs.
- Dashboard status and metrics feeds.

## Firewall Activity

Firewall log and live-stat telemetry was not collected because OPNsense API credentials were not available in the local environment.

Recommended next collection:

- Firmware status.
- Firewall block summary.
- Interface traffic stats.
- DNS resolver stats.
- CrowdSec plugin status.

## CrowdSec IPS

CrowdSec telemetry was not collected because no CrowdSec local API key was available and OPNsense plugin API access was not available.

Recommended next collection:

- Total alerts.
- Active decisions.
- Top scenarios.
- Recent ban activity.
- Bouncer health.

## OpenCanary Deception

The canary host responded on expected fake service ports. That confirms the deception surface is reachable from the admin LAN. Event volume was not collected because VictoriaLogs was unreachable.

Operational note: canary service reachability is expected, but routine monitoring should not repeatedly poll fake service ports. Use host-level reachability for uptime checks and reserve fake-service touches for intentional testing.

## Proxmox Control Plane

Proxmox HTTPS and SSH were reachable. rpcbind was not reachable, which matches the previous hardening decision to disable rpcbind when NFS is not in use.

Container inventory, task history, storage utilization, and node health were not collected because Proxmox API credentials were unavailable.

## secops-core Availability

secops-core services were unavailable from the admin machine during collection:

- HomeNet Operations Dashboard.
- Grafana.
- NetBox.
- VictoriaLogs.
- Dashboard status feed.
- Dashboard metrics feed.

This is the top operational issue from this run. Since Uptime Kuma is reachable, use it first to confirm whether the outage is already visible in monitoring history. Then inspect the Proxmox container state for the secops-core container.

## Access Point Observations

The access point answered HTTP, SSH, and DNS from the admin LAN. HTTPS was not reachable.

This is not automatically a critical issue, but it creates two review items:

- Confirm the access point is not competing with OPNsense for DHCP or DNS authority.
- Consider HTTPS-only or at least documented HTTP-only admin access later.

## Detection Architecture

| Layer | Tool | Role |
|---|---|---|
| Perimeter | OPNsense | Firewall, DNS path, traffic policy |
| Behavioral blocking | CrowdSec | Scenario-based detection and decisions |
| Deception | OpenCanary | High-confidence interaction alerts |
| Availability | Uptime Kuma | Service availability monitoring |
| Logs | VictoriaLogs | Central log query layer |
| Metrics | VictoriaMetrics/Grafana | Metrics and dashboards |
| Inventory | NetBox | Source of truth |
| Control plane | Proxmox | Hosts supporting services |

## Limitations

- No firewall logs were collected.
- No CrowdSec decisions were collected.
- No Proxmox container inventory was collected.
- No VictoriaLogs events were collected.
- No OpenCanary event history was collected.
- The visibility stack was unavailable, so this report focuses on reachability and operational status.

## Recommended Follow-Up

1. Restore or inspect secops-core.
2. Confirm Uptime Kuma shows the secops-core outage.
3. Confirm the operations dashboard, Grafana, NetBox, and VictoriaLogs return after secops-core is healthy.
4. Re-run API-backed collection after setting the required environment variables locally.
5. Verify the access point is not serving authoritative DNS/DHCP for clients.
6. Keep management interfaces internal-only until VLAN management segmentation is ready.

## Methodology

Data was collected using read-only network reachability checks and HTTP status checks from the admin machine. No configuration was modified, no service was restarted, and no firewall, DNS, DHCP, Wi-Fi, VLAN, or Proxmox setting was changed.

All internal addresses, hostnames, MAC addresses, secrets, and raw configuration details were omitted from this public document.
