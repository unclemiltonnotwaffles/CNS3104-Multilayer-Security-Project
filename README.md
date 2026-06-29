# CNS3104-Multilayer-Security-Project

**Enterprise-Grade Multi-Layer Security Architecture: VLAN Segmentation, IPS and Site-to-Site VPN**
CNS 3104 Project by 190358 (Chiwo Newa)

## Enterprise-Grade Multi-Layer Security Architecture

| Field | Details |
|---|---|
| Course | CNS 3104 – Computer Networks & Security |
| Student | Chiwo Newa (Reg. No. 190358) |
| Supervisor | Dr. Vitalis Ozianyi |
| Institution | Strathmore University, Nairobi, Kenya |
| Date | May 2026 |

---

## Overview

This project designs, implements and evaluates a multi-layer network security architecture for small and medium enterprises, simulated entirely in GNS3 2.2.59 using Cisco IOS images. The architecture integrates three core security layers:

- **VLAN Segmentation with Extended ACLs** — Six VLANs at HQ (HR, Finance, IT, Guest, DMZ, Management) and four at Branch (HR, Finance, IT, Guest), with ACLs enforcing departmental isolation and blocking lateral movement
- **Site-to-Site IPsec VPN** — Two encrypted tunnels (HQ↔Branch, HQ↔Cloud) using IKEv1, AES-256 encryption and SHA-256 hashing, protecting all inter-site traffic
- **Cisco IOS Intrusion Prevention System** — IPS engine active on both WAN interfaces of HQ-Router, inspecting all inbound north-south traffic
- **Centralized Syslog Monitoring** — All routers forwarding log events to a dedicated Syslog server on the Management VLAN

The simulation spans three sites (HQ, Branch, Cloud) with 5 routers/switches and 12 VPCS endpoint nodes. All 14 security test scenarios passed verification.

---

## Network Topology

| Site | Devices | Subnets |
|---|---|---|
| HQ | HQ-Router, HQ-Switch, 6 VPCS nodes | 192.168.10-60.0/24 |
| Branch | Branch-Router, Branch-Switch, 4 VPCS nodes | 192.168.110-140.0/24 |
| Cloud | Cloud-Router, 1 VPCS node | 192.168.3.0/24 |
| WAN Links | HQ↔Branch: 10.0.0.0/30, HQ↔Cloud: 10.0.1.0/30 | — |

---

## VLAN Addressing Table

| VLAN ID | Name | Site | Subnet | Gateway |
|---|---|---|---|---|
| 10 | HR | HQ | 192.168.10.0/24 | 192.168.10.254 |
| 20 | Finance | HQ | 192.168.20.0/24 | 192.168.20.254 |
| 30 | IT | HQ | 192.168.30.0/24 | 192.168.30.254 |
| 40 | Guest | HQ | 192.168.40.0/24 | 192.168.40.254 |
| 50 | DMZ | HQ | 192.168.50.0/24 | 192.168.50.254 |
| 60 | Management | HQ | 192.168.60.0/24 | 192.168.60.254 |
| 10 | HR | Branch | 192.168.110.0/24 | 192.168.110.254 |
| 20 | Finance | Branch | 192.168.120.0/24 | 192.168.120.254 |
| 30 | IT | Branch | 192.168.130.0/24 | 192.168.130.254 |
| 40 | Guest | Branch | 192.168.140.0/24 | 192.168.140.254 |

---

## Security Test Results

| ID | Test Scenario | Control | Result |
|---|---|---|---|
| T1 | HQ HR → Finance blocked | ACL: BLOCK-HR-TO-FINANCE | PASS |
| T2 | HQ Finance → HR blocked | ACL: BLOCK-FINANCE-TO-HR | PASS |
| T3 | HQ Guest → HR blocked | ACL: GUEST-RESTRICT | PASS |
| T4 | HQ Guest → Cloud blocked | ACL: GUEST-RESTRICT | PASS |
| T5 | HQ HR → IT permitted | ACL: permit ip any any | PASS |
| T6 | Branch HR → Finance blocked | ACL: BLOCK-HR-TO-FINANCE | PASS |
| T7 | Branch Guest → HR blocked | ACL: GUEST-RESTRICT | PASS |
| T8 | HQ HR → Branch HR via VPN | IPsec HQ-Branch tunnel | PASS |
| T9 | HQ-Branch tunnel QM_IDLE ACTIVE | IPsec Phase 1 | PASS |
| T10 | HQ IT → Cloud via VPN | IPsec HQ-Cloud tunnel | PASS |
| T11 | HQ-Cloud tunnel QM_IDLE ACTIVE | IPsec Phase 1 | PASS |
| T12 | IPS-RULE on both WAN interfaces | Cisco IOS IPS | PASS |
| T13 | Syslog forwarding to 192.168.60.100 | Syslog | PASS |
| T14 | HQ Management → Cloud permitted | Routing + VPN | PASS |

---

## Repository Structure
CNS3104-Multilayer-Security-Project/

├── GNS3/

│   └── SME-Security-Lab.gns3project

├── Configs/

│   ├── HQ-Router.txt

│   ├── Branch-Router.txt

│   ├── Cloud-Router.txt

│   ├── HQ-Switch.txt

│   └── Branch-Switch.txt

├── Diagrams/

│   ├── topology.drawio

│   └── topology.png

├── Report/

│   ├── Project-Proposal.pdf

│   └── Final-Report.pdf

├── TestMatrix/

│   └── Security-Test-Matrix.pdf

├── Presentation/

│   └── Defense-Slides.pptx

└── README.md
---

## Requirements

| Requirement | Details |
|---|---|
| GNS3 | Version 2.2.59 or later |
| GNS3 VM | VirtualBox or VMware |
| Router Image | c3725-adventerprisek9-mz.124-15.T14 |
| Switch Image | i86bi-linux-l2-adventerprisek9-15.1b.bin |

> **Note:** Cisco IOS images cannot be redistributed due to licensing restrictions. You must provide your own images and place them in your GNS3 images directory before opening the project.

---

## Known Limitations

- The c3725 IOS image does not include the full IPS signature database. Signatures 2004, 4000 and 4020 are not present in the atomic-ip engine. IPS effectiveness is demonstrated through `show ip ips all` and `show ip ips interfaces`. Full signature-based drop testing requires a Cisco device with an active IPS licence and updated SDF file.
- IKEv1 with pre-shared keys is used for VPN. IKEv2 and certificate-based authentication are not configured.
- The design is fully virtual with no physical hardware integration.
- Redundancy features are omitted to focus on core security controls.

---

## How to Use

1. Clone or download the repository
2. Install GNS3 2.2.59 and the GNS3 VM
3. Add the required Cisco IOS images to your GNS3 images directory
4. Open GNS3 → File → Import portable project → select `SME-Security-Lab.gns3project`
5. Start all nodes and follow the test matrix to verify security controls

---

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
