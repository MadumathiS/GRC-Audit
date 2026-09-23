# Final Audit Report — NVIDIA Regional R&D and Production Hub

**Project ID:** NVIDIA-REG-RD-2026-1106  
**Prepared by:** Sajjad Shahpoor, Audit Coordinator (Risk & Reporting)  
**On behalf of:** Control Freaks (Madumathi Singaraju, Hanah Marroun, Sajjad Shahpoor)  
**Date:** September 2026  
**Status:** Final — for submission

---

## 1. Purpose of this report

This document is the closing deliverable of the audit: it consolidates the scope, applicability decision, checklist results, all findings, and the risk register into a single comprehensive summary for decision-makers who have not followed the audit day-to-day. It points to detailed evidence in supporting documents rather than repeating it.

## 2. What we audited, and how

This was a **design (documentation) audit** of the network delivered for NVIDIA's Regional R&D and Production Hub — not a penetration test. We compared the contractor's dossier (design documents, device configuration, Packet Tracer test report) against the RFQ and recognised frameworks using this chain:

**requirement → control → check → result → finding**

Full scope, in/out-of-scope areas, methodology and assumptions are in `evidence/supporting-docs/scope-statement.md`.

## 3. Regulatory applicability

Full reasoning is in `evidence/supporting-docs/applicability-note.md`. Summary:

| Regulation / Framework | Status |
|---|---|
| **GDPR** | ✅ Applies to hub's personal-data processing |
| **Belgian NIS2 Law** | ✅ Applies as Important entity (corrected scenario) |
| **ISO 27001:2022** | ✅ Used as audit criteria (not certification) |
| **CyberFundamentals 2025** | ✅ Used as audit framework |
| **Cyber Resilience Act** | ❓ Uncertain — insufficient evidence |
| **EU AI Act** | ❓ Uncertain — insufficient evidence |
| **DORA** | ❌ Does not apply |

## 4. Results at a glance

**Checklist (15 controls, 5 areas):** 3 Pass · 2 Partial · 10 Fail

**All 12 findings:**

| ID | Title | Rating |
|---|---|---|
| **F-GAP-01** | Centralised logging incomplete; firewall and switches not monitored | **CRITICAL** (9) |
| **F-GAP-02** | Department ACLs configured but not enforced | HIGH (6) |
| **F-GAP-03** | DMZ isolation depends on non-enforced ACL; two-interface firewall limits segmentation | HIGH (6) |
| **F-GAP-04** | SSH management not deployed to all Layer 3 devices | HIGH (6) |
| **F-GAP-05** | RADIUS authentication non-functional; weak local credentials | HIGH (6) |
| **F-GAP-06** | No log retention policy documented | MEDIUM (4) |
| **F-GAP-07** | No backup design or restore testing; personal-data inventory missing | MEDIUM (4) |
| **F-GAP-08** | No 24/7 incident-response procedure; support only Mon–Fri 08:00–18:00 | MEDIUM (4) |
| **F-GAP-09** | Single points of failure at Internet edge (1 router, 1 firewall, 1 Syslog server) | MEDIUM (3) |
| **F-GAP-10** | No security requirements in supplier contract | MEDIUM (4) |
| **F-GAP-11** | Dossier internally inconsistent (contradicting claims, wrong IPs, unclear requirements) | MEDIUM (4) |
| **F-GAP-12** | Internet-facing FTP service uses cleartext transfer and default-style account | HIGH (6) |
**Detailed write-ups:**
- F-GAP-01 to F-GAP-05: `deliverables/findings/F-01-Critical-Findings.md`
- F-GAP-06 to F-GAP-11: `deliverables/findings/F-GAP-06-to-11-Additional-Findings.md`
- F-GAP-12: `deliverables/findings/F-GAP-12-FTP-Cleartext-Default-Credentials.md`
- Full register: `deliverables/findings/risk-register.md`

## 5. Top priorities

The risk register identifies **F-GAP-01 (logging), F-GAP-03 (DMZ), and F-GAP-05 (authentication)** as top three. 

**F-GAP-12 (FTP)** carries the same HIGH rating and same before-go-live urgency — the account is exposed on the Internet today, over unencrypted protocol, with file delete/rename permissions — and should be actioned in the same wave rather than deferred.

## 6. Go-live acceptance criteria

**Before production deployment, NVIDIA must demonstrate:**

✅ **F-GAP-01 (CRITICAL):** Syslog server receives logs from all Layer 3 devices and ASA  
✅ **F-GAP-02 (HIGH):** ACL enforcement verified on real hardware (or in-line firewall workaround deployed)  
✅ **F-GAP-03 (HIGH):** Three-interface firewall or equivalent architecture deployed and tested  
✅ **F-GAP-04 (HIGH):** SSH on all Layer 3 devices; Telnet disabled  
✅ **F-GAP-05 (HIGH):** RADIUS operational; local credentials upgraded (12+chars, RSA 2048+)  
✅ **F-GAP-12 (HIGH):** DMZ-FTP migrated to SFTP; default account removed; ASA ACL restricted to secure port  

**MEDIUM findings (F-GAP-06 to F-GAP-11)** can be addressed post-deployment if:
- Log retention policy approved before go-live
- Backup/restore readiness tested or formally accepted
- Incident response procedure drafted
- Supplier security clauses added to contract
- Dossier contradictions corrected before design acceptance

**Edge redundancy (F-GAP-09)** can be scheduled post-launch per approved risk acceptance (6-month timeline).

## 7. Limitations

This was a **documentation and Packet Tracer simulation review**, not a test of production hardware:

- **Several findings depend on PT limitations** that the dossier itself documents (SVI ACL enforcement, RADIUS commands, Syslog on switches/ASA, iSCSI, 802.1X)
- **Must be re-validated on real equipment** before production go-live
- **Dossier contains internal contradictions** (Syslog IPs, RADIUS pass/fail, VLAN counts) — see F-GAP-11
- **Should be resolved with contractor** before final acceptance

Full limitations: `deliverables/findings/risk-register.md`, section "Limitations & Caveats"

## 8. Conclusion

The delivered design demonstrates a coherent security architecture — segmentation, perimeter firewall, centralised authentication and logging are all present as concepts — but **as documented and tested, it is not ready for production go-live**.

**One Critical and five High findings remain open**, including one (F-GAP-12) that exposes an Internet-facing service today.

### Recommendations:

1. **Withhold go-live approval** until six Gate items (§6) are demonstrated on production hardware
2. **Ask contractor to resolve dossier contradictions** (F-GAP-11) as part of final acceptance
3. **Validate on representative hardware** before deployment (Packet Tracer gaps documented)
4. **Establish incident response 24/7 coverage** before go-live (supports NIS2/GDPR readiness per NIS2-GDPR-Incident-Notification.md)

---

**Prepared and coordinated by:** Sajjad Shahpoor — Audit Coordinator, Risk & Reporting

**Team:** Control Freaks  
— Madumathi Singaraju, Technical Network Auditor  
— Hanah Marroun, Regulatory & Compliance Auditor  
— Sajjad Shahpoor, Audit Coordinator, Risk & Reporting