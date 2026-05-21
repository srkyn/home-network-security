# LinkedIn Copy

## Featured Project Entry

**Title:** OPNsense Home Network Security Lab

**Description:**
Documented a live OPNsense home network perimeter covering WAN/LAN firewalling,
DNSSEC, DNS-over-TLS with Quad9, DHCP/local DNS operations, DNS-bypass blocking,
CrowdSec, private administration, traffic-shaping notes, and recurring review.

The public repository shows architecture and security decisions without exposing
raw firewall exports, public IPs, keys, hostnames, or internal addressing.

**Skills to attach:**
Network Security, Firewall Administration, OPNsense, DNS Security, DNSSEC,
CrowdSec, DHCP, Traffic Shaping, Security Operations, Documentation

**Project URL:**
https://github.com/srkyn/home-network-security

## Launch Post Draft

I published a public writeup of my OPNsense home network security project:

https://github.com/srkyn/home-network-security

It covers WAN/LAN firewalling, DNSSEC, DNS-over-TLS with Quad9, DHCP/local DNS,
DNS-bypass prevention, CrowdSec firewall blocking, private administration,
traffic-shaping notes, and recurring validation checks.

I did not publish raw firewall exports, public IPs, keys, hostnames, or detailed
internal addressing. The point is to show security engineering judgment without
turning a portfolio project into a target map.

The useful part was separating current controls from future work: what is
enabled, why it exists, how it can be validated, and what should stay private.

#cybersecurity #networksecurity #opnsense #homelab #securityoperations #firewall #dnssecurity

## Featured Project Entry: Security Control Plane

**Title:** Lightweight Home Security Control Plane for OPNsense

**Description:**
Built and documented a lightweight security control plane for a live OPNsense
home network using Proxmox LXCs, VictoriaLogs, NetAlertX, OpenCanary, Uptime
Kuma, Homepage, NetBox, Trivy, Syft, and CrowdSec.

The design prioritizes defensive value on limited hardware: centralized logs,
unknown-device awareness, canary interaction alerts, config backup/audit, safe
report-based scanning, and an internal Homepage dashboard. OPNsense remains the enforcement
point, while Proxmox provides visibility without sitting inline or affecting
gaming latency.

**Project URL:**
https://github.com/srkyn/home-network-security

**Skills to attach:**
Network Security, Proxmox, Linux Containers, OPNsense, Log Management,
Security Operations, Threat Detection, OpenCanary, Trivy, Syft, CrowdSec,
Technical Documentation

## Launch Post Draft: Security Control Plane

I finished documenting a home network security project that turns a small
Proxmox node into a lightweight control plane for an OPNsense firewall.

The goal was not to build a heavy SIEM or overcomplicate a home network. The
goal was practical defensive value on limited hardware:

- OPNsense stays the enforcement point.
- Proxmox provides visibility and operations.
- VictoriaLogs centralizes firewall and canary logs.
- NetAlertX tracks unknown devices.
- OpenCanary acts as a fake internal NAS tripwire.
- Uptime Kuma monitors service health.
- Trivy and Syft provide report-based visibility without automatic remediation.
- Homepage provides the internal dashboard for daily checks.

The design is intentionally gaming-safe: no inline proxy, no scheduled
vulnerability scans, no IPS blocking by default, and no traffic path moved
through the security node.

I also documented what I did not publish: raw configs, secrets, exact internal
inventory, keys, MACs, or anything that would turn a portfolio project into a
target map.

Project writeup:
https://github.com/srkyn/home-network-security

#cybersecurity #networksecurity #opnsense #proxmox #homelab #securityoperations #blueteam

## Resume Bullets

- Built and maintained an OPNsense-based home network security perimeter with WAN/LAN firewalling, DNSSEC, DNS-over-TLS forwarding, DNS-bypass prevention, CrowdSec firewall blocking, and private administrative access.
- Documented firewall control objectives, validation checks, and a public redaction model without exposing sensitive network details.
- Created a repeatable review workflow for firewall rules, DNS behavior, CrowdSec blocking, backups, updates, and future IDS/IPS or segmentation changes.
- Explained the design rationale behind each control, including current limitations and future improvements.
- Built a lightweight Proxmox security control plane for an OPNsense network using LXCs, Homepage, VictoriaLogs, VictoriaMetrics, NetAlertX, OpenCanary, Uptime Kuma, NetBox, Trivy, and Syft.
- Centralized firewall and canary logs, added unknown-device awareness and deception alerts, configured OPNsense backup/audit workflows, and preserved latency by keeping controls out of the traffic path.
