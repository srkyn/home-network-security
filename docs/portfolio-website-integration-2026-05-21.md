# Portfolio Website Integration - 2026-05-21

## Summary

The public portfolio site at `https://srkyn.com/` now reflects the current Home Network Security project state without exposing sensitive operational detail.

The website presents the project as:

**Home Network Security Control Plane**

## What The Website Now Shows

- OPNsense as the enforcement layer.
- Proxmox/secops-core as the lightweight visibility and control-plane layer.
- Homepage as the internal HomeNet Operations Dashboard.
- NetBox inventory/source-of-truth role.
- Uptime Kuma uptime checks.
- VictoriaMetrics and VictoriaLogs for metrics/log visibility.
- NetAlertX for unknown-device awareness.
- OpenCanary for high-signal canary events.
- Trivy/Syft for vulnerability and SBOM visibility.
- Recovery-status tracking and backup/freshness state.
- Honest known gaps: off-host backup/restore validation, remote access decision, endpoint pilot, and staged VLAN segmentation.

## What The Website Does Not Publish

- Internal dashboard URLs.
- Raw operations status feeds.
- Prometheus label output.
- Exact firewall rules or sensitive routes.
- Full host inventories.
- Public WAN IPs.
- MAC addresses or serial numbers.
- Tokens, API keys, passwords, or private keys.
- Unredacted screenshots.

## Website UX Updates Relevant To This Project

- Project search/filtering exposes the Network project by domain and tool terms.
- The command menu can jump directly to the Network project.
- The Network card remains visually emphasized without using overhyped SOC/war-room language.
- `/now/` summarizes current portfolio and network-security focus.
- `/changelog/` records the 2026-05-20 and 2026-05-21 public updates.

## Source Of Truth

- Public project repository: `https://github.com/srkyn/home-network-security`
- Public portfolio: `https://srkyn.com/`
- Website source repository: private `srkyn/srkyn-com`

The website should stay a recruiter-friendly summary. This repository remains the deeper public-safe technical record.

## Maintenance Notes

When this repository changes materially, update the website only with sanitized, role-based copy. Do not add live screenshots or private feed output unless they are cropped, blurred, and reviewed against `docs/redaction-guide.md`.
