# Decision Log

This file uses an ADR-style format. It is sanitized for public sharing.

## ADR-001: Treat The Network As Production Home Network

- Status: Accepted.
- Context: The network supports daily work, school, gaming, streaming, phones, and security learning.
- Decision: Treat it as production, not a disposable homelab.
- Why: Stability, rollback, and access safety matter more than maximal tool deployment.
- Tradeoffs: Slower rollout for risky controls.
- Validation: Risky items such as VLAN migration, remote access, endpoint agents, and container updates were staged instead of rushed.
- Follow-up: Keep using stop points and backup gates.

## ADR-002: Keep OPNsense As Enforcement Point And Proxmox As Visibility/Control Plane

- Status: Accepted.
- Context: The Proxmox host runs on limited laptop hardware.
- Decision: OPNsense enforces policy; Proxmox provides visibility and recovery services.
- Why: The network should keep routing if the control plane is down.
- Tradeoffs: Some same-subnet controls require host firewalls or future VLANs.
- Validation: No traffic was routed through the Proxmox services.
- Follow-up: Add segmentation later with controlled testing.

## ADR-003: Disable UPnP/NAT-PMP

- Status: Accepted.
- Context: Automatic port mapping increases exposure.
- Decision: Disable UPnP/NAT-PMP.
- Why: Exposure reduction is worth the possible manual work for gaming or peer apps.
- Tradeoffs: Some games or consoles may need manual troubleshooting.
- Validation: Internet and core services remained available after the change.
- Follow-up: Revisit only if a specific application requires it.

## ADR-004: Disable Proxmox rpcbind

- Status: Accepted.
- Context: rpcbind was present, but NFS was not in use.
- Decision: Disable and mask rpcbind.
- Why: Remove unneeded listening service.
- Tradeoffs: NFS would need rpcbind restored if added later.
- Validation: Storage and containers continued working.
- Follow-up: Recheck before adding NFS storage.

## ADR-005: Add NetBox As Source Of Truth

- Status: Accepted.
- Context: DHCP reservations, planned VLANs, and core services needed documentation.
- Decision: Add NetBox for documentation only.
- Why: Source of truth should come before automation.
- Tradeoffs: Another service to back up.
- Validation: NetBox was installed, populated minimally, and backed up.
- Follow-up: Keep NetBox internal-only and do not let it push firewall changes yet.

## ADR-006: Use OpenCanary For High-Signal Deception

- Status: Accepted.
- Context: Normal home use should not touch fake internal services.
- Decision: Use OpenCanary as a fake internal host.
- Why: Canary interaction is rare and meaningful.
- Tradeoffs: Monitoring must avoid polling fake services.
- Validation: Canary alert path was tested.
- Follow-up: Keep canary monitoring to host ping unless intentionally testing.

## ADR-007: Use Trivy/Syft For Supply-Chain Visibility, Not Auto-Remediation

- Status: Accepted.
- Context: CVE counts can create noise.
- Decision: Generate reports and SBOMs, then triage.
- Why: Blind updates can break services and still not reduce practical risk.
- Tradeoffs: Requires manual review.
- Validation: Findings were triaged; no CISA KEV matches were found at the time.
- Follow-up: Update one service at a time behind backup/restore gates.

## ADR-008: Defer WireGuard

- Status: Accepted.
- Context: Upstream double NAT/private WAN made inbound testing uncertain.
- Decision: Defer WireGuard until upstream path and outside testing are ready.
- Why: Remote access must not expose admin consoles or break production.
- Tradeoffs: No self-hosted remote admin path yet.
- Validation: No WAN exposure was added.
- Follow-up: Choose OPNsense WireGuard, Tailscale, NetBird, or Headscale later.

## ADR-009: Defer Fleet/osquery

- Status: Accepted.
- Context: Endpoint telemetry was useful but not urgent enough to deploy broadly.
- Decision: Defer until a one-endpoint pilot is selected.
- Why: Avoid alert fatigue and unnecessary endpoint changes.
- Tradeoffs: Less endpoint visibility for now.
- Validation: No broad agents were deployed.
- Follow-up: Pilot exactly one endpoint first.

## ADR-010: Defer VLAN Migration

- Status: Accepted.
- Context: Full segmentation risks Wi-Fi, DHCP, DNS, and management access.
- Decision: Document VLANs first; do not migrate household devices yet.
- Why: Lockout risk is high without a controlled physical path.
- Tradeoffs: Flat LAN risk remains.
- Validation: No network access broke.
- Follow-up: Test one wired lab VLAN before Wi-Fi migration.

## ADR-011: Move From Glance To Homepage For Dashboard

- Status: Accepted.
- Context: The dashboard needed live status, recovery state, and security state.
- Decision: Promote Homepage as the primary dashboard and retire Glance from active use.
- Why: Homepage better matched the dashboard model.
- Tradeoffs: Slightly more configuration complexity.
- Validation: Homepage became primary; Glance backups and rollback notes were preserved.
- Follow-up: Continue improving widgets and public-safe documentation.

## ADR-012: Use Custom API Widgets/status.json Before Privileged API Integrations

- Status: Accepted.
- Context: API widgets can require credentials.
- Decision: Start with a local sanitized `status.json` feed.
- Why: It gives live status without exposing secrets.
- Tradeoffs: Less rich than native API widgets.
- Validation: Homepage consumed local status feeds.
- Follow-up: Add read-only widgets only where least privilege is practical.

## ADR-013: Do Not Mount Raw Docker Socket

- Status: Accepted.
- Context: Raw Docker socket access is effectively high privilege.
- Decision: Do not mount it into Homepage.
- Why: Dashboard compromise should not become container control.
- Tradeoffs: No automatic Docker discovery yet.
- Validation: Homepage runs without raw socket access.
- Follow-up: Consider docker-socket-proxy with write methods disabled.

## ADR-014: Link OPNsense/Proxmox/FreshTomato, Do Not Embed Them

- Status: Accepted.
- Context: Privileged admin UIs have their own security boundaries.
- Decision: Link admin consoles; do not iframe them or weaken clickjacking protections.
- Why: Embedding privileged panels adds risk and brittleness.
- Tradeoffs: One extra click to open admin consoles.
- Validation: Dashboard uses launch links, not privileged iframes.
- Follow-up: Keep this rule even if internal reverse proxy names are added.

## ADR-015: Require Backup/Restore Gate Before Container Updates

- Status: Accepted.
- Context: Container updates can break dashboards, logs, metrics, and monitoring.
- Decision: Do not update containers broadly until backup and restore checks are proven.
- Why: Recovery matters more than chasing every CVE immediately.
- Tradeoffs: Some patches wait for a maintenance window.
- Validation: Updates were planned one service at a time.
- Follow-up: Continue update order: low-state services first, stateful services after backups.
