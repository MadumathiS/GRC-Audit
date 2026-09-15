# Evidence Citation Guide

**Project:** GRC-Audit (NVIDIA Regional R&D Hub)  
**Purpose:** Define how evidence is cited and traced to specific sources  
**Effective Date:** September 2026  
**Status:** Final

---

## Overview

All findings must be traced to **specific, verifiable evidence**. This guide defines citation formats for your project's evidence sources.

---

## Evidence Sources in This Audit

### 1️⃣ Design & Implementation Documentation

**Files in `evidence/design-document/`:**
- `Nvidia_network.pkt` (Packet Tracer simulation)
- `Network-Implementation-Report.pdf`
- `NVIDIA-Device-Configuration.docx`
- `NVIDIA-Security-Configuration.docx`
- `NVIDIA-VLAN-Subnet-Worksheet.docx`
- `A_Well_Routed_Router.pdf` (reference guide)
- `why_leaf_and_spine.docx`
- `Packet_tracer_limitations.docx`

**Format:** `[File Name], [Location] | [Specific Detail]`

**Examples:**

- `Packet Tracer file (Nvidia_network.pkt), Device: Core-Spine-1, Interface Gi0/1 | OSPF adjacency with Leaf-1 confirmed, metric 1`

- `Network-Implementation-Report.pdf, Section 3 (VLAN Configuration), p.4 | "Each VLAN received its own subnet and default gateway"`

- `NVIDIA-Security-Configuration.docx, Section 8.1 (Network Security) | "ACL-based segmentation, VLAN isolation, DMZ separation, Guest network restriction"`

- `Packet Tracer file (Nvidia_network.pkt), Device: IT-Access-Switch, Ports Fa0/1-5 | Assigned to VLAN 10, PortFast enabled`

**How to cite:**
1. Write the file name
2. Identify the exact location: Device name + interface, Section #, or page #
3. Provide the specific configuration detail or quote

---

### 2️⃣ Test Reports (Evidence of Control Validation)

**Files in `evidence/design-document/test-reports/`:**
- `Final-NVIDIA-Network-Security-Project-Testing-Report.pdf` (comprehensive)
- `Syslog-Logging-Test-Report.pdf`
- `RADIUS-Server-Test-Report.pdf`
- `ASA-Firewall-Testing-Report.pdf`
- `DMZ-Security-Validation-Report.pdf`
- `STP-Security-BPDU-Guard-Root-Guard-Validation-Report.pdf`
- `Spine-Switch-Interface-Status-Port-Test-Report.pdf`
- `Leaf-Switch-Interface-Status-Port-Test-Report.pdf`
- `Access-Switch-Port-Testing-Report.pdf`
- `Endpoint-Test-Report.pdf`
- `Edge-Router-Security-Test-Report.pdf`
- `Guest-VLAN-Connectivity-Test-Report.pdf`
- `NAT-Test-Report.pdf`
- `IP-Spoofing-Test-Report.pdf`
- `Internet-Connectivity-Test-Report.pdf`
- `FTP-Server-Test-Report.pdf`
- `DMZ-Isolation-ACL-Validation-Report.pdf`
- `ACL-VLAN-Security-Validation-Report.pdf`

**Format:** `[Test Report Name], [Test Section], Result: [PASS/FAIL]`

**Examples:**

- `Syslog-Logging-Test-Report.pdf, "Syslog Captures" (Figure 8) | Privilege Elevation: "%SYS-5-PRIV_AUTH_PASS confirms privilege level was set to 15 (administrative access)" | Result: PASS`

- `RADIUS-Server-Test-Report.pdf, "Successful SSH Login (RADIUS User)" (Figure 4) | SSH login using user 'sabir' authenticated through RADIUS server | Result: PASS`

- `ASA-Firewall-Testing-Report.pdf, Section 4 (Access Control List Verification) | "Configured ACL permits HTTP, HTTPS and FTP access to the published DMZ servers while restricting other unsolicited traffic" | Result: PASS`

