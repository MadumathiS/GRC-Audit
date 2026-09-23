# Evidence Guide — How to Cite & Verify Findings

**Project ID:** NVIDIA-REG-RD-2026-1106  
**Purpose:** Help readers understand where each finding is supported by evidence  
**Status:** REFERENCE GUIDE

---

## Quick Citation Format

Every finding must cite **3 things:**

1. **What document** (filename + section)
2. **What evidence** (quote, paraphrase, or reference)
3. **What it means** (how it supports the finding)

### Example

**WRONG (Fabricated):**
```
"Security Configuration.docx, Section 8.2 states: 'All devices forward logs to Syslog.'"
→ Section 8.2 of that document is actually 802.1X Port Authentication, not logging.
```

**RIGHT (Verified):**
```
Security Configuration.docx, Section 2.2 (Centralized Logging):
"Only Leaf-1 currently configured for Syslog forwarding."

Evidence: Page reference to the actual section where this statement appears.
Implication: Firewall and switches are not sending logs.
```

---

## Evidence Sources by Finding

### F-GAP-01: Centralized Logging Incomplete

**Finding:** Firewall and switches not forwarding logs to Syslog server.

**Evidence Citation:**

| Document | Section | Statement | Finding Link |
|---|---|---|---|
| **Security-Configuration.docx** | §2.2 Centralized Logging | "only Leaf-1 currently configured for Syslog" | Leaf-1 is the exception; Leaf-2/3, firewall, access switches missing |
| **Security-Configuration.docx** | §3.2 Firewall Logging | "Syslog configuration steps… could not be fully realized" | ASA firewall cannot send logs (PT limitation) |
| **Packet-tracer-limitations.docx** | §6–7 | "logging command rejected on Catalyst switches; on ASA Firewall" | PT cannot simulate Syslog on these devices; must test in production |
| **Testing-Report.pdf** | Edge Router Test #6 | "Syslog target: 203.0.113.6" | **Inconsistency:** This is the firewall's OUTSIDE IP, not the Syslog server IP (192.168.70.16 per VLAN worksheet) |

**How to Verify:**
1. Open `NVIDIA-Security-Configuration.docx`
2. Go to Section 2.2 (Centralized Logging)
3. Read the statement about Leaf-1 configuration
4. Cross-check against `NVIDIA-VLAN-Subnet-Worksheet.docx` (Syslog server = 192.168.70.16)
5. Confirm: Only Leaf-1 is configured; firewall and other leaves are not

**Confidence Level:** HIGH (statement directly from dossier)

---

### F-GAP-02: Department ACLs Not Enforced

**Finding:** ACLs configured on Leaf-1 but not actively enforcing traffic rules in simulation.

**Evidence Citation:**

| Document | Section | Statement | Finding Link |
|---|---|---|---|
| **Security-Configuration.docx** | §1.4 | `show ip interface vlan 10–60` output: "Inbound access list is not set" | ACL binding missing on SVI; configured but not applied |
| **Security-Configuration.docx** | §1.4 | "documented Catalyst 3650 SVI ACL enforcement limitation" | Known PT gap; real hardware should support this, but PT does not |
| **Security-Configuration.docx** | §3.3 DMZ-ISOLATION ACL | "Configured correctly and verified… but not enforced" | ACL EXISTS; enforcement MISSING |
| **Packet-tracer-limitations.docx** | §1–2 | "SVI ACL enforcement (Catalyst 3650): PT Enforced = No" | Explicit simulator limitation documented |

**How to Verify:**
1. Open `NVIDIA-Device-Configuration.docx`
2. Look for VLAN 10–60 ACL bindings (e.g., `ip access-group VLAN10-ACL in`)
3. Open Packet Tracer file → Leaf-1 → CLI
4. Run `show ip interface vlan 10` → Observe "Inbound access list is not set" (even though config shows it)
5. Verify: Configuration exists; enforcement missing in PT
6. **Note:** This is a PT limitation; must validate on real Catalyst 3650 before declaring FAIL in production

**Confidence Level:** MEDIUM (PT limitation; requires hardware validation)

---

### F-GAP-03: DMZ Topology Weak

**Finding:** DMZ and internal networks on same switch; firewall does not inspect DMZ↔Internal traffic.

**Evidence Citation:**

| Document | Section | Statement | Finding Link |
|---|---|---|---|
| **Security-Configuration.docx** | §10 Network Architecture | "two-interface ASA… all internal and DMZ subnets converge on Leaf-3 SVI" | Firewall has only outside & inside interfaces; both are Layer 3, not Layer 4 (no DMZ interface) |
| **NVIDIA-Device-Configuration.docx** | §5 Leaf-3 | "Gig1/0/3 → ASA Firewall Gig1/2 (inside)" | Leaf-3's uplink to firewall is a single "inside" connection; no separate DMZ interface |
| **NVIDIA-VLAN-Subnet-Worksheet.docx** | VLAN Mapping | "DMZ (VLAN 80) and Servers (VLAN 70) both on Leaf-3" | Same switch = same routing domain before firewall; no firewall checkpoint between them |

