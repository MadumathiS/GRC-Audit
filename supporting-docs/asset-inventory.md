# Asset Inventory — NVIDIA Regional R&D Hub

**Project ID:** NVIDIA-REG-RD-2026-1106  
**Inventory Date:** September 2026  
**Location:** Brussels, Belgium (Regional R&D and Production Hub)  
**Status:** FINAL

---

## Executive Summary

The NVIDIA Regional R&D Hub consists of:

- **12 Network Devices** (Spine switches, Leaf switches, routers, firewall)
- **6 Access Switches** (one per department/sector)
- **9 Servers** (compute, storage, authentication, logging, DNS, DHCP)
- **48 Workstations** (standard: HP Z4 G4, ~€1,095 each)
- **10 GPU Workstations** (production: GPX-WS 740 RTP, ~€7,499 each)
- **1 Firewall** (Cisco ASA, perimeter security)
- **2 Edge Routers** (Internet gateway, WAN termination)
- **Estimated Equipment Value:** €264–294k (new hardware + 5-year warranty)

---

## 1. NETWORK INFRASTRUCTURE

### 1.1 Core Fabric (Spine-Leaf Architecture)

#### Spine Layer (Layer 3 Routing)

| Device | Model | VLAN Support | Ports | Uplink | Role | IP Address |
|---|---|---|---|---|---|---|
| **Spine-1** | Cisco C9300-24T-E | OSPF only, no VLANs | 24 × 10G + 4 × 40G | N/A | Primary core router | 10.0.1.x |
| **Spine-2** | Cisco C9300-24T-E | OSPF only, no VLANs | 24 × 10G + 4 × 40G | N/A | Secondary core router (ECMP) | 10.0.2.x |

**Function:** OSPF ECMP routing between leaves. Fixed 2-hop topology (any leaf to any leaf = 2 hops via spine).

#### Leaf Layer (Layer 3 Access & Termination)

| Device | Model | VLANs Hosted | Ports | P2P Uplink to Spine | Role | IP Address |
|---|---|---|---|---|---|---|
| **Leaf-1 (COMPUTE)** | Cisco C9300-24T-E | 10, 20, 30, 40, 50, 60, 99 (7 VLANs) | 24 × 10G | Spine-1, Spine-2 | Compute workstations; guest VLAN | 10.0.1.0/30 |
| **Leaf-2 (SERVICES)** | Cisco C9300-24T-E | 70, 90 (2 VLANs) | 24 × 10G | Spine-1, Spine-2 | Internal servers; iSCSI storage | 10.0.1.4/30 |
| **Leaf-3 (EDGE)** | Cisco C9300-24T-E | 80 (1 VLAN) | 24 × 10G | Spine-1, Spine-2 + Firewall | DMZ; internet gateway | 10.0.3.0/30 |

**Function:** SVI (Switch Virtual Interface) for inter-VLAN routing, default gateway per VLAN.

### 1.2 Access Layer (Department/Sector)

| Access Switch | Model | Ports | Connected To (Leaf) | Departments/Groups | Workstations |
|---|---|---|---|---|---|
| **Access-1** | Cisco C9200-48T-E | 48 × 1G | Leaf-1 | IT Department | 5 |
| **Access-2** | Cisco C9200-48T-E | 48 × 1G | Leaf-1 | Production (GPU) | 10 (GPU) |
| **Access-3** | Cisco C9200-48T-E | 48 × 1G | Leaf-1 | Management | 5 + 1 (spare) |
| **Access-4** | Cisco C9200-48T-E | 48 × 1G | Leaf-1 | Support Sector 1 | 10 + 1 (spare) |
| **Access-5** | Cisco C9200-48T-E | 48 × 1G | Leaf-1 | Support Sector 2 | 10 + 1 (spare) |
| **Access-6** | Cisco C9200-48T-E | 48 × 1G | Leaf-1 | Study/Training | 8 + 1 (spare) |

**Function:** Access ports for workstations; trunk to Leaf-1 (allowed VLANs per department).

---

## 2. SECURITY PERIMETER

### 2.1 Firewall

| Device | Model | Interfaces | Security Level | Function | IP Addresses |
|---|---|---|---|---|---|
| **ASA Firewall** | Cisco Firepower 1120 | Gig1/1 (outside), Gig1/2 (inside) | 0 (outside), 100 (inside) | Perimeter security, NAT/PAT, DMZ filtering | Outside: 203.0.113.6; Inside: 10.0.3.2 |

