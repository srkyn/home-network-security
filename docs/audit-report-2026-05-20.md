# Audit Report: 2026-05-20

## Executive Summary

The repository already described the OPNsense and Proxmox security-control-plane work, but it needed a tighter update for the 2026-05-20 modernization sprint, Homepage Operations Dashboard migration, live status/recovery feeds, known gaps, and public website copy.

The live website still presents the project as "OPNsense + Proxmox Security Control Plane" and does not yet include Homepage, live operations status, NetBox, Trivy/Syft, recovery tracking, or the Glance retirement.

## Repo Files Reviewed

| File | Purpose | Current? | Needs update? | Risk? | Notes |
|---|---|---:|---:|---:|---|
| `README.md` | Public project overview | Mostly | Yes | Low | Updated for Homepage Operations Dashboard, feeds, known gaps, and safety posture. |
| `SECURITY.md` | Sensitive-info reporting policy | Yes | No | Low | Already warns against configs, credentials, and live network testing. |
| `LINKEDIN.md` | LinkedIn copy | Partial | Later | Low | Mentions Homepage but older first section still OPNsense-only; new `project-update` doc supersedes it. |
| `docs/current-state.md` | Current architecture snapshot | Partial | Yes | Low | Fixed dashboard heading and adjusted restore/off-host status to avoid overclaiming. |
| `docs/design-rationale.md` | Decision rationale | Partial | Yes | Low | Added production framing, UPnP/NAT-PMP, rpcbind, status feeds, Docker socket, admin UI, backup, and VLAN rationale. |
| `docs/operations.md` | Daily/weekly operations | Partial | Yes | Low | Added Homepage-first flow, weekly checks, and explicit change gates. |
| `docs/proxmox-security-control-plane.md` | Proxmox control-plane case study | Mostly | Minor | Low | Already reflects Homepage and Glance retirement. |
| `docs/homepage-dashboard.md` | Older dashboard doc | Partial | Yes | Low | Retained as compatibility pointer; canonical doc added. |
| `docs/homepage-operations-dashboard.md` | Canonical dashboard doc | New | No | Low | Created with internal-only access notes and redaction guidance. |
| `docs/modernization-sprint-2026-05-20.md` | Sprint narrative | Partial | Yes | Low | Added required executive summary, phase naming, posture, and next actions. |
| `docs/decision-log.md` | ADRs | Partial | Yes | Low | Rebuilt as ADR-001 through ADR-017. |
| `docs/roadmap.md` | Planned work | Partial | Yes | Low | Added current/next/later/research grouping and conservative gates. |
| `docs/redaction-guide.md` | Public redaction rules | Partial | Yes | Low | Added dashboard, feed, `.prom`, Uptime Kuma, NetBox, Trivy/Syft warnings. |
| `docs/website-update-suggestions-2026-05-20.md` | Website copy | New | No | Low | Created updated card, blurbs, tags, diagram and screenshot guidance. |
| `docs/project-update-2026-05-20.md` | Launch/update copy | New | No | Low | Created GitHub, LinkedIn, case-study, README, and resume copy. |
| `docs/assets/*` | Public images/diagrams | Mostly | Review before reuse | Medium | Existing screenshot names are marked sanitized; do not add raw dashboard screenshots. |
| `docs/architecture.md` | Older architecture note | Partial | Later | Low | Still useful but less current than README/current-state/control-plane docs. |
| `docs/evidence-screenshots.md` | Screenshot evidence index | Not deeply changed | Review before publish | Medium | Keep screenshot crop/blur checks strict. |

## Website Reviewed

Reviewed `https://srkyn.com` on 2026-05-21. The Network card currently says:

> OPNsense + Proxmox Security Control Plane

It emphasizes OPNsense, DNSSEC, Quad9 DoT, CrowdSec, VictoriaLogs, NetAlertX, OpenCanary, and Proxmox. That is accurate but stale after the Homepage migration.

## Outdated Items Found

