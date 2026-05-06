# LinkedIn Copy

## Featured Project Entry

**Title:** OPNsense Home Network Security Lab

**Description:**
Built and documented a live OPNsense-based home network security perimeter focused on WAN/LAN firewalling, DNSSEC, DNS-over-TLS with Quad9, DHCP/local DNS operations, DNS-bypass prevention, CrowdSec firewall blocking, traffic-shaping work, private administration, and operational review. The public GitHub repository is intentionally sanitized to show architecture and security decision-making without exposing sensitive network details.

The project is written to show the mentality behind the build: practical defense over security theater, accurate documentation over inflated claims, and controls that can be explained, validated, and maintained.

**Skills to attach:**
Network Security, Firewall Administration, OPNsense, DNS Security, DNSSEC, CrowdSec, DHCP, Traffic Shaping, Security Operations, Documentation

**Project URL:**
https://github.com/srkyn/home-network-security

## Launch Post Draft

I published a sanitized writeup of my OPNsense home network security project:

https://github.com/srkyn/home-network-security

The project documents the defensive design behind my personal network perimeter: WAN/LAN firewalling, DNSSEC, DNS-over-TLS with Quad9, DHCP/local DNS operations, DNS-bypass prevention, CrowdSec firewall blocking, traffic-shaping work, private administration, and recurring validation checks.

I intentionally did not publish raw firewall exports, public IPs, keys, hostnames, or detailed internal addressing. The point is to show security engineering judgment without turning a portfolio project into a target map.

This was a useful exercise because home labs are strongest when they are treated like real environments: document what is actually enabled, separate current controls from future work, validate assumptions, and keep sensitive details out of public places.

My favorite part of this project is the discipline behind it. I am not trying to make a home network sound like an enterprise SOC. I am trying to show how I think: define the boundary, control DNS, enforce the path clients should use, add useful reputation blocking, keep management private, and be honest about what is still future work.

#cybersecurity #networksecurity #opnsense #homelab #securityoperations #firewall #dnssecurity

## Resume Bullets

- Built and maintained an OPNsense-based home network security perimeter with WAN/LAN firewalling, DNSSEC, DNS-over-TLS forwarding, DNS-bypass prevention, CrowdSec firewall blocking, and private administrative access.
- Documented firewall control objectives, operational validation checks, and a public redaction model to demonstrate security engineering without exposing sensitive network details.
- Created a repeatable review workflow for firewall rules, DNS behavior, CrowdSec blocking, backups, updates, and future IDS/IPS or segmentation changes.
- Explained the design rationale behind each control, including operational tradeoffs, current-state honesty, and future security improvements.
