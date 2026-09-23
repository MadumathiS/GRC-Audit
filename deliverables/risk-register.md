# Risk Register — NVIDIA Regional R&D Hub 

**Project:** NVIDIA Regional R&D Hub  

---

## Risk Rating Scale

### Likelihood (How Probable?)

| Level | Score | Definition |
|-------|-------|-----------|
| **High** | 3 | No controls prevent it; common technique; easy to exploit in PT environment |
| **Medium** | 2 | Some controls exist but partial or bypass-able; simulation limitations mask gaps |
| **Low** | 1 | Rare; requires special conditions or insider access; mitigated by other controls |

### Impact (How Bad?)

| Level | Score | Definition |
|-------|-------|-----------|
| **High** | 3 | Affects confidentiality/integrity of sensitive data, operations, or audit capability |
| **Medium** | 2 | Affects one sector, one service, or single device; moderate recovery time |
| **Low** | 1 | Limited effect; easily recovered; non-critical systems only |

### Risk Score = Likelihood × Impact

| Score | Rating | Action |
|-------|--------|--------|
| **9** | **CRITICAL** | ⚠️ Fix immediately (days); block production go-live if unresolved |
| **6** | **HIGH** | ⚠️ Fix within 30 days; plan remediation before deployment |
| **4** | **MEDIUM** | ✓ Fix within 90 days; monitor during production |
| **2–3** | **LOW** | ✓ Nice to have; include in maintenance plan |
| **1** | **LOW** | ✓ Accept risk; document rationale |

---

## Risk Register (Sorted by Score — Highest First)

| # | Finding | Check(s) | Area | L | I | Score | Rating | Owner | Remediation | Target | Status |
|---|---------|----------|------|---|---|-------|--------|-------|-------------|--------|--------|
| **1** | Centralized logging incomplete; firewall/switches not monitored | LM-01, LM-02 | Logging | 3 | 3 | **9** | **CRITICAL** | Sec/Network | Deploy Syslog on 7 Layer 3 devices + ASA | Production go-live | Not started |
| **2** | Department ACLs configured but not enforced | NS-02 | Segmentation | 2 | 3 | **6** | **HIGH** | Network | Verify hardware ACL behavior; re-architecture if needed | 14 days (pre-go-live) | Not started |
| **3** | DMZ topology weak; internal↔DMZ traffic not firewalled | NS-03 | Segmentation | 2 | 3 | **6** | **HIGH** | Contractor | Add 3rd firewall interface OR re-architect to in-line firewall | 21 days | Not started |
| **4** | SSH management not on 5 of 8 Layer 3 devices; Telnet exposed | AC-01 | Access Control | 2 | 3 | **6** | **HIGH** | Network | Enable SSH on Spines, Leaf-3, ASA, access switches | 7 days (quick fix) | Not started |
| **5** | RADIUS non-functional; local credentials only; RSA 1024 weak | AC-02 | Access Control | 2 | 3 | **6** | **HIGH** | Security | Deploy real RADIUS server; upgrade RSA to 2048+; enforce strong local passwords | Production + 30 days | Not started |
| **6** | No log retention policy documented | LM-03 | Logging | 2 | 2 | **4** | **MEDIUM** | Sec/Ops | Document retention policy (minimum 90 days); configure log aging | 30 days | Not started |
| **7** | No backup design or restore testing | DP-02 | Data Protection | 2 | 2 | **4** | **MEDIUM** | Ops | Design iSCSI backup strategy; test restore procedure | 45 days | Not started |
| **8** | No 24/7 incident response procedure; support Mon-Fri 08-18 only | DP-03 | Data Protection | 2 | 2 | **4** | **MEDIUM** | Security | Draft 24/7 breach response plan; arrange on-call coverage | 60 days | Not started |
| **9** | Single points of failure at Internet edge (1 router, 1 firewall) | HA-02 | High Availability | 1 | 3 | **3** | **MEDIUM** | Network | Plan dual edge router + firewall failover (future) | 6 months (post-launch) | Accepted |
| **10** | Vendor security not addressed in contract | SC-01 | Supply Chain | 2 | 2 | **4** | **MEDIUM** | Procurement | Add security criteria, audit rights, 24h incident SLA to contract | Before contract signature | Not started |
| **11** | Dossier internally inconsistent (contradicting claims, wrong IPs, unclear requirements) | (All) | Quality | 2 | 2 | **4** | **MEDIUM** | Contractor | Validate dossier against actual .pkt config before signing | Before acceptance | Not started |
| **12** | Internet-facing FTP service uses cleartext transfer and a default-style account (`cisco`/`cisco`, delete/rename rights) — see `findings/F-GAP-12-FTP-Cleartext-Default-Credentials.md` | (Additional finding) | Data Protection | 3 | 2 | **6** | **HIGH** | Network/Security | Migrate DMZ-FTP to SFTP; remove `cisco` account; restrict ASA ACL to the secure port only | Before go-live | Not started |

---

## Priority Actions (Top 3)

