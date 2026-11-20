# Packet Tracer 9.0.1 Implementation

This directory documents the adaptation of the CNS 3104 multilayer security project for Cisco Packet Tracer 9.0.1.

## Implementation approach

The Packet Tracer build uses a three-site hub-and-spoke design:

- Headquarters (HQ)
- Branch office (BR)
- Cloud / data centre (CLOUD)

Layer 3 switching is used for inter-VLAN routing at HQ and the Branch. Site routers provide WAN connectivity. HQ is the hub for the Branch and Cloud WAN links.

> **Simulation note:** Packet Tracer does not emulate every feature available in GNS3 or on physical Cisco IOS platforms. Each security feature must be validated on the selected Packet Tracer device and IOS feature set. Features that cannot be demonstrated will be documented as limitations rather than reported as implemented.

## Stage 1 — Device inventory

| Site | Device name | Packet Tracer device | Quantity |
|---|---|---|---:|
| HQ | HQ-RTR | Cisco 2911 | 1 |
| HQ | HQ-CORE-SW | Cisco 3560-24PS | 1 |
| HQ | HQ-FIN-SW | Cisco 2960 | 1 |
| HQ | HQ-HR-SW | Cisco 2960 | 1 |
| HQ | HQ-IT-SW | Cisco 2960 | 1 |
| HQ | HQ-SRV-SW | Cisco 2960 | 1 |
| HQ | HQ-FIN-PC1, HQ-FIN-PC2 | PC-PT | 2 |
| HQ | HQ-HR-PC1, HQ-HR-PC2 | PC-PT | 2 |
| HQ | HQ-IT-PC1, HQ-IT-PC2 | PC-PT | 2 |
| HQ | HQ-SERVER, HQ-SYSLOG | Server-PT | 2 |
| Branch | BR-RTR | Cisco 2911 | 1 |
| Branch | BR-CORE-SW | Cisco 3560-24PS | 1 |
| Branch | BR-FIN-SW | Cisco 2960 | 1 |
| Branch | BR-HR-SW | Cisco 2960 | 1 |
| Branch | BR-FIN-PC1, BR-FIN-PC2 | PC-PT | 2 |
| Branch | BR-HR-PC1, BR-HR-PC2 | PC-PT | 2 |
| Cloud | CLOUD-RTR | Cisco 2911 | 1 |
| Cloud | CLOUD-SW | Cisco 2960 | 1 |
| Cloud | CLOUD-WEB, CLOUD-DNS, CLOUD-APP | Server-PT | 3 |

**Inventory total:** 27 devices — 3 routers, 9 switches, 10 PCs and 5 servers.

## Stage 1 — Cabling schedule

Use the exact interface pairs below. If an interface label differs in the installed Packet Tracer device, stop and verify the model/interface list before substituting another port.

### Headquarters

| From | Interface | To | Interface |
|---|---|---|---|
| HQ-RTR | G0/0 | HQ-CORE-SW | Gi0/1 |
| HQ-CORE-SW | Fa0/21 | HQ-FIN-SW | Gi0/1 |
| HQ-CORE-SW | Fa0/22 | HQ-HR-SW | Gi0/1 |
| HQ-CORE-SW | Fa0/23 | HQ-IT-SW | Gi0/1 |
| HQ-CORE-SW | Fa0/24 | HQ-SRV-SW | Gi0/1 |
| HQ-FIN-SW | Fa0/1 | HQ-FIN-PC1 | Fa0 |
| HQ-FIN-SW | Fa0/2 | HQ-FIN-PC2 | Fa0 |
| HQ-HR-SW | Fa0/1 | HQ-HR-PC1 | Fa0 |
| HQ-HR-SW | Fa0/2 | HQ-HR-PC2 | Fa0 |
| HQ-IT-SW | Fa0/1 | HQ-IT-PC1 | Fa0 |
| HQ-IT-SW | Fa0/2 | HQ-IT-PC2 | Fa0 |
| HQ-SRV-SW | Fa0/1 | HQ-SERVER | Fa0 |
| HQ-SRV-SW | Fa0/2 | HQ-SYSLOG | Fa0 |

### Branch office

| From | Interface | To | Interface |
|---|---|---|---|
| BR-RTR | G0/0 | BR-CORE-SW | Gi0/1 |
| BR-CORE-SW | Fa0/23 | BR-FIN-SW | Gi0/1 |
| BR-CORE-SW | Fa0/24 | BR-HR-SW | Gi0/1 |
| BR-FIN-SW | Fa0/1 | BR-FIN-PC1 | Fa0 |
| BR-FIN-SW | Fa0/2 | BR-FIN-PC2 | Fa0 |
| BR-HR-SW | Fa0/1 | BR-HR-PC1 | Fa0 |
| BR-HR-SW | Fa0/2 | BR-HR-PC2 | Fa0 |

### Cloud / data centre

| From | Interface | To | Interface |
|---|---|---|---|
| CLOUD-RTR | G0/0 | CLOUD-SW | Gi0/1 |
| CLOUD-SW | Fa0/1 | CLOUD-WEB | Fa0 |
| CLOUD-SW | Fa0/2 | CLOUD-DNS | Fa0 |
| CLOUD-SW | Fa0/3 | CLOUD-APP | Fa0 |

### WAN links

| From | Interface | To | Interface |
|---|---|---|---|
| HQ-RTR | G0/1 | BR-RTR | G0/1 |
| HQ-RTR | G0/2 | CLOUD-RTR | G0/1 |

This is a hub-and-spoke physical WAN: HQ has a direct routed link to the Branch and a separate direct routed link to the Cloud site.

## Cable selection

- Router-to-switch: copper straight-through.
- Switch-to-switch: copper crossover (or Packet Tracer automatic connection selection).
- Switch-to-PC/server: copper straight-through.
- Router-to-router Ethernet: copper crossover (or Packet Tracer automatic connection selection).

Packet Tracer's automatic connection tool can be used if the selected device ports support the resulting link.

## Stage 1 completion checklist

- [ ] All 27 devices placed and named exactly as listed.
- [ ] All HQ links match the cabling schedule.
- [ ] All Branch links match the cabling schedule.
- [ ] All Cloud links match the cabling schedule.
- [ ] Both WAN links connect directly to HQ-RTR.
- [ ] The Packet Tracer project is saved as `CNS3104_Multilayer_Security_Project.pkt`.
- [ ] A screenshot of the complete physical topology is saved as implementation evidence.

Router interfaces may remain down until they are configured and enabled in a later stage. Do not treat an administratively down router link at this stage as a cabling failure.

## Planned implementation stages

1. Physical topology and cabling.
2. VLAN creation, access-port assignment and 802.1Q trunks.
3. SVIs and inter-VLAN routing on the multilayer switches.
4. WAN interface addressing and routing between sites.
5. Extended ACLs and departmental access policy.
6. Device hardening, SSH and switch port security.
7. Site-to-site IPsec VPN, subject to Packet Tracer IOS support.
8. Server services and centralized Syslog.
9. Security validation, test evidence and documentation.

## Relationship to the original GNS3 project

The original GNS3 configuration and its test results are reference material only. Packet Tracer configurations, test outcomes and screenshots must be generated and verified independently. Do not mark a test as passed until it has been reproduced in the Packet Tracer simulation.