- `STP-Security-BPDU-Guard-Root-Guard-Validation-Report.pdf, Figure 2 | "PortFast BPDU Guard Default is enabled and interface configuration includes 'spanning-tree bpduguard enable'" | Result: PASS`

- `NAT-Test-Report.pdf, Results table | "Dynamic PAT for inside users is operational. Static NAT for DMZ web server is operational" | Result: PASS`

- `DMZ-Isolation-ACL-Validation-Report.pdf, "DMZ to Internal Test" (Figure) | "DMZ server received 'Destination host unreachable' when pinging the internal host (192.168.10.21) after ACL applied" | Result: PASS (control working)`

**How to cite:**
1. Test report name
2. Section/Figure reference
3. Specific test result or screenshot evidence
4. Pass/Fail status

---

### 3️⃣ RFQ & Requirements Documentation

**Files in `evidence/RFQ/`:**
- `NVIDIA-RFQ-master.pdf`
- `Technical-Scope-Requirements.pdf`
- `Security-Requirements.pdf`

**Format:** `RFQ, §[Section] | [Title] | "[Exact Requirement]"`

**Examples:**

- `RFQ, §2.1 | Network Architecture Design | "Scalable Spine-Leaf architecture"`

- `RFQ, §2.3 | Security Implementation | "The Contractor shall deploy and configure: Hardware firewall solution, Access Control Lists (ACLs), VLAN-based DMZ architecture"`

- `RFQ, §2.4 | Network Sectors | "The infrastructure shall support: Management/Secretariat (5 workstations), Production (10), Support Sector A (10), Support Sector B (10), Study (8), IT Department (5)"`

- `RFQ, §2.5 | Production Environment | "High-performance systems suitable for: Multimedia production, AI development, Machine Learning workloads, Large Language Model (LLM) training"`

**How to cite:**
1. "RFQ"
2. Section number (§)
3. Section title
4. Direct quote of the requirement

---

### 4️⃣ Infrastructure & Cost Documentation

**Files in `evidence/design-document/`:**
- `NVIDIA-Regional-R-D-Hub-Cost-Breakdown.pdf`
- `Insurance-Contract-Proposal.pdf`

**Format:** `[Document Name], [Section/Table] | [Detail]`

**Examples:**

- `Cost-Breakdown.pdf, Section 1 (Leaf-Spine Core Fabric) | Cisco C9300-24T-E switches: 2 Spines + 3 Leaves (Leaf-1 COMPUTE, Leaf-2 SERVICES, Leaf-3 EDGE), Total: €20,515.76`

- `Insurance-Contract-Proposal.pdf, Section 1 (Introduction) | "AXA Belgium — Hardware & Electronics (€1,800/year); Hiscox Europe — Cyber Risk & Professional Liability (€2,200/year)"`

- `Insurance-Contract-Proposal.pdf, Section 2 (Coverage Summary) | "Coverage Amount: €350,000–€600,000 for 48 workstations, 10 high-end GPU systems, server room equipment"`

---

### 5️⃣ Configuration & Architecture Documentation

**Files in `evidence/design-document/`:**
- `NVIDIA-VLAN-Subnet-Worksheet.docx`
- `NVIDIA-Device-Configuration.docx`
- `A-Well-Routed-Router.pdf` (reference guide)
- `why-leaf-and-spine.docx`
- `Packet-tracer-limitations.docx`

**Format:** `[Document Name], [Section/Table] | [Configuration Detail]`

**Examples:**

- `NVIDIA-VLAN-Subnet-Worksheet.docx, VLAN Configuration Table | "VLAN 10 (IT Department): 192.168.10.0/24, Gateway: 192.168.10.254, 254 usable IPs"`

- `NVIDIA-VLAN-Subnet-Worksheet.docx, VLAN Configuration Table | "VLAN 80 (DMZ): 192.168.80.0/24, Gateway: 192.168.80.254, 254 usable IPs"`

