# Audit Checklist — NVIDIA Regional R&D Hub

**Project:** NVIDIA Regional R&D and Production Hub  
**Project ID:** NVIDIA-REG-RD-2026-1106  
**Scale:** Likelihood (1-3) × Impact (1-3) = Risk Score; Rating (1-2=LOW, 3-4=MEDIUM, 6=HIGH, 9=CRITICAL)

---

## AREA 1: NETWORK SEGMENTATION (3 checks)

| ID | Control | Framework | Question | Evidence | Pass Condition | Result | Finding? |
|----|---------|-----------|----------|----------|---|---|---|
| **NS-01** | VLAN Isolation | ISO 27001 A.8.22 | Are workstations in different departments separated by different VLANs? | VLAN Worksheet (9 VLANs defined), Network Impl. Report §3 | 9 VLANs configured, one per dept/function | ✅ **PASS** | No |
| **NS-02** | ACL Enforcement | ISO 27001 A.8.20 | Do ACLs on Layer 3 switches prevent inter-department traffic? | Security Config §1.4 ("not set"), §3.3 ("PT Enforced: No"), Limitations §1–2 | ACLs actively restrict cross-dept traffic | ❌ **FAIL** | **F-GAP-02** |
| **NS-03** | DMZ Isolation | NIS2 Art. 21 | Is the DMZ isolated from internal networks via firewall rules? | Security Config §3.3, §10; Limitations §3 | DMZ ACL enforces, firewall blocks internal→DMZ | ❌ **PARTIAL** | **F-GAP-03** |

---

## AREA 2: ACCESS CONTROL (3 checks)

| ID | Control | Framework | Question | Evidence | Pass Condition | Result | Finding? |
|----|---------|-----------|----------|----------|---|---|---|
| **AC-01** | Encrypted Management | ISO 27001 A.8.5 | Is all management access (switch, router) encrypted (SSH)? | Device Config §1-6, SSH shown only on Leaf-1, Leaf-2, Edge Router | SSH on all Layer 3 devices (8 total); Telnet disabled | ❌ **PARTIAL** | **F-GAP-04** |
| **AC-02** | Authentication Framework | ISO 27001 A.8.5, NIS2 21(2)(i) | Is centralized authentication (AAA/RADIUS) configured and working? | Security Config §7.4 ("RADIUS does not function"), Limitations §4 (command rejected) | RADIUS authenticates users; local fallback operational | ❌ **FAIL** | **F-GAP-05** |
| **AC-03** | Guest Network Restriction | ISO 27001 A.8.22 | Are guest users isolated from internal networks? | GUEST-ACL tested in Security Config §2.2 | Guest VLAN isolated; ACLs block internal access | ✅ **PASS** | No |

---

## AREA 3: LOGGING & MONITORING (3 checks)

| ID | Control | Framework | Question | Evidence | Pass Condition | Result | Finding? |
|----|---------|-----------|----------|----------|---|---|---|
| **LM-01** | Centralized Logging | ISO 27001 A.8.15, NIS2 21(2)(b) | Are logs from network devices sent to central location? | Security Config §2.2 ("only Leaf-1"), Limitations §6 (switches rejected) | All Layer 3 devices forward logs to Syslog server | ❌ **FAIL** | **F-GAP-01** |
| **LM-02** | Firewall Logging | ISO 27001 A.8.16, NIS2 21(2)(b) | Does the firewall log inbound/outbound traffic and forward to Syslog? | Security Config §3.2, Limitations §7 (ASA logging rejected) | Firewall logs configured and forwarded | ❌ **FAIL** | **F-GAP-01** |
| **LM-03** | Log Retention Policy | GDPR Art. 32, NIS2 21(2)(c) | Is there a documented log retention policy (minimum 90 days)? | No retention period in any document | Policy documented (e.g., 90+ days) | ❌ **FAIL** | **F-GAP-06** |

---

## AREA 4: DATA PROTECTION (3 checks)

| ID | Control | Framework | Question | Evidence | Pass Condition | Result | Finding? |
|----|---------|-----------|----------|----------|---|---|---|
| **DP-01** | Personal Data Inventory | GDPR Art. 30, ISO 27001 A.5.9 | Is personal data storage documented? | No data classification or inventory in dossier | Server VLAN identified; data types listed | ❌ **FAIL** | **F-GAP-07** |
| **DP-02** | Backup & Recovery | GDPR Art. 32, ISO 27001 A.8.13 | Are critical servers backed up and restore tested? | Testing Report: "backup not simulated"; iSCSI = "Bonus" | iSCSI or backup solution with restore test | ❌ **FAIL** | **F-GAP-07** |
| **DP-03** | Incident Response | GDPR Art. 33, NIS2 Art. 21(2)(b) | Is incident response procedure documented (24h/72h notification)? | Contract §6 ("Mon-Fri 08-18 only"); no 24/7 IR plan | Procedure for 24h notification, breach investigation | ❌ **FAIL** | **F-GAP-08** |