### **Priority #1 — CRITICAL: Logging Incomplete (F-GAP-01)**
- **What:** Firewall and switches not sending logs to Syslog server.
- **Why:** Perimeter security and core network events are invisible. Cannot detect attacks or comply with NIS2/GDPR incident notification requirements (24h/72h need audit trail).
- **Who:** Network team (Leaf/spine config) + Security team (Syslog server setup).
- **Fix:** Configure `logging 192.168.70.16` on ASA and all 8 Layer 3 switches. Deploy Syslog server with 90-day retention.
- **Timeline:** 3–5 days (pre-go-live).
- **Cost:** Included in current contract.

### **Priority #2 — HIGH: DMZ Architecture Weak (F-GAP-03)**
- **What:** DMZ and internal networks on same switch; firewall does not inspect DMZ↔internal traffic.
- **Why:** Compromised DMZ server has direct access to all internal subnets. No checkpoint to block lateral movement.
- **Who:** Contractor (architecture); Network team (implementation).
- **Fix:** Option A (preferred): Add 3rd interface to ASA between DMZ and internal. Option B: Move DMZ to separate access switch with firewall in-line.
- **Timeline:** 10–14 days (requires hardware procurement and config change).
- **Cost:** ~€5–10k for ASA upgrade OR additional switching hardware.
- **Dependency:** Must resolve before production go-live (blocks risk acceptance).

### **Priority #3 — HIGH: Authentication Weak (F-GAP-05)**
- **What:** RADIUS does not work. Local credentials are weak (8 chars, dictionary word, publicly visible in examples).
- **Why:** Attacker can crack local admin password and compromise all Layer 3 devices.
- **Who:** Security team (credential policy) + Network team (implementation).
- **Fix:** Deploy real RADIUS server in production. Enforce strong local credentials (12+ chars, unique per device). Update RSA to 2048+.
- **Timeline:** RADIUS must be ready before go-live. Local credential upgrade within 30 days post-deployment.
- **Cost:** RADIUS hardware already in budget. Strong credential rotation no additional cost.

---

## Risk Summary

| Rating | Count | % of Total | Timeline |
|--------|-------|-----------|----------|
| **CRITICAL (9)** | 1 | 8% | ⚠️ Block production if unresolved |
| **HIGH (6)** | 5 | 42% | ⚠️ Must resolve within 30 days |
| **MEDIUM (4)** | 5 | 42% | ✓ Resolve within 90 days |
| **LOW (≤3)** | 1 | 8% | ✓ Accept/defer; plan fix |
| **TOTAL** | **12** | **100%** | |

*(Updated to include F-GAP-12, added as a supplementary finding — see `findings/F-GAP-12-FTP-Cleartext-Default-Credentials.md`.)*

---

## Limitations & Caveats

1. **Packet Tracer Simulation Gap:** The dossier documents known PT limitations:
   - SVI ACL enforcement (Catalyst 3650) inconsistent
   - RADIUS server configuration not supported
   - Syslog not supported on switches/ASA
   - iSCSI protocol not simulated
   - 802.1X not persistent
   
   **Implication:** 4 of the 5 HIGH findings (F-GAP-02, F-GAP-03, F-GAP-05 RADIUS part, LM-01/02) assume that PT accurately represents real hardware behavior, which may not be true. **All findings must be re-validated on production hardware before go-live.**

2. **Dossier Contradictions:** The evidence contains internal inconsistencies (Syslog IPs, RADIUS status, VLAN counts). These are flagged in F-GAP-11 (not detailed here) and should be resolved before production acceptance.

3. **RFQ vs. Dossier:** The RFQ asks for "Access Control Matrix" (defining inter-departmental traffic rules) and performance testing. These deliverables are not found in the dossier. **Recommendation:** Contractor should provide these documents, or update the requirements to remove them.

4. **Risk Rating Consistency:** The scale (Likelihood 1–3, Impact 1–3) is applied uniformly across all findings. Auditors may reasonably rate the same gap differently (e.g., the Spines are not user-facing; failure has lower impact than Leaf-1); the important thing is to be explicit about the rationale.

---

## Acceptance Criteria for Go-Live

**Before production deployment, the following must be demonstrated:**

✅ **F-GAP-01 (CRITICAL):** Syslog server receives logs from all 8 Layer 3 devices + ASA.  
✅ **F-GAP-02 (HIGH):** ACL enforcement verified on real Catalyst hardware; if not enforced, firewall in-line workaround deployed.  
✅ **F-GAP-03 (HIGH):** Three-interface firewall OR alternate architecture deployed and tested.  
✅ **F-GAP-04 (HIGH):** SSH on all 8 Layer 3 devices; Telnet disabled.  
✅ **F-GAP-05 (HIGH):** RADIUS server operational; local credentials upgraded (12+ chars, unique).  
✅ **F-GAP-12 (HIGH):** DMZ-FTP migrated to SFTP; `cisco` account removed; ASA ACL restricted to the secure port only.  

**Can defer post-deployment (within 30–90 days):**  
- ✓ F-GAP-06 (log retention policy)  
- ✓ F-GAP-07 (backup design & testing)  
- ✓ F-GAP-08 (incident response procedure)  
- ✓ F-GAP-10 (vendor security clauses)  

---