- `why-leaf-and-spine.docx | Justification for spine-leaf architecture: "Fixed 2-hop communication model, OSPF ECMP redundancy, Scalable horizontal expansion"`

- `Packet-tracer-limitations.docx | "Partial ACL enforcement inconsistencies, No full AAA/RADIUS simulation, No iSCSI or AI workload execution"`

---

## Evidence Location Map

| Evidence Type | Directory | File(s) | What It Proves |
|---|---|---|---|
| **Network Design** | `design-document/` | `Nvidia_network.pkt`, `Network-Implementation-Report.pdf` | Architecture, routing, VLAN segmentation, security controls |
| **Security Config** | `design-document/` | `NVIDIA-Security-Configuration.docx` | ACLs, firewall rules, AAA, DMZ isolation |
| **Test Evidence** | `design-document/test-reports/` | All `*-Test-Report.pdf`, `*-Validation-Report.pdf` | Controls validated, security measures confirmed |
| **Requirements** | `RFQ/` | `NVIDIA-RFQ-master.pdf`, `Security-Requirements.pdf` | Baseline requirements for audit mapping |
| **Cost & Risk** | `design-document/` | `Cost-Breakdown.pdf`, `Insurance-Contract.pdf` | Asset valuation, risk mitigation strategy |
| **Infrastructure** | `design-document/` | `NVIDIA-VLAN-Subnet-Worksheet.docx`, `NVIDIA-Device-Configuration.docx` | IP addressing, device inventory |

---

## Real-World Citation Examples from This Audit

### Example 1: VLAN Segmentation Control

**Finding:** VLAN segmentation is properly implemented and verified.

**Evidence:**
```
1. Network-Implementation-Report.pdf, Section 3 (VLAN Configuration), p.4
   | "Each VLAN received its own subnet and default gateway"

2. Packet Tracer file (Nvidia_network.pkt), Device: Leaf-1, SVI VLAN 10
   | IP: 192.168.10.1, Subnet: 255.255.255.0, Gateway: 192.168.10.254

3. RFQ, §2.3 | Security Implementation
   | "VLAN-based DMZ architecture"

4. Access-Switch-Port-Testing-Report.pdf, Result | PASS
   | "Ports Fa0/1 through Fa0/5 are connected and assigned to VLAN 10"
```

---

### Example 2: Firewall & ACL Control

**Finding:** Firewall ACLs restrict unauthorized inbound traffic; DMZ is isolated.

**Evidence:**
```
1. NVIDIA-Security-Configuration.docx, Section 8.2 (Perimeter Security)
   | "NAT/PAT via firewall, DMZ exposure control, Firewall rules"

2. Packet Tracer file (Nvidia_network.pkt), Device: ASA Firewall, ACL OUTSIDE-IN
   | Lines: permit tcp any object WEB-SERVER eq www
   |        permit tcp any object WEB-SERVER eq 443

3. ASA-Firewall-Testing-Report.pdf, Section 4 (ACL Verification)
   | "The configured ACL permits HTTP, HTTPS and FTP access"
   | Result: PASS

4. DMZ-Security-Validation-Report.pdf, External Access Test
   | "An ISP router failed to reach an internal host, confirming that 
   |  direct external access is blocked"
   | Result: PASS
```

---

### Example 3: Logging & Monitoring Control

**Finding:** Centralized Syslog logging is implemented and operational.

**Evidence:**
```
1. RFQ, §2.3 | Security Implementation
   | "Syslog monitoring"

2. Network-Implementation-Report.pdf, Section 11 (Testing and Verification)
   | "Verification of switch management through VLAN 99"

3. Packet Tracer file (Nvidia_network.pkt), Device: Edge-Router, show logging
   | "Syslog logging enabled; router forwarding logs to 203.0.113.6:514"

4. Syslog-Logging-Test-Report.pdf, Figure 8 (Syslog Captures)
   | "%SYS-5-CONFIG_I indicates that configuration commands were executed"
   | "%SYS-5-PRIV_AUTH_PASS confirms the privilege level was set to 15"
   | Result: PASS
```

