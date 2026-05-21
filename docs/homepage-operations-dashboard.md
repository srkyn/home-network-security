# Homepage Operations Dashboard

## Purpose

Homepage is the primary internal HomeNet Operations Dashboard and Command Center for the production home network. It gives one fast view of mission health, security signals, recovery posture, documentation, and launch links without making the dashboard an enforcement point.

## Why Homepage Replaced Glance

Glance was the original lightweight dashboard and launchpad. It was useful early on, but the operating model grew beyond static links. Homepage replaced it because the network needed live Mission Status, Security Snapshot, Recovery Snapshot, local status feeds, cleaner grouping, and a better path to controlled read-only widgets.

Glance was retired from active service after the Homepage migration. Glance backups and rollback notes were preserved.

## Dashboard Sections

- Mission Status: Internet, DNS, Firewall, Proxmox, Backups, Security.
- Security Snapshot: canary hits, Proxmox failed-login watch, CISA KEV matches, Trivy Critical, Trivy High.
- Recovery Snapshot: Proxmox backup age, secops-core backup age, NetBox backup age, Trivy freshness, Syft freshness, off-host copy status, restore test status.
- Command Center: launch links for common operations.
- Observability: Grafana, VictoriaMetrics, VictoriaLogs, and Uptime Kuma paths.
- Inventory: NetBox and NetAlertX review paths.
- Launchpad: internal-only admin and service links.
- Documentation: phase notes, runbooks, and public-safe project docs.

## Internal Access Locations

These are RFC1918/private internal addresses used as examples for local runbooks. They are not public internet endpoints.

| Role | Internal example |
|---|---|
| HomeNet Operations Dashboard | `http://192.168.2.60:8080/` |
| Operations status feed | `http://192.168.2.60:8080/cockpit/status.json` |
| Prometheus-style status metrics | `http://192.168.2.60:8080/cockpit/home_network_status.prom` |
| Phase notes | `http://192.168.2.60:8080/cockpit/phase-notes.html` |

Public docs should describe these by role unless an internal RFC1918 example is needed. Avoid overusing the internal path name; call it the operations status feed.

## What Is Linked vs Embedded

Linked:

- OPNsense.
- Proxmox.
- FreshTomato access point admin.
- Grafana.
- NetBox.
- Uptime Kuma.
- VictoriaLogs/VictoriaMetrics.
- NetAlertX.
- Documentation and phase notes.

Not embedded:

- OPNsense admin UI.
- Proxmox admin UI.
- FreshTomato admin UI.
- Any privileged service that depends on its own authentication and clickjacking protections.

## What Is Intentionally Not Included

- WAN exposure.
- Raw Docker socket.
- API keys or tokens in dashboard YAML.
- Privileged admin iframes.
- Public status pages containing sensitive labels.
- Full internal host inventory.
- Raw status feeds or `.prom` output when labels reveal real hostnames.
- Raw firewall, Proxmox, NetBox, or backup exports.

## Security Model

- Internal-only dashboard.
- No public dashboard exposure.
- No raw Docker socket.
- No secrets in YAML.
- No privileged iframe embedding.
- Read-only or least-purpose API access only when a widget genuinely needs it.
- Public screenshots must be cropped or blurred before publishing.

## Future Widgets

- OPNsense read-only widget.
- Proxmox read-only widget.
- Grafana widget.
- Uptime Kuma internal status page widget.
- NetAlertX summary widget.
- CrowdSec summary widget.
- Docker status through docker-socket-proxy, not the raw Docker socket.

## Known Gaps

- Prometheus/VictoriaMetrics ingestion for `home_*` status metrics needs final wiring and dashboards.
- Read-only API tokens should remain scoped and documented; do not add privileged credentials casually.
- Durable off-host backup and restore validation remain the next critical recovery gap.
