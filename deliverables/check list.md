# Audit Checklist — NVIDIA Regional R&D Hub 

**Project:** NVIDIA Regional R&D and Production Hub  
**Project ID:** NVIDIA-REG-RD-2026-1106  
**Team:** [Your Team Name]  
**Date:** 15 September 2026  
**Status:** Final  

---

## Overview

15 focused controls across 5 audit areas. Each check maps to a regulatory requirement identified in Step 1.

---

## CHECKLIST

### AREA 1: NETWORK SEGMENTATION (3 checks)

| ID | Control | Framework | Yes/No Question | Evidence Source | Pass Condition | ✓/✗ |
|----|---------|-----------|---|---|---|---|
| **NS-01** | VLAN Isolation | ISO 27001 A.8.1 | Are workstations in different departments separated by different VLANs? | Design doc: VLAN plan | 10+ separate VLANs defined, one per department/function | |
| **NS-02** | Access Control Lists | ISO 27001 A.8.2 | Do ACLs on Layer 3 switches prevent inter-department traffic? | Design doc: Security config | Department VLANs have ACLs denying cross-traffic | |
| **NS-03** | DMZ Isolation | NIS2 Article 21 | Is the DMZ (public services) isolated from internal networks? | Design doc: Firewall config | DMZ on separate VLAN; firewall restricts inbound/outbound | |

---

### AREA 2: ACCESS CONTROL (3 checks)

| ID | Control | Framework | Yes/No Question | Evidence Source | Pass Condition | ✓/✗ |
|----|---------|-----------|---|---|---|---|
| **AC-01** | Encrypted Management | ISO 27001 A.9.4 | Is all management access (switch, router) encrypted (SSH)? | Device configs | SSH enabled, Telnet disabled on all Layer 3 devices | |
| **AC-02** | Authentication Framework | CyFun PR.AC | Is centralized authentication (AAA) configured? | Design doc: Server config | RADIUS server deployed; devices configured to use it | |
| **AC-03** | Guest Network Restriction | ISO 27001 A.8.1 | Are guest users restricted from accessing internal networks? | Design doc: ACLs, VLAN plan | Guest VLAN isolated; ACLs deny internal access | |

---

### AREA 3: LOGGING & MONITORING (3 checks)

| ID | Control | Framework | Yes/No Question | Evidence Source | Pass Condition | ✓/✗ |
|----|---------|-----------|---|---|---|---|
| **LM-01** | Centralized Logging | ISO 27001 A.12.4 | Are logs from network devices sent to a central location? | Design doc: Server config, device config | Syslog server deployed; devices configured to log | |
| **LM-02** | Firewall Logging | NIS2 Article 21(h) | Does the firewall log inbound/outbound traffic? | Design doc: Security config | Firewall configured to log; logs sent to Syslog | |
| **LM-03** | Log Retention | GDPR Article 32 | Is there a documented log retention policy? | Design doc: Policies or Risk Register | Retention period specified (e.g., 90 days) | |

---

### AREA 4: DATA PROTECTION (3 checks)

| ID | Control | Framework | Yes/No Question | Evidence Source | Pass Condition | ✓/✗ |
|----|---------|-----------|---|---|---|---|
| **DP-01** | Personal Data Inventory | GDPR Article 30 | Is personal data (employee info) stored and are storage locations documented? | Design doc: Server config, RFQ | Server VLAN identified; data types listed | |
| **DP-02** | Backup & Recovery | GDPR Article 32 | Are critical servers backed up? | Design doc: Infrastructure | iSCSI storage or backup solution documented | |
| **DP-03** | Incident Response | NIS2 Article 21 | Is an incident response procedure documented? | Risk Register or appendix | Breach notification steps defined (72h to APD/GBA) | |

---

### AREA 5: HIGH AVAILABILITY & SUPPLY CHAIN (3 checks)

| ID | Control | Framework | Yes/No Question | Evidence Source | Pass Condition | ✓/✗ |
|----|---------|-----------|---|---|---|---|
| **HA-01** | Redundancy | ISO 27001 A.14.2 | Is there redundancy in the routing layer? | Design doc: Network topology | Dual spine switches with OSPF/ECMP documented | |
| **HA-02** | Internet Gateway Redundancy | NIS2 Article 21 | Is there redundancy for Internet access? | Design doc: Network topology | Dual edge routers or failover mechanism documented | |
| **SC-01** | Vendor Security | ISO 27001 A.15.1 | Are third-party dependencies (vendors) documented? | RFQ, design doc, contract | Vendors listed; security requirements noted | |

---

## Summary

| Area | Checks | Status |
|------|--------|--------|
| Network Segmentation | 3 | |
| Access Control | 3 | |
| Logging & Monitoring | 3 | |
| Data Protection | 3 | |
| High Availability & Supply Chain | 3 | |
| **TOTAL** | **15** | |

---