---

## What Counts as Valid Evidence

### ✅ Valid Evidence
- Specific quotes from `NVIDIA-Security-Configuration.docx` with section reference
- Configuration lines from `Packet Tracer file (Nvidia_network.pkt)` with device/interface name
- Specific test results from test reports with section/figure reference
- RFQ requirements with section number (§) and exact quote
- Policy settings with page numbers
- Test report results (PASS/FAIL) with supporting evidence

### ❌ Invalid Evidence
- "The design is secure" (no source)
- "Everyone knows firewalls block traffic" (cite the test result)
- "Probably configured correctly" (find proof in PKT or test report)
- Vague references: "Design Document mentions VLANs" (cite §5.1, p.9)
- "It should work like this" (show the test passed)

---

## Citation Checklist

Before writing a finding, verify the evidence:

- [ ] Can I point to the exact file in `evidence/`?
- [ ] Can I cite a specific section, page, or device name?
- [ ] Can someone else verify this by looking at the source?
- [ ] Is the quote exact or is it clearly a paraphrase?
- [ ] For PKT files: Can I name the device and interface?
- [ ] For test reports: Does it show PASS/FAIL result?
- [ ] For RFQ: Is the section number (§) correct?
- [ ] Does the evidence actually support the claim?

---

## How to Structure a Finding with Evidence

### Template

```markdown
# F-XX-[Control-Name]

**Title:** [What was tested/found]

**Description:** [1-2 sentence summary of the control]

**Finding Type:** Control Implementation / Best Practice / Observation

---

## Evidence

### 1. Requirement Baseline
**RFQ Requirement:**
`RFQ, §[X.X] | [Title] | "[Exact quote of requirement]"`

### 2. Design Implementation
**[Document Name]:**
`[Document Name], Section [X], p.[Y] | "[Quote or detail]"`

### 3. Technical Validation
**Packet Tracer Configuration:**
`Packet Tracer file (Nvidia_network.pkt), Device: [Name], [Config Detail]`

**Test Results:**
`[Test Report Name], [Section/Figure] | [Result/Observation] | Result: PASS/FAIL`

### 4. Compliance Mapping
| Standard | Control | Evidence |
|---|---|---|
| **ISO 27001** | A.X.X.X | [Specific evidence] |
| **GDPR** | Article X | [Specific evidence] |

---

## Control Assessment

✅ **EFFECTIVE** / ⚠️ **NEEDS REMEDIATION** / ❌ **NOT IMPLEMENTED**

**Justification:** [2-3 sentences explaining why]

---

## Risk Rating: LOW / MEDIUM / HIGH

**Rationale:** [1-2 sentences on impact]

---

## Auditor Notes

[Any caveats, simulator limitations, or follow-up items]

---

## Remediation Status

✅ **IMPLEMENTED** / 🔄 **IN PROGRESS** / ❌ **REQUIRES ACTION**

[Any required follow-up actions for production]
```

---

## Evidence Maintenance

### Before Committing Findings:

1. [ ] **Verify File Exists:** Is the evidence file in `evidence/` directory?
2. [ ] **Verify Content:** Can you open the file and find the exact quote/detail?
3. [ ] **Verify Citation:** Is section #, page #, or device name correct?
4. [ ] **Verify Logic:** Does evidence actually support the finding statement?

### After Audit Completion:

Keep `evidence/` folder updated:
- Keep all PDFs, DOCX, and PKT files
- Update `NOTES.md` if external links change
- Archive old findings with their evidence for record-keeping

---

## Questions?

If a finding references evidence but you can't find it:
1. Check `evidence/design-document/` for PDFs and PKT files
2. Check `evidence/RFQ/` for requirements
3. Check `evidence/NOTES.md` for external reference links
4. Refer to this guide's "Evidence Location Map" section

