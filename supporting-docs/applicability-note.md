# Applicability Note — NVIDIA Regional R&D Hub

**Project:** NVIDIA Regional R&D and Production Hub  
**Project ID:** NVIDIA-REG-RD-2026-1106  
**Audit type:** Pre-go-live network-design and documentation audit  
**Status:** Draft — Under Review  

---

## 1. Purpose and Evidence Basis

This note determines which regulations and recognised frameworks are relevant to the audit of the delivered network design. It is based on the NVIDIA RFQ, the assigned team's design dossier, the Packet Tracer submission and the instructor's clarification for the separate incident-notification exercise.

An applicability decision does not establish compliance. Compliance results, findings and risk ratings must be recorded only after the audit checks have been performed against cited evidence.

## 2. Organisation Profile

| Field | Value |
|---|---|
| Organisation / Site | NVIDIA Regional R&D and Production Hub — educational project |
| Project ID | NVIDIA-REG-RD-2026-1106 |
| Activity | Research, development and production, as described in the RFQ |
| Location | Belgium |
| Network size | 48 workstations specified in the RFQ. Infrastructure devices are recorded separately in the asset inventory. |
| Legal entity and size | The number of workstations does not establish the legal entity's employee count, turnover or balance-sheet total. See the scenario clarification below. |
| Information in scope | R&D and production information, network configurations, authentication information and logs. Exact personal-data categories and storage locations require evidence. |
| Audit scope | Review of the delivered network design and supporting documentation before go-live |

### Scenario clarification

The instructor's correction to the incident-notification exercise describes a Belgian subsidiary wholly owned by the NVIDIA group, with 48 employees, EUR 7 million turnover and a parent group above the relevant size thresholds. Its stated activity is the manufacture of computer, electronic and optical products.

Those facts are established for the incident exercise. Their use in the broader design audit remains subject to confirmation and must not be inferred from the number of workstations.

The HR export described in the incident exercise must not be treated as evidence that employee addresses, national register numbers or salary information are stored on the AAA server in the audited design.

## 3. Applicability Summary

| Regulation or framework | Assessment | Basis / outstanding information |
|---|---|---|
| GDPR | **Applies** to the intended personal-data processing | Operational accounts and logs are expected to relate to identifiable individuals. Confirm the actual data categories, purposes, retention periods and storage locations. |
| Belgian NIS2 Law | **Uncertain for the design audit**; **Applies — Important entity** in the corrected incident scenario | Confirm whether the stated manufacturing activity, group ownership and consolidated size facts also apply to the audited legal entity. |
| ISO/IEC 27001:2022 | **Used as audit criteria** | The project requires selected Annex A controls. No certification status has been established. |
| CyberFundamentals 2025 | **Used as an audit framework** | Record each selected control precisely. The applicable assurance level requires a documented justification. |
| Cyber Resilience Act | **Uncertain** | Confirm the products placed on the EU market and the audited entity's role as manufacturer, importer or distributor. Product conformity is outside this network audit. |
| DORA | **Does not apply to the described activities**, based on available evidence | The dossier does not establish that the entity is a financial entity or a designated critical ICT third-party provider. |
| EU AI Act | **Uncertain** | Identify the actual AI systems or models, intended purposes and the entity's role before determining obligations or risk classification. |

## 4. GDPR — General Data Protection Regulation

**Assessment: Applies to the hub's intended personal-data processing.**

The hub is located in Belgium. GDPR applies to personal-data processing in the context of an EU establishment's activities. For this design audit, operational user accounts and activity logs are expected to be linked to identifiable individuals; the simulator's test accounts alone do not prove actual personal-data processing.

The dossier describes AAA authentication and logging services. User identifiers and logs may be personal data when they relate to identifiable individuals. The exact categories, purposes, retention periods and storage locations require confirmation.

There is no established evidence that the AAA server stores employee addresses, national register numbers or salary information. The HR export on the FTP server belongs to the separate incident exercise.

Relevant requirements include:

- Article 5: data minimisation, storage limitation, integrity and confidentiality.
- Article 30: records of processing activities where its conditions apply.
- Article 32: security measures appropriate to the risk.
- Article 33: notification of qualifying personal-data breaches without undue delay and, where feasible, within 72 hours of awareness.
- Article 34: communication to affected individuals without undue delay where the breach is likely to result in a high risk, subject to the Article's exceptions.

This is not a complete GDPR assessment. A Data Protection Impact Assessment or data processing agreement must not be declared mandatory without first assessing the processing activities and organisational roles.

