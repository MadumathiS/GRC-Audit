# Asset Inventory — NVIDIA Regional R&D Hub (Step 1)

**Project:** NVIDIA Regional R&D and Production Hub  
**Project ID:** NVIDIA-REG-RD-2026-1106  
**Team:**   

---

## Asset Inventory (Single Table)

| Asset Type | Count | VLAN(s) | Function | Data Classification | Critical? |
|-----------|-------|---------|----------|---------------------|-----------|
| **Core Switches** | 2 | N/A (inter-switch) | Spine routing backbone (OSPF) | Infrastructure | YES |
| **Distribution Switches** | 3 | N/A (inter-switch) | Leaf: departments, servers, DMZ | Infrastructure | YES |
| **Access Switches** | 6 | 10-60, 99 | Department workstation connectivity | Infrastructure | YES |
| **Workstations** | 48 | 10, 20, 30, 40, 50, 60 | End-user devices (6 departments) | Research, Admin, Operational | YES |
| **Servers - DNS** | 1 | 70 | Name resolution (nvidia.local) | Infrastructure | YES |
| **Servers - DHCP** | 1 | 70 | IP address allocation (all VLANs) | Infrastructure | YES |
| **Servers - AAA/RADIUS** | 1 | 70 | Authentication, employee credentials (PII) | Employee Data (GDPR) | YES |
| **Servers - Syslog** | 1 | 70 | Centralized logging | Security/Audit Data | YES |
| **Servers - iSCSI Storage** | 1 | 90 | Block storage for backups/archival | Research, Backup Data | YES |
| **Servers - Internal FTP** | 1 | 70 | Internal file transfer | Research/IP | YES |
| **Servers - Compute (AI/GPU)** | 1 | 70 | AI/ML workload execution | Research/IP | YES |
| **Edge Router** | 1 | N/A | Internet gateway (edge layer) | Infrastructure | YES |
| **Firewall (ASA)** | 1 | 80 (DMZ) | Perimeter security, NAT/PAT | Infrastructure | YES |
| **DMZ Web Server** | 1 | 80 | Public website hosting | Public Data | NO |
| **DMZ FTP Server** | 1 | 80 | Public file distribution | Public Data | NO |

---

## Data Assets by Location

| Data Type | VLAN | Server/Location | Classification | Regulation |
|-----------|------|-----------------|-----------------|-----------|
| Employee personal data (name, ID, salary) | 70 | AAA/RADIUS Server (192.168.70.12) | GDPR Personal Data | GDPR, NIS2 |
| R&D intellectual property | 70, 20 | Internal-FTP + Compute Server | Confidential | NIS2 |
| Production artifacts | 20 | Production workstations + iSCSI | Confidential | NIS2 |
| System logs | 70 | Syslog server (192.168.70.16) | Security/Audit | GDPR, NIS2 |
| Financial records | 30 | Management workstations | Confidential | GDPR |

---

## VLANs & Departments

| VLAN | Name | Department | Workstations | Purpose |
|------|------|-----------|---|---------|
| 10 | IT | IT Support | 5 | Administrative support |
| 20 | Production | Production (High-End) | 10 | Multimedia/GPU workload |
| 30 | Management | Management/Secretariat | 5 | Office/admin |
| 40 | Support-1 | Support Sector 1 | 11 | General support |
| 50 | Support-2 | Support Sector 2 | 11 | General support |
| 60 | Study | Study/R&D | 8 | Research/training |
| 70 | Servers | Infrastructure | 9 servers | DNS, DHCP, AAA, Syslog, storage, compute |
| 80 | DMZ | Public-Facing | 2 servers | Web, FTP (internet-accessible) |
| 90 | iSCSI | Storage | 1 server | Block storage (backup) |
| 99 | Guest | Visitor Network | Dynamic | Guest access (restricted) |

---

## Summary

- **Total Network Devices:** 20 (2 spine + 3 leaf + 6 access + 1 router + 1 firewall + 7 servers)
- **Total Workstations:** 48 (across 6 departments)
- **Total VLANs:** 10
- **Critical Assets:** 16 (all infrastructure + AAA + Syslog)
- **Personal Data Location:** VLAN 70 (AAA server)
- **Public Services (DMZ):** VLAN 80 (2 servers)
- **High-Security Areas:** Production (VLAN 20), Management (VLAN 30)

---