**Security Policies:**
- Outside-in ACL: HTTP/HTTPS to DMZ-Web (192.168.80.10), FTP to DMZ-FTP (192.168.80.11)
- Inside-out: Implicit allow (dynamic PAT for internet access)
- DMZ-internal: Blocked (firewall enforces separation)

### 2.2 Edge Routers (Internet Gateway)

| Device | Model | Ports | Function | IP Addresses |
|---|---|---|---|---|
| **Edge-Router-1** | Cisco ISR4331 | 2 × GigE, 1 × Serial | Primary internet uplink; default route origination (OSPF) | WAN: 203.0.113.1; LAN: 10.0.3.x |
| **Edge-Router-2** | Cisco ISR4331 | 2 × GigE, 1 × Serial | Backup internet (standby, not active) | WAN: 203.0.113.x; LAN: 10.0.3.x |

**Status:** Single active router (Edge-Router-1). Edge-Router-2 is **not configured for failover** (FINDING F-GAP-09: Single point of failure).

---

## 3. SERVERS (VLAN 70 & 80)

### 3.1 Internal Servers (VLAN 70 — Management VLAN)

| Server | Model | CPU | RAM | Function | IP Address | Port | Notes |
|---|---|---|---|---|---|---|---|
| **DNS** | Dell R450 | Intel Xeon | 32 GB | DNS service; domain name resolution | 192.168.70.10 | Static | Downstream: All workstations via DHCP |
| **DHCP** | Dell R450 | Intel Xeon | 32 GB | DHCP server; address pool allocation | 192.168.70.11 | Static | Pools by VLAN; gateway via ip helper-address |
| **AAA/RADIUS** | Dell R450 | Intel Xeon | 32 GB | Authentication, Authorization, Accounting (RADIUS) | 192.168.70.12 | Static | BROKEN: RADIUS not functional in PT (F-GAP-05) |
| **Central-Compute-LLM** | Dell R750 | Intel Xeon Platinum | 128 GB | Compute workload; LLM/AI training(?) | 192.168.70.14 | Static | **Unclear purpose — see AI Act risk in applicability note** |
| **Internal-FTP** | Dell R550 | Intel Xeon Gold | 64 GB | File transfer server (internal only) | 192.168.70.15 | Static | Cleartext FTP; no SFTP (F-GAP-06: HIGH) |
| **Syslog** | Dell R450 | Intel Xeon | 32 GB | Centralized logging; syslog aggregation | 192.168.70.16 | UDP 514 | **CRITICAL GAP: Only Leaf-1 forwards logs; firewall/switches missing (F-GAP-01)** |
| **(Reserved)** | — | — | — | (Future use) | 192.168.70.17+ | — | — |

**VLAN 70 Security:** Static IPs only. Gateway: 192.168.70.254. No DHCP on this VLAN.

### 3.2 DMZ Servers (VLAN 80)

| Server | Model | Function | IP Address | Port | Exposed To |
|---|---|---|---|---|---|
| **DMZ-Web** | Dell R550 | HTTP/HTTPS web server | 192.168.80.10 | 80, 443 | Internet (ASA allows) |
| **DMZ-FTP** | Dell R550 | FTP server (cleartext) | 192.168.80.11 | 20, 21 | Internet (ASA allows) |

**VLAN 80 Security:** Static IPs only. Gateway: 192.168.80.254. Isolated from internal (VLAN 70) by firewall.

**Risk:** DMZ and internal on same Leaf-3 switch (F-GAP-03: HIGH — firewall does not inspect internal↔DMZ traffic).

### 3.3 Storage (iSCSI)

| Server | Model | Function | IP Address | Port | Storage Type |
|---|---|---|---|---|---|
| **iSCSI-Storage** | Dell R750 | Network-attached storage (block-level) | 192.168.90.10 | 3260 | 1 TB SAS SSD (indicated) |

**VLAN 90 Isolation:** Only servers on VLAN 70 can access iSCSI (Leaf-2 gateway).

**Status:** Not simulated in Packet Tracer. Backup procedure **not tested** (F-GAP-07: MEDIUM).

---

