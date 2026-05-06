# Operations

This document captures recurring maintenance and review habits for the OPNsense home network security project.

## Weekly Review

- Review firewall log denies for repeated unexpected traffic.
- Review CrowdSec decisions/blocklist behavior.
- Confirm Unbound and Dnsmasq are healthy.
- Check IDS/IPS alerts only after IDS/IPS is enabled.
- Confirm no temporary firewall rules were left open.
- Note any network changes worth documenting.

## Monthly Review

- Check OPNsense firmware and plugin updates.
- Export an encrypted/safely stored configuration backup.
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

## Backup Handling

Firewall backups can contain secrets and network details. They should not be committed to this repository. Store them outside public source control and protect them with appropriate local encryption or access controls.

## Incident Notes

When a suspicious event appears, capture:

- Timestamp.
- Alert or log source.
- Source and destination, sanitized if shared publicly.
- Initial hypothesis.
- Action taken.
- Result.
- Follow-up needed.
