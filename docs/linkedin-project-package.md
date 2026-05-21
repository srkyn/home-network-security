# LinkedIn Project Package

Use this page to turn the project into LinkedIn profile copy, a featured project description, a resume bullet, or a short launch post.

## Project Title

Lightweight Home Security Control Plane for OPNsense using Proxmox, Homepage, VictoriaLogs, VictoriaMetrics, NetAlertX, OpenCanary, Uptime Kuma, NetBox, Trivy, Syft, and CrowdSec

## Short Description

Built a low-overhead home security control plane around an OPNsense firewall and an 8 GB Proxmox node. The setup uses repurposed hardware: a small mini PC for OPNsense and a recycled laptop for Proxmox. The design keeps OPNsense as the enforcement point while Proxmox provides centralized logs, uptime monitoring, asset discovery, canary detection, configuration backup, and safe on-demand security checks.

## What It Demonstrates

- Network security architecture and tradeoff analysis.
- OPNsense firewall operations.
- Lightweight Proxmox/LXC service design.
- Log centralization with retention and disk caps.
- Canary/deception engineering with OpenCanary.
- Asset discovery and unknown-device review with NetAlertX.
- Uptime monitoring with Uptime Kuma.
- Safe use of Trivy and Syft without intrusive scheduled scanning or automatic remediation.
- Operational documentation, rollback planning, and public redaction discipline.

## Why It Matters

This is not a lab that exists only on paper. It is a live home network security build designed around real constraints: repurposed hardware, limited RAM, household usability, gaming latency, private data protection, and the need to avoid breaking the internet while adding defensive visibility.

The main design decision was to keep OPNsense as the traffic enforcement point and make Proxmox the visibility/control plane. That means the security stack improves awareness without becoming an inline performance bottleneck.

## LinkedIn Featured Project Entry

**Title:** Lightweight Home Security Control Plane for OPNsense

**Description:**
Built and documented a lightweight security control plane for a live OPNsense home network using Proxmox LXCs, Homepage, VictoriaLogs, VictoriaMetrics, NetAlertX, OpenCanary, Uptime Kuma, NetBox, Trivy, Syft, and CrowdSec.

The build runs on repurposed hardware: a mini PC for the OPNsense firewall and a recycled 8 GB laptop for Proxmox. The design prioritizes defensive value on limited hardware: centralized logs, unknown-device awareness, canary interaction alerts, config backup/audit, safe report-based scanning, and an internal Homepage dashboard. OPNsense remains the enforcement point, while Proxmox provides visibility without sitting inline or affecting gaming latency.

**Project URL:**
https://github.com/srkyn/home-network-security

**Skills to attach:**
Network Security, OPNsense, Proxmox, Linux Containers, Security Operations, Log Management, Threat Detection, OpenCanary, Trivy, Syft, CrowdSec, Documentation

## Resume Bullet

Built a lightweight home security control plane for an OPNsense network using Proxmox LXCs, Homepage, VictoriaLogs, VictoriaMetrics, NetAlertX, OpenCanary, Uptime Kuma, NetBox, Trivy, and Syft; centralized firewall/canary logs, added unknown-device awareness and deception alerts, configured safe backups/audits, and preserved gaming latency by keeping all controls out of the traffic path.

## Launch Post Draft

I finished documenting a home network security project that turns a small Proxmox node into a lightweight control plane for an OPNsense firewall.

The goal was not to build a heavy SIEM or overcomplicate a home network. The goal was practical defensive value on limited hardware:

- OPNsense stays the enforcement point.
- Proxmox provides visibility and operations.
- VictoriaLogs centralizes firewall and canary logs.
- NetAlertX tracks unknown devices.
- OpenCanary acts as a fake internal NAS tripwire.
- Uptime Kuma monitors service health.
- Trivy and Syft provide report-based visibility without automatic remediation.
- Homepage provides the internal dashboard for daily checks.

The design is intentionally gaming-safe: no inline proxy, no scheduled vulnerability scans, no IPS blocking by default, and no traffic path moved through the security node.

I also documented what I did not publish: raw configs, secrets, exact internal inventory, keys, MACs, or anything that would turn a portfolio project into a target map.

Project writeup:
https://github.com/srkyn/home-network-security

#cybersecurity #networksecurity #opnsense #proxmox #homelab #securityoperations #blueteam

## Screenshots To Capture

Keep screenshots sanitized. Crop or blur exact internal details where possible.

- Homepage dashboard overview.
- Uptime Kuma monitor list.
- VictoriaLogs query showing a redacted OPNsense syslog event.
- NetAlertX device list with hostnames/MACs blurred.
- OpenCanary fake NAS page.
- Proxmox CT list with sensitive notes hidden.
