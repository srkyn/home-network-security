# Design Rationale

This document explains the thinking behind the OPNsense home network security project. The goal is to make the project understandable from the outside: what is configured, why each choice makes sense, and what tradeoff it represents.

## Overall Mentality

I am treating the network like a small production environment. That means I care about clarity, repeatability, and knowing what is actually enabled. I do not want a portfolio project that looks impressive only because every feature name is listed. I want the configuration to tell a believable story: a real perimeter, real DNS control, real blocking, real maintenance habits, and honest notes about what is not finished yet.

My bias is toward controls I can explain and validate. If I cannot describe what a setting does, why I enabled it, and how I would check whether it is working, it does not belong in the story yet.

## OPNsense As The Edge Firewall

OPNsense is the control point because the firewall should be the place where network intent becomes enforceable policy. Instead of relying on each device to behave perfectly, the network has one central system for WAN/LAN boundary decisions, DNS path control, NAT, blocking, and future inspection.

The mentality here is ownership. I do not want the ISP router to be a mystery box doing the important work. I want a system where I can see the rules, back up the configuration, review changes, and understand the path traffic takes.

## Why Proxmox Is The Visibility/Control Plane

Proxmox is not inline and does not decide whether traffic is allowed. It hosts the tools that make the network easier to understand: Homepage, Uptime Kuma, Grafana, VictoriaMetrics, VictoriaLogs, NetBox, NetAlertX, OpenCanary, and report runners.

That separation matters. If the Proxmox node is down, normal internet should still route through OPNsense. The control plane improves visibility, recovery, and documentation without becoming a fragile dependency for household connectivity.

## WAN Hardening

The WAN interface blocks private networks and bogon networks. This is a simple control, but it matches the way I think about security: start by rejecting traffic that should not make sense at that boundary.

It is not glamorous, but it is clean hygiene. The goal is to remove obvious bad or invalid paths before spending attention on more complex detections.

## Single LAN Design

The current network is a single LAN, and I am documenting that directly. I could make the project sound more advanced by pretending segmentation already exists, but that would be the wrong instinct.

The mentality is accuracy first. A single-LAN network can still have useful security controls, but it should not be described like a segmented enterprise network. Future VLAN or guest/lab segmentation belongs in the roadmap until it is actually configured.

## DHCP Model

Dnsmasq provides DHCP for the LAN with a scoped range for trusted clients. This creates order without publishing exact internal addressing or host-reservation details.

The mentality is inventory without overcomplication. Even in a home network, knowing what should be present makes troubleshooting and security review easier.

## Unbound DNS

Unbound is the main DNS resolver on port 53. DNS is one of the best places to add control because almost every device depends on it before reaching a service.

The mentality is to make quiet infrastructure visible. DNS is not just a convenience service; it is a control layer, a troubleshooting layer, and a place where suspicious behavior often leaves early clues.

## DNSSEC

DNSSEC is enabled to add validation to DNS responses where supported. It is not a complete control by itself, but it improves trust in the resolver path and shows that DNS integrity matters.

The mentality is layered assurance. I do not expect one setting to solve everything, but I do want each layer to reduce a real class of risk.

## DNS-over-TLS To Quad9

The resolver forwards upstream DNS over TLS to Quad9. This gives the network a more private and security-oriented upstream path than plain DNS to arbitrary resolvers.

The choice of Quad9 reflects a defensive bias: use an upstream provider that is commonly associated with security filtering and privacy-minded DNS service. The important part is not brand loyalty; it is choosing the resolver path deliberately instead of accepting defaults blindly.

## Dnsmasq For Local DHCP/DNS Support

Dnsmasq is still used for DHCP and local naming support while Unbound handles the main DNS listener. This lets the environment keep local host awareness while still using Unbound as the primary resolver path.

The mentality is practical integration. Security setups often become messy when tools overlap. The better approach is to understand which component owns which job and document the handoff.

## DNS Bypass Prevention

The LAN firewall includes a DNS-bypass block rule so clients are pushed toward the approved local resolver instead of quietly using outside DNS servers.

The mentality is enforcement over hope. If DNS is meant to be a control point, clients cannot be allowed to casually route around it. This is the difference between "I configured DNS" and "the network is actually expected to use it."

## Why UPnP/NAT-PMP Was Disabled

UPnP/NAT-PMP can create inbound exposure automatically. Disabling it was a low-drama reduction in attack surface that fit the production-safe sprint model: back up first, change one exposure-control setting, then verify normal use.

The tradeoff is that some gaming or peer-to-peer applications may need manual troubleshooting later. That is preferable to leaving broad automatic port mapping enabled by default.

## CrowdSec

CrowdSec is enabled with the agent, local API, firewall bouncer, and blocklist aliases. This adds a reputation and community-signal layer to the firewall.

The mentality is shared signal, locally enforced. I like controls that turn outside intelligence into a local decision without making the whole network dependent on manual review. It is not a replacement for good firewall rules, but it adds another useful filter.

## IDS/IPS Status

Suricata configuration exists, but IDS is currently disabled. That is documented as current state, not hidden.

The mentality is honesty about operational load. IDS/IPS is only valuable if someone reviews alerts and tunes noise. Turning it on for appearance would be weaker than saying, "the configuration path is there, but I have not made it an active operating control yet."

## VPN Status

WireGuard and OpenVPN are not currently active. Remote access is therefore future work, not a current capability.

The mentality is controlled exposure. Remote access should be added when there is a clear reason, a secure authentication model, and a review process. Until then, not exposing remote management is a valid security choice.