**Source:** [Regulation (EU) 2016/679](https://eur-lex.europa.eu/eli/reg/2016/679/oj/eng)

## 5. NIS2 — Belgian NIS2 Law

**Assessment: Applies as an Important entity in the corrected incident scenario. Applicability to the design audit remains conditional on confirmation of the same organisational facts.**

The corrected incident scenario describes a Belgian subsidiary wholly owned by NVIDIA, with 48 employees, EUR 7 million turnover and a group above the relevant size thresholds. Its manufacturing activity falls within Annex II, point 5(b), covering the manufacture of computer, electronic and optical products.

The size assessment considers linked-enterprise data under Recommendation 2003/361/EC rather than the subsidiary's staff and turnover alone. On the corrected scenario's facts, the entity is classified as Important. No separate basis for Essential classification has been established.

Relevant obligations include cybersecurity risk-management measures and significant-incident reporting. For a qualifying significant incident, the reporting stages are:

- Early warning: within 24 hours of awareness.
- Incident notification: within 72 hours of awareness.
- Final report: no later than one month after submission of the incident notification. If the incident remains ongoing, a progress report is submitted and the final report follows within one month after the incident has been handled.

The Belgian authority is the Centre for Cybersecurity Belgium (CCB), with operational incident handling through CERT.be.

The RFQ's 48 workstations do not establish the entity's legal size. Until the exercise's organisational facts are confirmed for the design audit, NIS2 must remain marked **Uncertain** in that audit context.

**Sources:** [Directive (EU) 2022/2555](https://eur-lex.europa.eu/eli/dir/2022/2555/oj/eng) · [Belgian NIS2 information — CCB](https://ccb.belgium.be/en/nis2)

## 6. ISO/IEC 27001:2022

**Assessment: Used as audit criteria, as required by the project instructions.**

ISO/IEC 27001:2022 is an international standard for an information security management system; it is not a law. Selected Annex A controls provide criteria for assessing the delivered network design. Each selected control must be linked to a check, a defined pass condition and supporting evidence.

No evidence establishing a valid ISO/IEC 27001 certificate covering this hub has been identified. Certification status is therefore not verified. This project is a limited design audit, not a certification audit or a complete assessment of the organisation's ISMS. Passing selected network checks does not demonstrate full conformity with ISO/IEC 27001.

**Source:** [ISO/IEC 27001:2022](https://www.iso.org/standard/27001)

## 7. CyberFundamentals 2025

**Assessment: Used as an audit framework, as required by the project instructions.**

CyberFundamentals is a cybersecurity framework developed by the Centre for Cybersecurity Belgium. Selected measures help translate cybersecurity objectives into checks and evidence requirements.

This audit uses **CyberFundamentals 2025, version 2025-10-01**. Each checklist reference must identify the individual measure rather than cite only a broad category such as `PR.AC`.

For example, the centralised-authentication check is mapped to:

- `PR.AA-01.1`: identities and credentials for authorised users, services and hardware are managed.
- `PR.AA-01.2`: identities and credentials are managed through automated mechanisms whenever feasible.

The initial draft selected the CyFun Important assurance level without documenting the rationale. NIS2 entity classification and CyFun assurance level must be recorded separately; sharing the word “Important” does not itself justify the CyFun level.

This limited design audit does not establish full CyFun conformity or certification. The existence of a firewall, VLANs or a Syslog server does not by itself demonstrate that the relevant measures are operating effectively.

**Source:** [CyberFundamentals Framework — CCB](https://atwork.safeonweb.be/en/tools-resources/cyberfundamentals-framework)

## 8. Cyber Resilience Act

**Assessment: Uncertain for the audited legal entity. Product conformity assessment is outside this network-design audit.**

The Cyber Resilience Act establishes cybersecurity requirements for products with digital elements made available on the EU market, subject to its scope and exclusions.

The available dossier does not establish which products the audited entity places on the EU market or whether it acts as manufacturer, importer or distributor. Therefore, the initial draft's conclusion that the CRA does not apply is unsupported.

The internal network audit does not assess the conformity of products manufactured or sold by the organisation. Excluding product conformity from this audit does not establish that the CRA is inapplicable to the organisation.

CRA reporting obligations apply from 11 September 2026, while the Regulation's main obligations apply from 11 December 2027. Any assessment must consider the relevant obligation and date.

**Source:** [European Commission — Cyber Resilience Act](https://digital-strategy.ec.europa.eu/en/policies/cyber-resilience-act)

## 9. DORA — Digital Operational Resilience Act

**Assessment: Does not apply to the hub's described activities, based on the available evidence.**

DORA governs digital operational resilience in the financial sector. The dossier describes an R&D and production hub and provides no evidence that the audited entity belongs to a financial-entity category listed in Article 2.

DORA also regulates certain ICT third-party relationships. Financial-sector customers may impose contractual security requirements, while designated critical ICT third-party providers are subject to a specific oversight framework. The dossier provides no evidence of a relevant designation or contract establishing those obligations for the audited entity.

**Source:** [Regulation (EU) 2022/2554](https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng)

## 10. EU AI Act

**Assessment: Uncertain — insufficient information about the AI systems, models and activities involved.**

The dossier mentions computing infrastructure intended for AI/ML workloads. This does not establish the systems or models involved, their intended purposes or the audited entity's legal role. AI research or training is not automatically classified as high-risk.

To determine applicability, the organisation must identify:

- The AI systems or models and their intended purposes.
- Whether the entity acts as a provider, deployer or another regulated operator.
- Whether any exclusions, including relevant research exclusions, apply.
- The applicable obligations and application dates.

This network-design audit does not assess AI product conformity or the organisation's overall compliance with the EU AI Act.

**Source:** [Regulation (EU) 2024/1689](https://eur-lex.europa.eu/eli/reg/2024/1689/oj/eng)

## 11. Conclusion and Limitations

This note establishes the regulatory and framework basis for the audit; it does not demonstrate compliance.

GDPR is relevant to the hub's intended personal-data processing. NIS2 applicability to the design audit remains conditional on confirmation of the organisational facts supplied for the separate incident exercise. ISO/IEC 27001:2022 and CyberFundamentals 2025 provide audit criteria and are not presented as additional laws or evidence of certification.

Unresolved applicability decisions must be updated if supporting evidence becomes available. Confirmed legal requirements and selected framework measures must be linked to the audit checklist. Findings and risk ratings must be recorded separately after the checks have been performed.

### Key limitations

- This is a documentation and Packet Tracer design audit; no production systems were tested.
- Legal-entity identity, consolidated size and group structure were not established by the RFQ.
- Exact personal-data categories, purposes, storage locations and retention periods require confirmation.
- ISO/IEC 27001 certification and full CyFun conformity were not verified.
- Product-level CRA and AI Act conformity are outside the network-design audit.