| Current wording | Issue | Replacement wording |
|---|---|---|
| "OPNsense + Proxmox Security Control Plane" | Misses the new operating model. | "Home Network Security Control Plane" |
| "logs, canary alerts, asset awareness, and safe on-demand checks" | Missing dashboard, feeds, NetBox, Trivy/Syft, recovery tracking. | "logs, metrics, inventory, canary alerts, vulnerability/SBOM visibility, and recovery-status tracking" |
| "controls: OPNsense, DNSSEC, Quad9 DoT, CrowdSec, VictoriaLogs, NetAlertX, OpenCanary" | Missing current stack. | "controls: OPNsense, DNSSEC, Quad9 DoT, CrowdSec, Homepage, NetBox, Uptime Kuma, Victoria, NetAlertX, OpenCanary, Trivy/Syft" |
| Any public wording implying Glance is the active front door | Glance is retired. | "Homepage is the active operations dashboard. Glance was the original lightweight dashboard and was retired from active service after the Homepage migration. Glance backups were preserved." |
| "restore testing passed" in current state | Current public posture should not overclaim restore maturity. | "Restore testing remains a required gate before broad stateful updates." |

## Consistency Audit

- Glance: corrected to historical/retired wording where needed.
- Homepage/dashboard/operations dashboard/command center: standardized around HomeNet Operations Dashboard and Homepage Operations Dashboard.
- cockpit/status feeds: public language now says operations status feed and Prometheus-style status metrics; internal paths are only shown in the dashboard doc as RFC1918 examples.
- OPNsense/Proxmox/FreshTomato: documented as enforcement, visibility/control plane, and AP/admin-link layer respectively.
- NetBox/Uptime Kuma/Grafana/VictoriaMetrics/VictoriaLogs/NetAlertX/OpenCanary/Trivy/Syft/CrowdSec: included in README, dashboard, operations, roadmap, and website suggestions.
- WireGuard/Tailscale/VLAN/endpoint agents: documented as deferred or pending, not current production controls.
- UPnP/NAT-PMP/rpcbind: documented as disabled after validation.
- backup/restore/off-host: documented as known gaps and change gates, avoiding overclaiming restore maturity.
- Docker socket/API key/token/secret: documented as redaction and design constraints; no raw Docker socket or casual privileged API widgets.

## Security/Redaction Issues Found

- No committed raw OPNsense config XML, Proxmox backup, NetBox dump, `.env`, `.kdbx`, `.pem`, `.key`, `.crt`, `.p12`, `.ovpn`, or WireGuard key material was found in the repository scan.
- Existing screenshot assets are named sanitized; they should still be visually reviewed before publication.
- Internal RFC1918 examples are acceptable only when clearly labeled as private/non-routable and not reachable from the internet.
- Raw status feeds and `.prom` files should not be committed if labels expose real hostnames, device names, usernames, or topology.

## Updates Made

- Updated README summary, At A Glance, architecture diagram, current status, why-it-matters section, repository structure, and safety notes.
- Added canonical `docs/homepage-operations-dashboard.md`.
- Updated `docs/design-rationale.md`, `docs/operations.md`, `docs/current-state.md`, `docs/roadmap.md`, `docs/redaction-guide.md`, and `docs/modernization-sprint-2026-05-20.md`.
- Rebuilt `docs/decision-log.md` with ADR-001 through ADR-017.
- Created `docs/website-update-suggestions-2026-05-20.md`.
- Created `docs/project-update-2026-05-20.md`.

## Gaps Remaining

- Durable off-host backup target.
- Restore validation.
- Uptime Kuma internal status page slug if not already created locally.
- Prometheus-style `home_*` metrics ingestion into VictoriaMetrics/Grafana.
- Least-privilege read-only API widgets.
- Proxmox named admin/MFA and SSH key-only plan.
- WireGuard/Tailscale remote access decision.
- One endpoint telemetry pilot.
- Wired VLAN test before any household Wi-Fi segmentation.

## Suggested Screenshots/Diagrams

- Sanitized architecture diagram showing OPNsense enforcement and Proxmox visibility/control plane.
- Cropped Homepage Operations Dashboard screenshot with browser chrome removed and sensitive labels blurred.
- Control-plane flow diagram showing operations status feed and Prometheus-style status metrics.
- Recovery workflow diagram showing backup age, off-host copy, and restore-test gates.

## Final Publish Checklist

- [x] README current.
- [x] Website copy current in suggestion doc.
- [x] Glance references corrected.
- [x] Homepage Operations Dashboard documented.
- [x] Access locations documented safely as RFC1918/private examples.
- [x] Decision log created.
- [x] Modernization sprint documented.
- [x] Operations updated.
- [x] Redaction guide updated.
- [x] No high-risk secret findings in repo scan.
- [x] No new sensitive screenshots committed.
- [x] Links reviewed in changed Markdown.
- [x] Diagrams use sanitized Mermaid.
- [x] Tone is professional and practical.
- [x] Known gaps are honest.
- [x] Claims match current stated reality.