## Why rpcbind Was Disabled

Proxmox had `rpcbind` available, but NFS was not in use. After checking the dependency, the service was disabled to remove an unnecessary listener.

This is deliberately modest hardening: remove what is not needed, validate that storage and containers still work, and document how to revisit the decision if NFS is introduced later.

## Traffic Shaping

Traffic-shaping queues and rules exist for gaming and latency-sensitive traffic, but the pipes are currently disabled. This shows performance tuning work without pretending the full shaping model is active.

The mentality is that usability matters. A secure network that feels broken gets bypassed. Latency, gaming, updates, and normal household traffic are part of the environment, so performance tuning belongs in the design conversation.

## Private Administration

Administrative access is kept to the trusted LAN side. Public documentation avoids raw config exports, secrets, detailed host inventory, and management specifics.

The mentality is that a portfolio should demonstrate judgment, not publish a target map. The point is to show how I think and what I built while keeping the actual environment safe.

## Future Direction

The next meaningful improvements are durable off-host backups, staged Proxmox admin hardening, segmentation testing, remote access only if there is a real need and test path, and clearer operational evidence through sanitized diagrams or change logs.

The mentality is iteration. I want each added control to have a reason, a validation step, and a maintenance habit. That is how a home lab becomes real experience instead of a collection of toggles.

## Proxmox As A Visibility Plane

The Proxmox security node was added after the firewall baseline because the network needed better visibility without adding packet-path risk. The firewall still enforces traffic policy. Proxmox hosts supporting controls: logs, dashboards, uptime checks, asset discovery, canary services, backup targets, and safe manual scanners.

This separation is deliberate. A home security stack can easily become fragile if every tool is placed inline or every feature is turned on. The better pattern for this environment is:

- OPNsense decides what traffic is allowed.
- Proxmox helps explain what is happening.
- On-demand scanners run only when authorized and useful.
- Always-on services stay small enough for the hardware.

## Why LXCs Instead Of Full VMs

The host has limited RAM, so LXCs were chosen first. That gives the project Linux isolation with much less overhead than full virtual machines. The only Docker runtime lives inside the `secops-core` LXC, keeping the Proxmox host cleaner and reducing the blast radius of container experiments.

The tradeoff is that Docker-in-LXC needs explicit features and a bit more care. That was acceptable because the result avoids a heavier VM while keeping the services inspectable and easy to back up.

## Why VictoriaLogs

VictoriaLogs was selected because the project needed a lightweight log backend, not a heavyweight SIEM. The goal is searchable evidence for OPNsense and canary events with retention and disk caps. This matches the size of the environment and avoids spending most of the host's RAM on the logging layer.

## Why Homepage Replaced Glance

Glance was a good lightweight front door, but the operating model grew beyond a link dashboard. Homepage replaced it because the network needed Mission Status, Recovery Snapshot, Security Snapshot, local status feeds, and a clearer dashboard-style hierarchy.

Grafana remains the deeper metrics layer. Homepage is the fast daily view: what is healthy, what is stale, what is risky, and where to click next.

## Why Local Status Feeds Came Before Privileged API Widgets

Homepage can support richer API widgets, but privileged tokens should not be the first move in a public-facing portfolio project or a daily-use network. The local operations status feed and Prometheus-style metrics feed provide useful live state without adding broad credentials.

Read-only API widgets can be added later, one at a time, with least-privilege tokens and a clear reason for each integration.

## Why Uptime Kuma Uses SQLite

Uptime Kuma was configured with SQLite because the monitor set is small. Running MariaDB for a handful of home service checks would add unnecessary memory and maintenance overhead. SQLite is simpler to back up and easier to reason about.

## Why OpenCanary

The fake NAS creates a signal that should be rare. Normal household use should not touch it. That makes canary interaction more meaningful than another noisy vulnerability dashboard.

Canary lures were added to make accidental or exploratory interaction more visible. Public docs describe the idea without publishing real lure contents, exact paths, or raw logs.

## Why Scanning Is Manual

Trivy and Syft are useful for visibility, but scheduled or automatic remediation can create noise, load, and breakage. They are used as report generators and triage inputs so the operator chooses when and what to update. That keeps the project defensive and controlled.

## Why Raw Docker Socket Is Avoided

The Docker socket is effectively administrative control over containers. Mounting it into a dashboard would make a convenience feature more privileged than it looks.

If container status visibility is needed later, the safer path is a docker-socket-proxy with write methods disabled and a narrow widget purpose.

## Why Admin UIs Are Linked, Not Embedded

OPNsense, Proxmox, and FreshTomato are privileged admin interfaces. The dashboard links to them but does not iframe them or work around their browser protections.

That keeps authentication boundaries clear and avoids turning the dashboard into a brittle wrapper around sensitive consoles.

## Why Off-Host Backup/Restore Is The Next Critical Gap

Local backups are useful, but they do not solve host loss, disk loss, or operator mistakes. The next recovery milestone is durable off-host backup plus a documented restore test.

Container updates, stateful service upgrades, and larger control-plane changes should wait behind that gate.

## Why VLANs Are Staged, Not Rushed

Segmentation is valuable, but a rushed VLAN migration can break DHCP, DNS, Wi-Fi, management access, and daily household use. The current flat LAN is documented honestly.

The staged path is one wired VLAN test first, then management/IoT/guest segmentation only after the physical path, AP support, firewall rules, and rollback plan are clear.