## 4. WORKSTATIONS & CLIENT DEVICES

### 4.1 Standard Workstations

| Department | VLAN | Model | Count | Role | Data Classification |
|---|---|---|---|---|---|
| **IT Department** | 10 | HP Z4 G4 | 5 | Network admin, scripting | Internal networks, passwords (SENSITIVE) |
| **Production (Non-GPU)** | 20 | HP Z4 G4 | — | (Not used for production) | — |
| **Management** | 30 | HP Z4 G4 | 5 | Managers, project planning | Project files, contractor data (INTERNAL) |
| **Support Sector 1** | 40 | HP Z4 G4 | 10 | Level-1/2 support staff | User data, troubleshooting logs (INTERNAL) |
| **Support Sector 2** | 50 | HP Z4 G4 | 10 | Level-1/2 support staff | Same as above |
| **Study/Training** | 60 | HP Z4 G4 | 8 | R&D engineers, testing | Experimental results, model data (CONFIDENTIAL) |
| **Guest/Contractor** | 99 | (BYOD) | — | Visiting engineers, consultants | Restricted access (internet only) |
| **Total Standard WS** | — | HP Z4 G4 | **48** | — | — |

**Unit Cost:** ~€1,095 each (without monitor/peripherals)

### 4.2 GPU-Accelerated Workstations

| Department | VLAN | Model | Count | Role | GPU Type | Data Handling |
|---|---|---|---|---|---|---|
| **Production (GPU)** | 20 | GPX-WS 740 RTP | 10 | Model training, inference, rendering | NVIDIA A100 (80GB VRAM) | Proprietary models (CONFIDENTIAL) |

**Unit Cost:** ~€7,499 each

**Storage:** Local NVMe + iSCSI access for checkpointing large models.

### 4.3 Access Points (Implicit)

| Network | SSID | Security | Role | Notes |
|---|---|---|---|---|
| **Guest Wi-Fi** | NVIDIA-Guest | WPA2-PSK | Contractor/visitor access | Bridged to VLAN 99 |
| **Management Wi-Fi** | NVIDIA-MGMT (Implied) | WPA2-Enterprise | Not in dossier scope | — |

---

## 5. VLAN & IP ADDRESSING SCHEME

### 5.1 VLAN Allocations

| VLAN | Name | Subnet | Gateway | Scope | Primary Leaf | Access Switch |
|---|---|---|---|---|---|---|
| **10** | IT Department | 192.168.10.0/24 | .254 | 5 WS + IT infrastructure | Leaf-1 | Access-1 |
| **20** | Production | 192.168.20.0/24 | .254 | 10 GPU WS + compute nodes | Leaf-1 | Access-2 |
| **30** | Management | 192.168.30.0/24 | .254 | 5 management WS | Leaf-1 | Access-3 |
| **40** | Support Sector 1 | 192.168.40.0/24 | .254 | 10 support staff | Leaf-1 | Access-4 |
| **50** | Support Sector 2 | 192.168.50.0/24 | .254 | 10 support staff | Leaf-1 | Access-5 |
| **60** | Study/Training | 192.168.60.0/24 | .254 | 8 engineers | Leaf-1 | Access-6 |
| **70** | Servers (internal) | 192.168.70.0/24 | .254 | DNS, DHCP, AAA, Syslog, Compute, FTP | Leaf-2 | Direct on Leaf-2 |
| **80** | DMZ | 192.168.80.0/24 | .254 | Web, FTP, public-facing | Leaf-3 | Direct on Leaf-3 |
| **90** | iSCSI Storage | 192.168.90.0/24 | .254 | Storage network (block-level) | Leaf-2 | Direct on Leaf-2 |
| **99** | Guest | 192.168.99.0/24 | .254 | Visitor/contractor access (internet only) | Leaf-1 | Direct on Leaf-1 |
| **999** | Unused Ports | — | — | Blackhole for non-trunked ports | All | All (native VLAN) |

### 5.2 Static IP Assignments

**Servers (VLAN 70):**
- 192.168.70.10: DNS
- 192.168.70.11: DHCP
- 192.168.70.12: RADIUS/AAA
- 192.168.70.14: LLM-Compute
- 192.168.70.15: Internal-FTP
- 192.168.70.16: Syslog server

**DMZ (VLAN 80):**
- 192.168.80.10: Web server
- 192.168.80.11: FTP server