---

## AREA 5: HIGH AVAILABILITY & SUPPLY CHAIN (3 checks)

| ID | Control | Framework | Question | Evidence | Pass Condition | Result | Finding? |
|----|---------|-----------|----------|----------|---|---|---|
| **HA-01** | Redundancy | ISO 27001 A.8.14 | Is there redundancy in the routing layer (dual spines, ECMP)? | Device Config §1-2, Network Impl. Report §4.1.2 | Dual spine switches with OSPF/ECMP | ✅ **PASS** | No |
| **HA-02** | Internet Gateway Redundancy | ISO 27001 A.8.14, NIS2 21(2)(c) | Is there failover for Internet access? | Device Config §7-8 (single router, single ASA) | Dual edge routers or documented failover | ❌ **FAIL** | **F-GAP-09** |
| **SC-01** | Vendor Security | ISO 27001 A.5.19, NIS2 21(2)(d) | Are vendor security requirements in contract? | Contract §2-6 (no security criteria, audit rights, timeline); Cost Breakdown (refurbished hardware option) | Security clauses, audit rights, incident SLA | ❌ **FAIL** | **F-GAP-10** |

---

## Coverage Summary

| Area | Checks | Pass | Partial | Fail | Total |
|------|--------|------|---------|------|-------|
| **Network Segmentation** | 3 | 1 | 1 | 1 | 3 |
| **Access Control** | 3 | 1 | 1 | 1 | 3 |
| **Logging & Monitoring** | 3 | 0 | 0 | 3 | 3 |
| **Data Protection** | 3 | 0 | 0 | 3 | 3 |
| **High Availability & Supply Chain** | 3 | 1 | 0 | 2 | 3 |
| **TOTAL** | 15 | 3 | 2 | 10 | 15 |

**Pass Rate:** 3/15 (20%) | **Fail Rate:** 10/15 (67%) | **Partial:** 2/15 (13%)

---

## Findings Raised

| ID | Title | Check(s) | Priority | Risk Score |
|----|-------|----------|----------|---|
| **F-GAP-01** | Centralized logging incomplete; firewall/switches not forwarding | LM-01, LM-02 | CRITICAL | 3×3 = 9 |
| **F-GAP-02** | Department ACLs configured but not enforced | NS-02 | HIGH | 2×3 = 6 |
| **F-GAP-03** | DMZ isolation depends on unenforced ACL; two-interface firewall limits control | NS-03 | HIGH | 2×3 = 6 |
| **F-GAP-04** | SSH management not deployed to 5 of 8 Layer 3 devices | AC-01 | HIGH | 2×3 = 6 |
| **F-GAP-05** | RADIUS non-functional; local credentials only; weak key material | AC-02 | HIGH | 2×3 = 6 |
| **F-GAP-06** | No log retention policy documented | LM-03 | MEDIUM | 2×2 = 4 |
| **F-GAP-07** | No backup design, data inventory, or restore testing | DP-01, DP-02 | MEDIUM | 2×2 = 4 |
| **F-GAP-08** | No 24/7 incident response; support only Mon-Fri 08-18 | DP-03 | MEDIUM | 2×2 = 4 |
| **F-GAP-09** | Single points of failure at Internet edge (1 router, 1 firewall, 1 Syslog) | HA-02 | MEDIUM | 1×3 = 3 |
| **F-GAP-10** | No security requirements in supplier contract; refurbished hardware option | SC-01 | MEDIUM | 2×2 = 4 |

---

## Notes

- **Packet Tracer Limitations:** Dossier documents that SVI ACL enforcement (Catalyst 3650), RADIUS server commands, Syslog on switches/ASA, 802.1X, and iSCSI are not simulated. All findings account for these gaps.
- **Evidence Quality:** The dossier contradicts itself (Syslog server IP 203.0.113.6 vs 192.168.70.16; "RADIUS PASS" vs "does not function"; "all devices" vs "Leaf-1 only"). See F-GAP-11 (not raised here) for dossier consistency.
- **Tested Controls:** NS-01 (VLAN design), AC-03 (guest isolation), HA-01 (dual spine) have supporting evidence and pass the checklist.