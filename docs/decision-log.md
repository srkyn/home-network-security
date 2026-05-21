# Decision Log

This file uses an ADR-style format. It is sanitized for public sharing.

## ADR-001: Treat As Production Home Network, Not Disposable Homelab

- Status: Accepted.
- Context: The network supports daily work, school, gaming, streaming, phones, and security learning.
- Decision: Treat changes like production operations.
- Why: Stability, rollback, and access safety matter more than maximum tool count.
- Tradeoffs: Risky controls roll out more slowly.
- Validation: VLAN migration, remote access, endpoint agents, and broad container updates were staged instead of rushed.
- Follow-up: Keep using stop points and backup gates.

## ADR-002: Keep OPNsense As Enforcement Point

- Status: Accepted.
- Context: Routing, firewalling, DNS, DHCP, NAT, and edge controls belong in the network control point.
- Decision: Keep OPNsense as the system that enforces policy.
- Why: It keeps enforcement centralized and reviewable.
- Tradeoffs: Some visibility still requires external logging and dashboards.
- Validation: Normal traffic continues through OPNsense, not Proxmox.
- Follow-up: Keep firewall changes backed up and documented.

## ADR-003: Keep Proxmox As Visibility/Control Plane

- Status: Accepted.
- Context: The Proxmox node can host support services without sitting inline.
- Decision: Use Proxmox for dashboards, logs, metrics, inventory, canary signal, reports, and recovery checks.
- Why: Visibility should not become a packet-path dependency.
- Tradeoffs: Same-subnet controls remain limited until segmentation work.
- Validation: If Proxmox is unavailable, OPNsense should still route the network.
- Follow-up: Keep service backups and startup order documented.

## ADR-004: Disable UPnP/NAT-PMP

- Status: Accepted.
- Context: Automatic port mapping increases exposure.
- Decision: Disable UPnP/NAT-PMP.
- Why: Exposure reduction is worth the possible manual troubleshooting for gaming or peer apps.
- Tradeoffs: Some applications may require manual port review later.
- Validation: Internet and core services remained available after the change.
- Follow-up: Revisit only for a specific justified application.

## ADR-005: Disable rpcbind After Confirming NFS Was Not Used

- Status: Accepted.
- Context: rpcbind was present, but NFS was not in use.
- Decision: Disable and mask rpcbind.
- Why: Remove an unnecessary listening service.
- Tradeoffs: NFS would need rpcbind restored if added later.
- Validation: Storage and containers continued working.
- Follow-up: Recheck before adding NFS storage.

## ADR-006: Add NetBox As Source Of Truth

- Status: Accepted.
- Context: Core services, reservations, and planned segmentation needed one documented source.
- Decision: Add NetBox for inventory and planning, not firewall automation.
- Why: Source-of-truth documentation should come before automation.
- Tradeoffs: Another stateful service to back up.
- Validation: NetBox was installed, minimally populated, and backed up.
- Follow-up: Keep NetBox internal-only and avoid raw exports in public docs.

## ADR-007: Add OpenCanary/Lures For High-Signal Deception

- Status: Accepted.
- Context: Normal home use should not touch fake internal services.
- Decision: Use OpenCanary and simple lures as high-signal tripwires.
- Why: Canary interaction is rare and easier to reason about than noisy alert streams.
- Tradeoffs: Monitoring must avoid polling fake services.
- Validation: Canary alert path was tested.
- Follow-up: Do not publish raw lure paths or logs.

## ADR-008: Add Trivy/Syft For Visibility, Not Auto-Remediation

- Status: Accepted.
- Context: Vulnerability and SBOM visibility are useful, but blind updates can break services.
- Decision: Generate reports and SBOMs for triage.
- Why: Recovery and context matter more than reacting to every CVE count.
- Tradeoffs: Requires manual review.
- Validation: Reports were generated and KEV matching was reviewed.
- Follow-up: Update one service at a time behind backup/restore gates.

## ADR-009: Defer WireGuard/Tailscale Until Remote Access Path Is Tested