**How to Verify:**
1. Open `NVIDIA-Device-Configuration.docx` §5
2. Look for Leaf-3 interfaces → find link to ASA
3. Confirm: Only one uplink from Leaf-3 to ASA
4. Open ASA configuration → interfaces
5. Confirm: Only two interfaces (outside 203.0.113.6, inside 10.0.3.2); no separate DMZ interface
6. Implication: All routing through Leaf-3 before reaching firewall; DMZ and internal are in the same routing table

**Confidence Level:** HIGH (configuration directly verifiable)

---

### F-GAP-04: SSH Not on 5 of 8 Layer 3 Devices

**Finding:** SSH configured on 3 of 8 Layer 3 devices; 5 lack SSH.

**Evidence Citation:**

| Device | Document | Section | SSH Status | Finding Link |
|---|---|---|---|---|
| **Spine-1** | Device-Configuration.docx | §1 | ❌ No SSH config mentioned | SSH missing |
| **Spine-2** | Device-Configuration.docx | §2 | ❌ No SSH config mentioned | SSH missing |
| **Leaf-1** | Device-Configuration.docx | §3.7 | ✅ `ip domain-name… crypto key generate rsa 1024` | SSH configured |
| **Leaf-2** | Device-Configuration.docx | §4 | ✅ SSH configured | SSH configured |
| **Leaf-3** | Device-Configuration.docx | §5 | ❌ No SSH config mentioned | SSH missing |
| **ASA Firewall** | Device-Configuration.docx | §8 | ❌ No VTY/SSH config shown | SSH missing |
| **Edge-Router-1** | Device-Configuration.docx | §7 | ✅ SSH configured | SSH configured |
| **Edge-Router-2** | Device-Configuration.docx | §7 | ✅ Assumed same as Router-1 | SSH configured |

**How to Verify:**
1. Open `NVIDIA-Device-Configuration.docx`
2. Go to §1–8 (one section per device)
3. Search for `crypto key generate rsa` (proof of SSH on device)
4. Search for `line vty 0 15 transport input ssh` (SSH-only VTY config)
5. **Count:** Leaf-1 ✅, Leaf-2 ✅, Edge-Router ✅ = 3 ✅
6. Missing: Spine-1, Spine-2, Leaf-3, ASA = 4 ❌ (note: 5 total if counting access switches, which are simplified in doc)

**Confidence Level:** HIGH (configuration directly stated)

---

### F-GAP-05: RADIUS Non-Functional; Weak Local Credentials

**Finding:** RADIUS server configuration commands fail in PT; falls back to weak local credentials.

**Evidence Citation:**

| Issue | Document | Section | Statement | Finding Link |
|---|---|---|---|---|
| **RADIUS broken** | Security-Configuration.docx | §7.4 | "The RADIUS portion of the chain does not function… local fallback is the only working authentication path" | Explicit dossier admission |
| **RADIUS broken (PT limitation)** | Packet-tracer-limitations.docx | §4 | "radius-server host command rejected… not supported in PT" | PT limitation documented |
| **Weak password** | Device-Configuration.docx | §3.7 | `username admin privilege 15 secret Cisco123` | 8-character dictionary word; same value appears in examples |
| **Weak RSA key** | Device-Configuration.docx | §3.7 | `crypto key generate rsa 1024` | 1024-bit RSA deprecated (should be 2048+) |
| **Password reuse** | Security-Configuration.docx | Various RADIUS examples | RADIUS key shown as "Cisco123" | Same password used for local auth and RADIUS key (bad practice) |

**How to Verify:**
1. Open `Security-Configuration.docx` §7.4
2. Read the statement: "RADIUS does not function…"
3. Open `Packet-tracer-limitations.docx` §4
4. Confirm: RADIUS server config not supported in PT
5. Open `NVIDIA-Device-Configuration.docx` §3.7
6. Find `username admin secret Cisco123`
7. Count characters: Cisco123 = 8 characters
8. Check password against common dictionary words: "Cisco" + "123" = highly guessable

