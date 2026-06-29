# Security Test Matrix
## CNS3104 - Enterprise Multi-Layer Security Architecture
**Student:** Chiwo Newa (Reg. No. 190358)
**Supervisor:** Dr. Vitalis Ozianyi
**Date:** May 2026

---

## Test Environment

| Parameter | Details |
|---|---|
| Simulation Platform | GNS3 2.2.59 |
| Router Image | c3725-adventerprisek9-mz.124-15.T14 |
| Switch Image | i86bi-linux-l2-adventerprisek9-15.1b.bin |
| Test Date | June 2026 |
| Total Tests | 14 |
| Tests Passed | 14 |
| Tests Failed | 0 |

---

## Layer 1 — VLAN Segmentation and ACL Enforcement

| ID | Test Scenario | Device | Command Used | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| T1 | HQ HR → Finance blocked | HQ-HR-PC | `ping 192.168.20.10` | Communication administratively prohibited from 192.168.10.254 | Communication administratively prohibited from 192.168.10.254 | PASS |
| T2 | HQ Finance → HR blocked | HQ-Finance-PC | `ping 192.168.10.10` | Communication administratively prohibited from 192.168.20.254 | Communication administratively prohibited from 192.168.20.254 | PASS |
| T3 | HQ Guest → HR blocked | HQ-Guest-PC | `ping 192.168.10.10` | Communication administratively prohibited from 192.168.40.254 | Communication administratively prohibited from 192.168.40.254 | PASS |
| T4 | HQ Guest → Cloud blocked | HQ-Guest-PC | `ping 192.168.3.10` | Communication administratively prohibited from 192.168.40.254 | Communication administratively prohibited from 192.168.40.254 | PASS |
| T5 | HQ HR → IT permitted | HQ-HR-PC | `ping 192.168.30.10` | 84 bytes reply from 192.168.30.10 | 84 bytes reply from 192.168.30.10 | PASS |
| T6 | Branch HR → Finance blocked | BR-HR-PC | `ping 192.168.120.10` | Communication administratively prohibited from 192.168.110.254 | Communication administratively prohibited from 192.168.110.254 | PASS |
| T7 | Branch Guest → HR blocked | BR-Guest-PC | `ping 192.168.110.10` | Communication administratively prohibited from 192.168.140.254 | Communication administratively prohibited from 192.168.140.254 | PASS |

---

## Layer 2 — Site-to-Site IPsec VPN

| ID | Test Scenario | Device | Command Used | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| T8 | HQ HR → Branch HR via VPN | HQ-HR-PC | `ping 192.168.110.10` | 84 bytes reply from 192.168.110.10 | 84 bytes reply from 192.168.110.10 | PASS |
| T9 | HQ-Branch tunnel active | HQ-Router | `show crypto isakmp sa` | QM_IDLE ACTIVE for peer 10.0.0.2 | QM_IDLE ACTIVE for peer 10.0.0.2 | PASS |
| T10 | HQ IT → Cloud via VPN | HQ-IT-PC | `ping 192.168.3.10` | 84 bytes reply from 192.168.3.10 | 84 bytes reply from 192.168.3.10 | PASS |
| T11 | HQ-Cloud tunnel active | HQ-Router | `show crypto isakmp sa` | QM_IDLE ACTIVE for peer 10.0.1.2 | QM_IDLE ACTIVE for peer 10.0.1.2 | PASS |

---

## Layer 3 — Cisco IOS IPS

| ID | Test Scenario | Device | Command Used | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| T12 | IPS engine active on WAN interfaces | HQ-Router | `show ip ips interfaces` | IPS-RULE inbound on f0/1 and f1/0 | IPS-RULE inbound on f0/1 and f1/0 | PASS |

---

## Monitoring — Syslog

| ID | Test Scenario | Device | Command Used | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| T13 | Syslog forwarding active | HQ-Router | `show logging` | Messages forwarded to 192.168.60.100 port 514 | 61 message lines logged to 192.168.60.100 port 514 | PASS |

---

## Connectivity Verification

| ID | Test Scenario | Device | Command Used | Expected Result | Actual Result | Status |
|---|---|---|---|---|---|---|
| T14 | HQ Management → Cloud permitted | HQ-Mgmt-PC | `ping 192.168.3.10` | 84 bytes reply from 192.168.3.10 | 84 bytes reply from 192.168.3.10 | PASS |

---

## ACL Verification Commands

Run these on HQ-Router and Branch-Router to confirm ACL hit counts:
show ip access-lists BLOCK-HR-TO-FINANCE

show ip access-lists BLOCK-FINANCE-TO-HR

show ip access-lists GUEST-RESTRICT
---

## VPN Verification Commands

Run these on HQ-Router to confirm tunnel status and packet counts:
show crypto isakmp sa

show crypto ipsec sa

show crypto map
---

## IPS Verification Commands

Run these on HQ-Router to confirm IPS engine status:
show ip ips all

show ip ips interfaces

show ip ips signature
---

## Known Limitations

- The c3725 IOS image does not include the full IPS signature database. Signatures 2004, 4000 and 4020 are not present in the atomic-ip engine. IPS is verified through engine configuration and interface assignment only.
- IPS packet drop behaviour cannot be fully verified in this emulation environment. A physical Cisco device with an active IPS licence and updated SDF file would be required for full signature-based testing.
