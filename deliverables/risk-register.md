# Risk Register — NVIDIA Regional R&D Hub GRC Audit

**Project:** NVIDIA Regional R&D and Production Hub  
**Project ID:** NVIDIA-REG-RD-2026-1106  
**Audit Status:** Findings Assessed  

---

## Rating Scale

### Likelihood (How Probable?)

| Level | Score | Definition |
|-------|-------|-----------|
| **High** | 3 | No controls prevent it; common technique; easy to exploit |
| **Medium** | 2 | Some controls exist but partial or can be bypassed |
| **Low** | 1 | Rare; requires special conditions; insider access needed |

### Impact (How Bad?)

| Level | Score | Definition |
|-------|-------|-----------|
| **High** | 3 | Affects sensitive data, operations, or business continuity |
| **Medium** | 2 | Affects one sector or one service; moderate recovery time |
| **Low** | 1 | Limited effect; easily recovered; non-sensitive data |

### Risk Score

| Likelihood | Impact | Score | Rating |
|-----------|--------|-------|--------|
| 3 | 3 | 9 | **CRITICAL** |
| 3 | 2 | 6 | **HIGH** |
| 3 | 1 | 3 | **MEDIUM** |
| 2 | 3 | 6 | **HIGH** |
| 2 | 2 | 4 | **MEDIUM** |
| 2 | 1 | 2 | **LOW** |
| 1 | 3 | 3 | **MEDIUM** |
| 1 | 2 | 2 | **LOW** |
| 1 | 1 | 1 | **LOW** |

---

## Risk Summary

| Rating | Count | % of Total | Priority |
|--------|-------|-----------|----------|
| **CRITICAL** | 1 | 33% | ⚠️ Fix Immediately |
| **HIGH** | 1 | 33% | ⚠️ Fix Within 30 Days |
| **MEDIUM** | 1 | 33% | ✓ Fix Within 90 Days |
| **LOW** | 0 | 0% | ✓ Nice to Have |
| **TOTAL** | **3** | **100%** | |

---

## Risk Register (Sorted by Score - Highest First)

| ID | Finding | Area | L | I | Score | Rating | Owner | Target Date | Status |
|----|---------|------|---|---|-------|--------|-------|-------------|--------|
| F-02 | Firewall Syslog Not Functional | Logging | 3 | 3 | **9** | **CRITICAL** | Security | 30 Sep 2026 | Not Started |
| F-01 | VLAN 1 Not Hardened | Segmentation | 2 | 2 | **4** | **MEDIUM** | Network | 30 Oct 2026 | Not Started |
| F-03 | Access Switches No Logging | Logging | 2 | 2 | **4** | **MEDIUM** | Network | 30 Oct 2026 | Not Started |

---

## Critical Risk Details (Score: 9)

### F-02: Firewall Syslog Not Functional

**Status:** CRITICAL | **Timeline:** 30 days | **Owner:** Security Team

**Why Critical:**
- Firewall is perimeter security for entire network
- DMZ servers (HTTP, FTP) completely unmonitored for attacks
- Internet-based attacks undetectable
- Violates NIS2 Article 21(2)(h) — "incident detection"
- Violates GDPR Article 32 — "security measures"

**Impact if not fixed:**
- DMZ compromise undetectable
- Cannot meet 24-hour NIS2 breach notification requirement
- Regulatory violation (CCB, GDPR authority audit)

**Remediation:** See AUDIT_F02 for detailed fix

---

## High Risks (Score: 6)

Currently: None identified

---

## Medium Risks (Score: 4)

### F-01: VLAN 1 Not Hardened
- **Reason:** Secondary to F-02; VLAN 1 not critical but creates hopping vector
- **Remediation:** Remove VLAN 1 from trunk allowed lists

### F-03: Access Switches No Logging  
- **Reason:** Helpful for incident investigation but less critical than firewall
- **Remediation:** Configure logging on all 6 access switches

---

## Low Risks (Score: 2)

Currently: None identified

---

## Top 3 Priorities

### Priority #1 (CRITICAL)
**F-02: Firewall Syslog Not Functional**
- **What:** Firewall not sending logs to Syslog server
- **Why:** DMZ completely unmonitored; cannot detect internet-based attacks
- **Who:** Security Team
- **Fix:** Troubleshoot and implement Syslog or alternative logging solution

### Priority #2 (MEDIUM)
**F-01: VLAN 1 Not Hardened**
- **What:** VLAN 1 not removed from trunks
- **Why:** Creates VLAN hopping attack vector
- **Who:** Network Team
- **Fix:** Remove VLAN 1 from all trunk "allowed vlan" lists

### Priority #3 (MEDIUM)
**F-03: Access Switches No Logging**
- **What:** 6 access switches not sending logs to Syslog
- **Why:** Cannot detect workstation-level compromise or policy violations
- **Who:** Network Team
- **Fix:** Configure logging on all 6 access switches

---