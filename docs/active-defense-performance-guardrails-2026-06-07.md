# Active Defense And Performance Guardrails - 2026-06-07

This note summarizes a sanitized follow-up sprint after the initial dashboard and control-plane work.

The goal was to make the network more responsive to suspicious local behavior while keeping the control plane lightweight. This is still a production home network, not a disposable lab, so containment must be reversible and monitoring must not become the largest workload.

## What Changed

- Added stronger active-defense observability around firewall security posture, deception signal, watchlist assets, and local containment workflow.
- Added guarded quarantine planning for suspicious local devices.
- Added AP-side MAC-deny guidance for confirmed unwanted wireless clients.
- Added performance guardrails for the security-services stack.
- Reduced redundant dashboard-side polling of raw metrics/log/exporter ports.
- Added a lightweight process-hygiene metric after observing stale child processes in the dashboard container.

## Active Defense Model

The active-defense model is deliberately defensive:

- Detect suspicious local behavior.
- Preserve evidence.
- Enrich events with known/unknown asset context.
- Prefer reversible containment.
- Avoid retaliation, hack-back, or third-party targeting.

This project uses deception and tarpitting concepts as tripwires and delay mechanisms inside the owned network. They are not used to attack outside systems.

## Containment Workflow

The public-safe containment workflow is:

1. Confirm the asset is unknown, suspicious, or on a watchlist.
2. Check whether it touched deception, tarpit, firewall, or admin surfaces.
3. Capture evidence before blocking.
4. Prefer AP-side MAC deny for confirmed unwanted wireless clients.
5. Use firewall-based quarantine for routed traffic only.
6. Keep rollback simple and documented.

Important limitation: on a flat LAN with an unmanaged switch, firewall rules can block routed traffic but cannot reliably stop same-subnet peer-to-peer traffic that never crosses the firewall.

## Performance Guardrails

Because the control plane runs on limited hardware, the sprint added guardrails for:

- Sustained container CPU use.
- Container memory pressure.
- Proxmox host CPU pressure.
- Proxmox host I/O wait.
- Dashboard process hygiene.

The intent is not to alert on every small blip. The thresholds are designed to catch sustained pressure before visibility tooling starts harming normal network use.

## Dashboard Polling Cleanup

Homepage remains the internal command-center dashboard, but it should not duplicate every low-level probe that Grafana and the metrics backend already perform.

Redundant dashboard-side checks against raw metrics/log/exporter ports were removed. Links remain for operators, while health and performance monitoring remain in Grafana/VictoriaMetrics.

## Public Redaction Notes

Not published:

- MAC addresses.
- Watchlist identifiers.
- Raw firewall aliases.
- Internal host maps.
- Raw dashboard configuration with operational paths.
- Device screenshots or raw API output.
- Credentials, tokens, keys, or recovery material.

Published:

- Control rationale.
- Operating sequence.
- Design tradeoffs.
- Sanitized guardrail descriptions.
- Known limitations.

## Result

The network gained a clearer active-defense response path without adding heavy always-on tooling or sacrificing performance. The work also clarified an important rule: containment is only useful if it is reversible, documented, and does not lock out the operator.
