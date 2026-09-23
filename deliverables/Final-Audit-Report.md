# Final Audit Report — NVIDIA Regional R&D and Production Hub

**Project ID:** NVIDIA-REG-RD-2026-1106
**Prepared by:** Sajjad Shahpoor, Audit Coordinator (Risk & Reporting), on behalf of **Control Freaks** (Madumathi Singaraju, Hanah Marroun, Sajjad Shahpoor)
**Date:** 22 September 2026
**Status:** Final — for submission

---

## 1. Purpose of this report

This document is the closing deliverable of the audit: it pulls together the scope, the applicability decision, the checklist results, all findings — including one raised after the original 15-control checklist was closed — and the risk register into a single reader for someone who has not followed the audit day to day. It does not repeat the detail already recorded in `supporting-docs/`, `deliverables/check list.md`, `deliverables/findings/` and `deliverables/risk-register.md`; it points to them and summarises what they say.

## 2. What we audited, and how

This was a **design (documentation) audit** of the network delivered for NVIDIA's Regional R&D and Production Hub — not a penetration test. We compared the contractor's dossier (design documents, device configuration, the Packet Tracer test report) against the RFQ and against recognised frameworks, using the chain the module teaches: **requirement → control → check → result → finding**. Full scope, in/out-of-scope areas, methodology and assumptions are in `supporting-docs/scope-statement.md`.

## 3. What applies

Full reasoning is in `supporting-docs/applicability-note.md`. In summary:

| Regulation / framework | Status |
|---|---|
| GDPR | Applies to the hub's intended personal-data processing |
| Belgian NIS2 Law | Uncertain for this design audit; applies as an *Important* entity under the corrected incident-exercise scenario, pending confirmation of the same facts for the audited entity |
| ISO/IEC 27001:2022 | Used as audit criteria (not a certification claim) |
| CyberFundamentals 2025 | Used as an audit framework (not a certification claim) |
| Cyber Resilience Act, EU AI Act | Uncertain — insufficient evidence in the dossier to decide |
| DORA | Does not apply, on the evidence available |

## 4. Results at a glance

**Core checklist (15 controls, 5 areas):** 3 Pass · 2 Partial · 10 Fail. Detail in `deliverables/check list.md`.

**All findings (12 total — 11 from the checklist, plus 1 supplementary finding raised while reviewing the Testing Report's evidence):**

| ID | Finding | Rating |
|---|---|---|
| F-GAP-01 | Centralised logging incomplete; firewall and switches not monitored | **CRITICAL** |
| F-GAP-02 | Department ACLs configured but not enforced | HIGH |
| F-GAP-03 | DMZ isolation depends on a non-enforced ACL; two-interface firewall limits segmentation | HIGH |
| F-GAP-04 | SSH management not deployed to all Layer 3 devices | HIGH |
| F-GAP-05 | RADIUS authentication and credential strength | HIGH |
| F-GAP-12 | Internet-facing FTP service uses cleartext transfer and a default-style account | HIGH |
| F-GAP-06 | No log retention policy documented | MEDIUM |
| F-GAP-07 | No backup design or restore testing | MEDIUM |
| F-GAP-08 | No 24/7 incident-response procedure; support Mon–Fri 08:00–18:00 only | MEDIUM |
| F-GAP-10 | No security requirements in the supplier contract | MEDIUM |
| F-GAP-11 | Dossier is internally inconsistent (contradicting claims, wrong IPs) | MEDIUM |
| F-GAP-09 | Single points of failure at the Internet edge | LOW |

Full write-ups: `deliverables/findings/Critical Findings.md` (F-GAP-01 to 05) and `deliverables/findings/F-GAP-12-FTP-Cleartext-Default-Credentials.md`. Full register with likelihood/impact reasoning, owners and timelines: `deliverables/risk-register.md`.

## 5. What NVIDIA should do first

The register's Priority Actions (`deliverables/risk-register.md`) name **F-GAP-01 (logging), F-GAP-03 (DMZ architecture) and F-GAP-05 (authentication)** as the top three. **F-GAP-12 (FTP) carries the same HIGH rating and the same before-go-live urgency** — the account it flags is reachable from the Internet today, over an unencrypted protocol, with permission to delete and rename files — and should be actioned in the same wave as those three rather than deferred behind the MEDIUM findings.

## 6. Go-live gate

Before production deployment, the register requires:

- **F-GAP-01 (CRITICAL):** Syslog receiving logs from all Layer 3 devices and the ASA.
- **F-GAP-02 (HIGH):** ACL enforcement verified on real hardware, or an in-line workaround deployed.
- **F-GAP-03 (HIGH):** Three-interface firewall, or an equivalent architecture, deployed and tested.
- **F-GAP-04 (HIGH):** SSH on every Layer 3 device; Telnet disabled.
- **F-GAP-05 (HIGH):** Authentication chain operational; local credentials upgraded.
- **F-GAP-12 (HIGH):** DMZ-FTP migrated to SFTP; the `cisco` account removed; the ASA ACL restricted to the secure port only.

The MEDIUM and LOW findings (retention, backup, incident response, supplier contract, dossier consistency, edge redundancy) can be scheduled within 30–90 days post-deployment; detail and target dates are in the register.

## 7. Limitations

This was a documentation and Packet Tracer simulation review, not a test of production hardware — several findings (notably F-GAP-02, F-GAP-03 and the RADIUS element of F-GAP-05) depend on Packet Tracer limitations that the dossier itself documents, and must be re-validated once the design is built on real equipment. The dossier also contains internal contradictions (see F-GAP-11), which should be resolved with the contractor before final acceptance. Full limitations: `deliverables/risk-register.md`, "Limitations & Caveats".

## 8. Conclusion

The delivered design demonstrates a coherent security architecture — segmentation, a perimeter firewall, centralised authentication and logging are all present as concepts — but as documented and tested, it is **not ready for production go-live**. One Critical and five High findings remain open, including one, F-GAP-12, that exposes an Internet-facing service today. We recommend NVIDIA withhold go-live approval until the six Gate items in §6 are demonstrated on production hardware, and that the contractor be asked to resolve the dossier's internal contradictions (F-GAP-11) as part of final acceptance.

---

**Prepared and coordinated by:** Sajjad Shahpoor — Audit Coordinator, Risk & Reporting
**Team:** Control Freaks — Madumathi Singaraju (Technical Network Auditor) · Hanah Marroun (Regulatory & Compliance Auditor) · Sajjad Shahpoor (Audit Coordinator, Risk & Reporting)
