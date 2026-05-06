# Redaction Guide

This repository is public, so it documents the security project without exposing the live environment.

## Never Publish

- Public IP addresses.
- Raw OPNsense configuration exports.
- VPN keys, pre-shared keys, certificates, tokens, or credentials.
- Full internal IP plans.
- Real hostnames, usernames, serial numbers, MAC addresses, or ISP account details.
- Screenshots showing sensitive firewall, DHCP, DNS, ARP, VPN, or user/session data.

## Usually Redact

- Private management addresses.
- Exact VLAN IDs and subnet assignments.
- Device brands/models if they create unnecessary targeting detail.
- Log lines that identify household members, devices, locations, or usage patterns.

## Safe To Share

- High-level architecture diagrams.
- Zone names with generic labels.
- Rule intent without exact live values.
- Sanitized screenshots.
- Lessons learned.
- Maintenance checklists.
- Detection and review workflows.

## Screenshot Checklist

Before publishing a screenshot:

- Blur WAN IPs and gateway details.
- Blur internal IP addresses unless they are generic examples.
- Blur hostnames, usernames, and MAC addresses.
- Blur DNS query logs that reveal personal usage.
- Blur certificate, VPN, and key material.
- Re-read the screenshot at full size before committing it.

## Good Portfolio Pattern

Use this framing:

> I built and maintain a segmented OPNsense network with private administrative access, DNS filtering, IDS/IPS review, and documented validation checks.

Avoid this framing:

> Here is my exact firewall export, host inventory, management address, and VPN configuration.