**iSCSI (VLAN 90):**
- 192.168.90.10: Storage controller

### 5.3 DHCP Pools (Dynamic)

| VLAN | Pool Range | Gateway | DNS Server | Lease Time |
|---|---|---|---|---|
| **10–60** (departments) | .20 to .253 | .254 | 192.168.70.10 | 24 hours |
| **99** (guest) | .20 to .253 | .254 | 192.168.70.10 | 2 hours |
| **70, 80, 90** | None (static only) | — | — | — |

---

## 6. DATA CLASSIFICATION & SENSITIVE ASSETS

### 6.1 Data Flows

| Source | Destination | Data Type | Sensitivity | Volume | Encryption |
|---|---|---|---|---|---|
| **Production WS (VLAN 20)** → **iSCSI (VLAN 90)** | GPU workloads → Model checkpoints | AI models, training data | CONFIDENTIAL | ~10 TB/week | None (internal) |
| **GPU WS (VLAN 20)** → **LLM-Compute (VLAN 70)** | Results → compute server | Model artifacts, metrics | CONFIDENTIAL | ~1 TB/day | None |
| **IT (VLAN 10)** → **All SVIs (Leaf-1/2/3)** | Management traffic | Config commands, passwords | SENSITIVE | Intermittent | **SSH on Leaf-1/2 only; NOT on Spines/Leaf-3/ASA (F-GAP-04)** |
| **All WS (VLAN 10–60)** → **AAA (VLAN 70)** | Authentication | Usernames, passwords | SENSITIVE | Per-login | **RADIUS broken; falls back to local auth (F-GAP-05)** |
| **All devices** → **Syslog (192.168.70.16)** | System logs | Device events, config changes, auth attempts | INTERNAL | ~100 MB/day | **Only Leaf-1 forwards; firewall/switches missing (F-GAP-01)** |
| **Internet** ↔ **DMZ (VLAN 80)** | Web/FTP traffic | Public-facing services | PUBLIC | ~50 GB/day | HTTP (80), HTTPS (443), FTP (21) — **FTP cleartext (F-GAP-06)** |

### 6.2 Personal Data Handling

**Types:** Employee network IDs, usernames, IP addresses, login timestamps (in logs).

**Storage Locations:**
- AAA server (192.168.70.12) — User credentials, RADIUS logs
- Syslog server (192.168.70.16) — Authentication events with usernames/IPs
- Access switches (logs in memory) — Failed login attempts, port security events

**Data Retention:** Not documented (FINDING: No retention policy, F-GAP-06 MEDIUM).

**Access Controls:** VLAN 70 protected by inter-VLAN ACLs (not enforced, F-GAP-02). Management traffic via SSH on Leaf-1/2 only (incomplete, F-GAP-04).

---

## 7. COMPLIANCE MAPPING

### 7.1 Assets by Regulatory Framework

| Asset | ISO 27001 Control | GDPR Obligation | NIS2 Requirement | CyFun Category |
|---|---|---|---|---|
| **Network (Spines, Leaves)** | A.8.22 Segmentation | Art. 32 integrity/confidentiality | Art. 21(2)(h) cryptography | PR.DS-1 data protection |
| **Firewall (ASA)** | A.8.20 Network security | Art. 32 technical measures | Art. 21(2)(b/h) incident detection + encryption | PR.AC-1 access control |
| **Servers (VLAN 70)** | A.8.2 Privileged access | Art. 30 DPA; Art. 32 pseudonymization | Art. 21(2)(d) supply chain + audit | PR.AC-7 authentication |
| **Syslog** | A.8.15 Logging | Art. 33 breach detection | Art. 21(2)(b) incident response | DE.AE-1 monitoring |
| **Workstations** | A.6.2 Asset management | Art. 32 endpoint security | Art. 21(2)(i) access control | PR.AC-1 access control |

### 7.2 Criticality Assessment

