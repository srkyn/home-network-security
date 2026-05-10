# Operations

This document captures recurring maintenance and review habits for the OPNsense home network security project.

## Weekly Review

- Review firewall log denies for repeated unexpected traffic.
- Review CrowdSec decisions/blocklist behavior.
- Confirm Unbound and Dnsmasq are healthy.
- Check the security dashboard for service health.
- Review NetAlertX for unknown or newly seen devices.
- Review OpenCanary events and investigate any interaction with the fake NAS.
- Check IDS/IPS alerts only after IDS/IPS is enabled.
- Confirm no temporary firewall rules were left open.
- Note any network changes worth documenting.

## Monthly Review

- Check OPNsense firmware and plugin updates.
- Check Proxmox and LXC package updates during a maintenance window.
- Export an encrypted/safely stored configuration backup.
- Confirm OPNsense git-backup still succeeds.
- Review inbound NAT and WAN-facing rules.
- Review whether the single-LAN design still matches the risk model.
- Review CrowdSec agent/LAPI/firewall bouncer status.
- Confirm administrative accounts and access paths still make sense.

## Change Process

For meaningful firewall changes, document:

- Date.
- Reason for change.
- Source zone.
- Destination zone.
- Service or port.
- Expected behavior.
- Rollback plan.
- Review date.

## Validation Tests

Use simple tests after rule or segmentation changes:

- From trusted LAN, confirm DNS resolution uses the intended resolver.
- From trusted LAN, confirm direct DNS bypass attempts are blocked when not pointed at the approved resolver.
- From outside the network, confirm firewall administration is not exposed.
- After IDS/IPS is enabled/tuned, confirm normal browsing and updates still work.
- After traffic shaping changes, confirm latency-sensitive traffic behaves as intended.
- After Proxmox updates, confirm LXCs autostart, the dashboard loads, logs ingest, and OpenCanary owns its fake service ports.
- After Uptime Kuma changes, confirm all monitors are green and false positives are explained.

## Control Plane Daily Flow

Start with the dashboard. It should answer the first operational question: what needs attention?

1. Check service status.
2. Open NetAlertX if there are unknown devices.
3. Open VictoriaLogs if a service or canary event needs evidence.
4. Use Uptime Kuma for availability history.
5. Run Nuclei, Trivy, or opnDossier only when there is a reason.

The control plane is not a traffic enforcement point. If the Proxmox node is down, OPNsense should continue routing the network.

## Gaming-Safe Operating Rules

- Do not enable IDS/IPS blocking without alert-only testing first.
- Do not schedule vulnerability scans by default.
- Keep scanning scoped to owned internal targets.
- Treat UPnP/NAT-PMP as a usability/security tradeoff for gaming devices.
- Avoid adding heavy always-on dashboards or databases unless resource headroom justifies them.

## Backup Handling

Firewall backups can contain secrets and network details. They should not be committed to this repository. Store them outside public source control and protect them with appropriate local encryption or access controls.

The same applies to Proxmox backups, Uptime Kuma databases, SSH keys, OPNsense `config.xml`, and generated scanner reports if they include internal details.

## Incident Notes

When a suspicious event appears, capture:

- Timestamp.
- Alert or log source.
- Source and destination, sanitized if shared publicly.
- Initial hypothesis.
- Action taken.
- Result.
- Follow-up needed.