- Status: Accepted.
- Context: Remote access can expose management paths if rushed.
- Decision: Defer WireGuard/Tailscale/overlay decision until testing is clear.
- Why: No remote access is better than poorly understood remote access.
- Tradeoffs: No self-hosted remote admin path yet.
- Validation: No WAN exposure was added.
- Follow-up: Compare OPNsense WireGuard, Tailscale, NetBird, and Headscale later.

## ADR-010: Defer Endpoint Agents Until One Endpoint Pilot Is Selected

- Status: Accepted.
- Context: Broad endpoint telemetry can create noise and support burden.
- Decision: Defer Fleet/osquery or similar rollout until one endpoint pilot is chosen.
- Why: Learn on one endpoint before touching the household.
- Tradeoffs: Less endpoint visibility for now.
- Validation: No broad agents were deployed.
- Follow-up: Pilot exactly one endpoint.

## ADR-011: Defer VLAN Migration Until Physical Path And Rollback Are Confirmed

- Status: Accepted.
- Context: VLANs affect DHCP, DNS, Wi-Fi, admin access, and normal household use.
- Decision: Document VLAN goals but defer migration.
- Why: Segmentation is valuable, but lockout risk is real.
- Tradeoffs: Flat LAN risk remains.
- Validation: No access broke during the sprint.
- Follow-up: Test one wired VLAN before Wi-Fi migration.

## ADR-012: Replace Glance With Homepage Operations Dashboard

- Status: Accepted.
- Context: The dashboard needed live status, recovery state, and security state.
- Decision: Promote Homepage as the active operations dashboard and retire Glance from active service.
- Why: Homepage better matched the command-center model.
- Tradeoffs: Slightly more configuration complexity.
- Validation: Homepage became primary; Glance backups and rollback notes were preserved.
- Follow-up: Keep public docs clear that Glance is historical.

## ADR-013: Use Local Operations Status Feed Before Privileged API Widgets

- Status: Accepted.
- Context: API widgets can require tokens or privileged credentials.
- Decision: Start with a sanitized local operations status feed and Prometheus-style metrics.
- Why: Useful live status does not require broad credentials.
- Tradeoffs: Less rich than native API widgets.
- Validation: Homepage consumed local status data.
- Follow-up: Add read-only widgets only where least privilege is practical.

## ADR-014: Avoid Raw Docker Socket; Plan docker-socket-proxy Later

- Status: Accepted.
- Context: Raw Docker socket access is effectively administrative control over containers.
- Decision: Do not mount it into Homepage.
- Why: Dashboard compromise should not become container control.
- Tradeoffs: No automatic Docker discovery yet.
- Validation: Homepage runs without raw Docker socket access.
- Follow-up: Consider docker-socket-proxy with write methods disabled.

## ADR-015: Link OPNsense/Proxmox/FreshTomato Admin UIs Instead Of Embedding Them

- Status: Accepted.
- Context: Privileged admin UIs have their own authentication and browser protections.
- Decision: Link admin consoles; do not iframe them.
- Why: Embedding privileged panels adds risk and brittleness.
- Tradeoffs: One extra click to open admin consoles.
- Validation: Dashboard uses launch links, not privileged iframes.
- Follow-up: Keep this rule even if internal reverse proxy names are added.

## ADR-016: Require Backup/Restore Gate Before Container Updates

- Status: Accepted.
- Context: Container updates can break dashboards, logs, metrics, monitoring, and inventory.
- Decision: Do not update containers broadly until backups and restore checks are proven.
- Why: Recovery matters more than chasing every version immediately.
- Tradeoffs: Some updates wait for a maintenance window.
- Validation: Updates were planned one service at a time.
- Follow-up: Low-state services first; stateful services after backups.

## ADR-017: Use Temporary Laptop/Off-Host Copy Only As Interim Backup Step

- Status: Accepted.
- Context: A temporary off-host copy is better than local-only backups, but it is not a durable backup architecture.
- Decision: Use laptop/off-host copies only as an interim step if no SSD/NAS target exists.
- Why: The project needs recoverability without pretending a stopgap is complete.
- Tradeoffs: Temporary copies require manual discipline.
- Validation: Recovery state is tracked as a dashboard signal and roadmap gap.
- Follow-up: Add durable off-host backup and run restore validation.