| Asset | Criticality | Impact if Compromised |
|---|---|---|
| **Spines** | HIGH | Network down; no routing; all services offline |
| **Leaf-3/Firewall** | HIGH | Internet access lost; DMZ exposed; all traffic inspectable |
| **Syslog Server** | HIGH | Audit trail lost; cannot detect incidents; violates NIS2 24h SLA |
| **RADIUS/AAA** | HIGH | Authentication fails; all device access uses weak local credentials |
| **iSCSI Storage** | MEDIUM-HIGH | Model checkpoints lost; training pipeline delayed |
| **GPU Workstations** | MEDIUM | Model training/inference capability degraded; not critical for operations |
| **Standard Workstations** | MEDIUM | Staff productivity reduced; not critical for infrastructure |

---

## 8. ASSET VALUATION & INSURANCE

### 8.1 Hardware Costs (from RFQ/Cost Breakdown)

| Category | New Cost | Refurbished Cost | Unit Count |
|---|---|---|---|
| **Core Fabric (Spines + Leaves)** | €20,516 | €4,688 | 5 switches |
| **Edge/Access (Routers, Firewall, Access switches)** | €15,854 | €9,691 | 9 devices |
| **Servers** | €39,163 | €30,166 | 9 units |
| **Workstations** | €124,410 | €53,530 | 48 standard + 10 GPU |
| **Infrastructure (cabling, racks, UPS)** | €5,774 | €3,582 | — |
| **Licensing & add-ons** | €30,100 | €30,100 | — |
| **Labour (6-person, 8 days)** | €28,900 | €28,900 | — |
| **TOTAL** | **€264,716** | **€160,657** | — |

### 8.2 Insurance Coverage

| Carrier | Coverage | Annual Premium | Coverage Amount | Exclusions |
|---|---|---|---|---|
| **AXA Belgium** | Hardware & Electronics | €1,800 | €350k–€600k | Cyber attacks, negligence |
| **Hiscox Europe** | Cyber Risk + E&O | €2,200 | €500k–€1m each | War, nuclear, terrorism |
| **TOTAL** | | **€4,000/year** | **€850k–€1.6m** | — |

**Cyber Minimum Requirements:**
- MFA on all admin accounts
- Patch management (critical: 7–14 days)
- EDR on workstations
- Network segmentation (VLANs)
- Daily backups with offline copy
- Email security (DMARC/DKIM/SPF)
- Incident runbooks

**Status:** VLANs deployed ✅. All other requirements **not documented in dossier** (F-GAP-07, F-GAP-08: MEDIUM).

---

## 9. ASSET INVENTORY AUDIT GAPS

| Gap | Finding | Impact | Remediation |
|---|---|---|---|
| **No centralized logging** | F-GAP-01 CRITICAL | Cannot track asset access/changes; audit trail missing | Enable Syslog on firewall/switches |
| **No backup design** | F-GAP-07 MEDIUM | iSCSI storage not protected; recovery capability unknown | Design backup strategy; test restore |
| **No asset inventory documentation** | Scope limitation | Cannot audit data retention, encryption, ownership | Create formal asset registry |
| **No incident response plan** | F-GAP-08 MEDIUM | Breach investigation & notification delayed; violates NIS2 24h SLA | Draft incident response runbook |
| **Unclear LLM/AI workload** | Risk (not finding) | AI Act compliance unknown; high-risk AI possible | Clarify Central-Compute-LLM purpose & scope |

---

## 10. Summary Table: All Assets

| Device Category | Count | Model(s) | Primary Function | Audit Status |
|---|---|---|---|---|
| **Core Switches** | 2 | Cisco C9300-24T-E | OSPF routing | ✅ Tested |
| **Access Switches** | 3 | Cisco C9300-24T-E | Leaf SVIs + VLAN termination | ✅ Tested |
| **Access Switches** | 6 | Cisco C9200-48T-E | Department uplinks | ✅ Tested |
| **Firewall** | 1 | Cisco ASA Firepower 1120 | Perimeter security | 🟡 Partial (logging not working) |
| **Routers** | 2 | Cisco ISR4331 | Internet gateway | 🟡 Partial (no failover) |
| **Servers** | 9 | Dell R450/R550/R750 | DNS, DHCP, AAA, Compute, FTP, Syslog, Storage | 🟡 Partial (RADIUS broken, iSCSI untested) |
| **Workstations** | 48 | HP Z4 G4 | Department computing | ✅ Assumed |
| **GPU Workstations** | 10 | GPX-WS 740 RTP | AI model training | ✅ Assumed |

---

**Document Status:** FINAL | **Inventory Date:**  September 2026 | **Asset Count:** 82 devices