# Redaction Guide

This repository is public, so it documents the security project without exposing the live environment.

## Never Publish

- Public IP addresses.
- Raw OPNsense configuration exports.
- VPN keys, pre-shared keys, certificates, tokens, or credentials.
- Full internal IP plans.
- Real hostnames, usernames, serial numbers, MAC addresses, or ISP account details.
- Screenshots showing sensitive firewall, DHCP, DNS, ARP, VPN, or user/session data.
- Screenshots with visible browser tabs, bookmarks, profiles, downloads, extensions, or desktop notifications.
- Raw dashboard configuration files if they contain tokens, labels, internal host maps, or private hostnames.
- Uptime Kuma push URLs.
- API widget configuration that includes tokens or bearer strings.
- Raw NetBox exports or database dumps.
- Trivy reports that expose exact image names, internal registry names, private paths, or sensitive package context.
- Syft SBOMs that expose exact private image names, internal registry names, or sensitive file paths.

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
- Crop or blur browser tabs, bookmarks, toolbar extensions, profile icons, and download bars.
- Crop or blur dashboard URLs if they reveal internal hostnames, paths, or tokens.
- Re-read the screenshot at full size before committing it.

## Dashboard And Telemetry Files

- Do not publish live dashboard screenshots unless internal IPs, tokens, usernames, browser chrome, and sensitive labels are cropped or blurred.
- Dashboard screenshots must be cropped or blurred before publication.
- Browser tabs, bookmarks, profile icons, extensions, downloads, and desktop notifications must not appear in screenshots.
- Internal IPs should be minimized or generalized unless an RFC1918 example is needed for sanitized architecture.
- Do not publish raw `status.json` if it contains sensitive hostnames, exact internal maps, usernames, or private paths.
- Do not publish `.prom` files if labels reveal sensitive hostnames, device names, exact internal topology, or user behavior.
- Do not publish Uptime Kuma push URLs.
- Do not publish API widget config with tokens.
- Do not publish raw NetBox exports.
- Do not publish Trivy/Syft reports until exact image names, private registry names, private paths, and sensitive labels are removed.
- Public docs should use roles such as `firewall`, `Proxmox host`, `secops-core`, `canary host`, and `access point` instead of a full private host map.
- Sanitized diagrams are preferred over browser screenshots.

## Good Portfolio Pattern

Use this framing:

> I built and maintain an OPNsense network security lab with private administrative access, DNSSEC, DNS-over-TLS, DNS-bypass blocking, CrowdSec, and documented validation checks.

Avoid this framing:

> Here is my exact firewall export, host inventory, management address, and VPN configuration.
