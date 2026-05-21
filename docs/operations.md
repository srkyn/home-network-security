# Operations

This document captures recurring maintenance and review habits for the production home-network security project.

## Daily Dashboard Review

Start with the HomeNet Operations Dashboard. It should answer what needs attention without opening every tool.

Review:

- Mission Status: Internet, DNS, Firewall, Proxmox, Backups, Security.
- Security Snapshot: canary hits, known laptop failed-login watch, KEV matches, Trivy critical count, Trivy high count.
- Recovery Snapshot: Proxmox backup age, secops-core backup age, NetBox backup age, Trivy freshness, Syft freshness, off-host copy state, restore test state.
- Uptime Kuma for availability history and current monitor state.
- NetBox for inventory or planned-change context.
- VictoriaLogs when an event needs evidence.

If Mission Status is green except Backups, the expected next action is backup improvement, not emergency response.

## Weekly Review

- Review firewall log denies for repeated unexpected traffic.
- Review CrowdSec decisions and blocklist behavior.
- Confirm DNS services are healthy.
- Review Homepage Mission Status and snapshots.
- Review Uptime Kuma monitors.
- Review NetAlertX for unknown or newly seen devices.
- Review OpenCanary events and investigate any interaction with the fake host.
- Check Trivy/Syft freshness.
- Check backup freshness.
- Check the Proxmox failed-login watch.
- Confirm no temporary firewall rules were left open.
- Note any network changes worth documenting.

## Monthly Review

- Check OPNsense firmware and plugin updates.
- Check Proxmox and LXC package updates during a maintenance window.
- Export and protect a current OPNsense configuration backup.
- Confirm Proxmox and secops-core backups still exist.
- Confirm NetBox backup still succeeds.
- Review inbound NAT and WAN-facing rules.
- Review whether the single-LAN design still matches the risk model.
- Review CrowdSec agent/LAPI/firewall bouncer status.
- Confirm administrative accounts and access paths still make sense.

## Change Process

For meaningful changes, document:

- Date.
- Reason for change.
- Affected service or control.
- Expected behavior.
- Backup state.
- Rollback plan.
- Validation test.
- Review date.

Do not stack multiple risky changes before testing.

## Validation Tests

Use simple tests after changes:

- Confirm normal internet works.
- Confirm DNS resolution uses the intended resolver path.
- Confirm admin consoles are still reachable from the trusted admin path.
- Confirm Homepage loads.
- Confirm Uptime Kuma monitors are not falsely alarming.
- Confirm Grafana, NetBox, and logs still load.
- Confirm canary fake services are not being repeatedly polled by monitoring.
- After Proxmox updates, confirm containers autostart and the dashboard loads.

## Control Plane Daily Flow

1. Open HomeNet Operations Dashboard.
2. If Mission Status is degraded, inspect the relevant snapshot.
3. Use Uptime Kuma for availability history.
4. Use Grafana for deep metrics.
5. Use VictoriaLogs for evidence.
6. Use NetBox for inventory context.
7. Run Trivy/Syft only as scoped visibility tasks, not automatic remediation.

The control plane is not a traffic enforcement point. If the Proxmox node is down, OPNsense should continue routing the network.

## Gaming-Safe Operating Rules

- Do not enable IDS/IPS blocking without alert-only testing first.
- Do not schedule intrusive vulnerability scans by default.
- Keep scanning scoped to owned internal targets.
- Avoid heavy always-on tools unless resource headroom justifies them.
- Do not update containers until the backup/restore gate passes.

## Backup Handling

Firewall backups can contain secrets and network details. They should not be committed to this repository. Store them outside public source control and protect them with local encryption, access control, or an offline/off-host backup workflow.

The same applies to Proxmox backups, NetBox backups, Uptime Kuma databases, SSH keys, OPNsense `config.xml`, generated scanner reports, and dashboard environment files.

## Container Update Rule

Do not update all containers at once.

Required gate:

- Backup exists.
- Off-host copy exists or risk is explicitly accepted.
- Restore test is documented.
- Monitoring is green.
- Rollback command is written.
- One service is updated at a time.

## Incident Notes

When a suspicious event appears, capture:

- Timestamp.
- Alert or log source.
- Sanitized source and destination.
- Initial hypothesis.
- Action taken.
- Result.
- Follow-up needed.