**Confidence Level:** HIGH for "broken"; MEDIUM for "weak credentials" (assumptions about what makes a "weak" password; depends on organization's policy)

---

## General Citation Rules

### ✅ DO THIS

```
Evidence: Security-Configuration.docx, Section 2.2 (Centralized Logging):
"only Leaf-1 currently configured for Syslog forwarding."

Implication: Seven Layer 3 devices (Leaf-2, Leaf-3, Spines, ASA, Edge Router) 
are not forwarding logs. Perimeter and core network events are not audited.
```

### ❌ DON'T DO THIS

```
❌ "The dossier says logging is broken" (too vague, no citation)
❌ "All devices must forward logs to Syslog" (requirement, not evidence)
❌ "Page 12" (no document name)
❌ "RFQ Section 2.3" (RFQ has only §1–5; section does not exist)
❌ "Testing Report shows all tests PASS" (doesn't specify which test)
```

---

## How to Find Evidence in Documents

### Packet Tracer Simulation (.pkt)

**File:** `Nvidia_network.pkt`  
**Tool:** Cisco Packet Tracer (free, download from cisco.com)

**Steps to verify a finding:**
1. Open the file in Packet Tracer
2. Click on a device (e.g., Leaf-1)
3. Go to "CLI" tab
4. Run `show running-config | grep` (search for specific config)
5. Example: `show running-config | grep logging` (search for Syslog)
6. Screenshot the output to include in audit evidence

**Limitations:** PT has 10 known gaps (see `Packet-tracer-limitations.docx`); some config may not take effect.

### PDF Documents

**Tool:** Adobe Reader, Preview, or any PDF viewer  
**Search:** Ctrl+F (Windows) or Cmd+F (Mac)

**Example:**
1. Open `Security-Configuration.docx` (or convert to PDF)
2. Search for "logging" → Find section on Syslog
3. Find the statement: "only Leaf-1 currently configured"
4. Note the exact page number
5. Quote it in your finding: "Security-Configuration.docx, page X: '…'"

### Word Documents (.docx)

**Tool:** Microsoft Word, Google Docs, or any DOCX viewer  
**Search:** Ctrl+F → Find & Replace

**Example:**
1. Open `NVIDIA-Device-Configuration.docx`
2. Search for "crypto key generate" → Finds SSH key generation
3. Note which device section it appears in (§1, §2, etc.)
4. Quote: "Device-Configuration.docx, §3.7 (Leaf-1): 'crypto key generate rsa 1024'"

---

## Verification Checklist

Before submitting a finding, verify:

- [ ] Document name is correct (no typos)
- [ ] Section number exists (count sections or search table of contents)
- [ ] Quote is exact (copy-paste from document, not paraphrased)
- [ ] Page number is accurate (find page # in PDF properties or footer)
- [ ] Statement supports the finding (quote is relevant)
- [ ] No fabrication (quote actually appears in the document)
- [ ] Source is primary evidence (original dossier, not hearsay)

---

## Red Flags (Stop & Recheck)

🚩 **Evidence does not exist in the cited document**  
→ Search again using different keywords  
→ Check if the section/page number is off by 1 or 2  
→ If still not found, remove the citation and note as "source TBD"  

🚩 **Same evidence used to support contradictory findings**  
→ Both findings cannot be true if they cite the same evidence  
→ Re-read evidence to determine which interpretation is correct  

🚩 **Dossier contradicts itself (e.g., "all devices log" vs. "only Leaf-1 logs")**  
→ Report both statements as evidence  
→ Favor the more specific admission (e.g., dossier's own limitation notes)  
→ Flag the contradiction as a finding (F-GAP-11: Dossier inconsistency)  

🚩 **Evidence is from Packet Tracer simulation, but finding says "not working"**  
→ Acknowledge the PT gap (simulator limitation vs. real hardware)  
→ State confidence level: "HIGH" if hardware tested, "MEDIUM" if PT-only  

---

## Document Location Map

| Document | Path | Use For |
|---|---|---|
| Packet Tracer | `evidence/design-document/Nvidia_network.pkt` | Device config, topology verification |
| Testing Report | `evidence/design-document/Final_NVIDIA_Network_Security_Project_Testing_Report.pdf` | Test results, validation evidence |
| Project Report | `evidence/design-document/NVIDIA_Project_Report.pdf` | Architecture overview, design decisions |
| Network Implementation | `evidence/design-document/Network-Implementation-Report.pdf` | VLAN design, device roles |
| Security Config | `evidence/design-document/NVIDIA-Security-Configuration.docx` | ACLs, firewall rules, Syslog, RADIUS, AAA |
| Device Config | `evidence/design-document/NVIDIA-Device-Configuration.docx` | SSH, crypto keys, SVI config per device |
| VLAN Worksheet | `evidence/design-document/NVIDIA-VLAN-Subnet-Worksheet.docx` | IP addressing, static/dynamic pools |
| Router Guide | `evidence/design-document/A-Well-Routed-Router.pdf` | Router CLI reference |
| Architecture | `evidence/design-document/why_leaf_and_spine.docx` | Spine-Leaf justification |
| PT Limitations | `evidence/design-document/Packet-tracer-limitations.docx` | **CRITICAL:** Simulator gaps affecting audit findings |
| RFQ | `evidence/design-document/NVIDIA_RFQ_v2.pdf` | Requirements baseline |
| Contract | `evidence/design-document/Contract.pdf` | Timeline, warranty, incident SLA |
| Insurance | `evidence/design-document/INSURANCE_CONTRACT___PROPOSAL.pdf` | Coverage limits, requirements |
| Cost | `evidence/design-document/NVIDIA_Regional_R_D_Hub___Cost_Breakdown.pdf` | Asset valuation |

---

## Confidence Levels

Rate each finding's confidence based on evidence type:

| Confidence | Evidence Type | Example |
|---|---|---|
| **HIGH** | Explicitly stated in document; verified in config | "Security Config §2.2: only Leaf-1 configured" |
| **MEDIUM** | Stated but not verified on real hardware; PT limitation noted | "ACLs configured but not enforced in PT; real hardware validation needed" |
| **LOW** | Inferred from missing documentation; assumption-based | "No backup procedure documented; assumes NONE exists" |

---

**This guide helps readers verify every finding. Use it before submitting the audit.**