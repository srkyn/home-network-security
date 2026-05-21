# Roadmap

This roadmap is intentionally conservative because the network is production, not disposable lab infrastructure.

## Now

- Fix any `NaN` or unknown values in dashboard widgets.
- Create or keep a Uptime Kuma internal status page slug for dashboard use.
- Use API widgets only with read-only or least-purpose credentials.
- Ingest `home_network_status.prom` into VictoriaMetrics/Grafana where practical.
- Add a temporary laptop off-host copy if no durable target exists, then replace it with a durable off-host backup target.
- Run and document restore tests before broad updates.
- Keep Homepage as the primary dashboard and Glance retired.

## Next

- Update blackbox-exporter one service at a time.
- Update cAdvisor one service at a time.
- Update Grafana after backup validation.
- Update VictoriaLogs after backup validation.
- Create a named Proxmox admin account and add MFA.
- Build the SSH key-only plan without locking out root until tested.
- Decide between OPNsense WireGuard, Tailscale, NetBird, or Headscale for remote access.
- Pilot Fleet/osquery on exactly one endpoint.
- Review NetBox update workflow through the official container stack.

## Later

- Run one wired VLAN test before touching Wi-Fi.
- Create a management VLAN.
- Create an IoT VLAN.
- Create a guest VLAN.
- Replace or supplement older access points with modern VLAN-capable APs.
- Add docker-socket-proxy for safe container status visibility.
- Add internal-only Caddy names.
- Consider Authelia or authentik only after core operations are stable.

## Research/Lab Only

- Security Onion.
- Arkime.
- T-Pot.
- Atomic Red Team.
- OpenCTI/MISP.
- Broad Nuclei scanning.
- Aggressive Greenbone scanning.

These may be useful learning tools, but they are not current production controls for the daily home network.

## Gates

Before risky changes:

- Current backup exists.
- Off-host copy exists.
- Restore test is documented.
- Rollback command is written.
- Monitoring is green.
- Only one major service changes at a time.
