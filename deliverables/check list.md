# Audit Checklist — NVIDIA Regional R&D Hub

**Project:** NVIDIA Regional R&D and Production Hub  
**Project ID:** NVIDIA-REG-RD-2026-1106  
**Team:** [Control Freaks]  
**Date:** 21 September 2026  
**Status:** Draft — Technical validation pending

---

## Overview

This checklist contains 15 checks across five audit areas. Each check is linked to an applicable regulatory requirement and to a recognised control framework. Results must be based on cited evidence from the RFQ, the delivered design documents and the Packet Tracer configuration.

**Allowed results:** `Pass` · `Fail` · `Not verifiable / Insufficient evidence`

---

## Area 1 — Network segmentation

| ID | Requirement | Control reference | Yes/No question | Evidence source | Pass condition | Result | Evidence citation / notes |
|---|---|---|---|---|---|---|---|
| **NS-01** | NIS2 Article 21(2)(i): access control and asset management | ISO/IEC 27001:2022 A.8.22 — Segregation of networks | Are departments and security zones separated into VLANs according to their functions and risk levels? | RFQ; VLAN/subnet worksheet; network diagram; Packet Tracer | Passes if the required departments and security zones are assigned to separate VLANs and the VLAN IDs, subnets and gateways are consistent across the documentation and Packet Tracer. |  |  |
| **NS-02** | NIS2 Article 21(2)(i): access control | ISO/IEC 27001:2022 A.8.20 — Network security; A.8.22 — Segregation of networks | Do ACLs or equivalent controls restrict traffic between departmental VLANs to authorised flows only? | Security configuration; access-control matrix; Layer 3 device configurations; Packet Tracer tests | Passes if documented ACLs are applied to the correct interfaces or VLANs, permit only required flows and deny unauthorised inter-VLAN traffic, with successful positive and negative tests. |  |  |
| **NS-03** | NIS2 Article 21(2)(i): access control | ISO/IEC 27001:2022 A.8.20 — Network security; A.8.22 — Segregation of networks | Is the DMZ separated from internal networks by restrictive firewall rules? | Network diagram; firewall configuration; security configuration; Packet Tracer tests | Passes if the DMZ uses a separate subnet or VLAN and firewall rules allow only documented services, sources and destinations while blocking unauthorised DMZ-to-internal traffic. |  |  |

---

## Area 2 — Access control

| ID | Requirement | Control reference | Yes/No question | Evidence source | Pass condition | Result | Evidence citation / notes |
|---|---|---|---|---|---|---|---|
| **AC-01** | NIS2 Article 21(2)(h): policies and procedures regarding cryptography | ISO/IEC 27001:2022 A.8.20 — Network security; A.8.24 — Use of cryptography | Is administrative access to routers, switches and the firewall protected by encrypted protocols? | Device configurations; security configuration; Packet Tracer | Passes if SSH or another approved encrypted protocol is configured for every manageable network device and insecure remote-management protocols such as Telnet are disabled. |  |  |
| **AC-02** | NIS2 Article 21(2)(i): access control and asset management | ISO/IEC 27001:2022 A.8.5 — Secure authentication; CyFun 2025 PR.AA-01.1 and PR.AA-01.2 | Is authentication for network administration centrally managed through AAA, with controlled identities and credentials? | AAA/RADIUS server configuration; device AAA configuration; access-control documentation; Packet Tracer | Passes if the AAA/RADIUS service exists, relevant devices use it, individual administrative identities can be demonstrated and a documented fallback method does not bypass accountability. |  |  |
| **AC-03** | NIS2 Article 21(2)(i): access control | ISO/IEC 27001:2022 A.8.22 — Segregation of networks | Is the guest network prevented from accessing internal and management networks? | VLAN plan; ACL/firewall configuration; access-control matrix; Packet Tracer tests | Passes if the guest network is separately segmented, can reach only explicitly authorised services such as the internet and cannot reach internal, server or management subnets in negative tests. |  |  |

---

## Area 3 — Logging and monitoring

| ID | Requirement | Control reference | Yes/No question | Evidence source | Pass condition | Result | Evidence citation / notes |
|---|---|---|---|---|---|---|---|
| **LM-01** | NIS2 Article 21(2)(b) and (f): incident handling and assessment of control effectiveness | ISO/IEC 27001:2022 A.8.15 — Logging | Are relevant security events from in-scope network devices sent to a central logging service? | Syslog design; server configuration; device configurations; Packet Tracer event evidence | Passes if the central log server is documented and each in-scope device is configured to send relevant timestamped events to it; unsupported simulator functions must be recorded as limitations. |  |  |
| **LM-02** | NIS2 Article 21(2)(b): incident handling | ISO/IEC 27001:2022 A.8.15 — Logging | Does the firewall record relevant permitted, denied and administrative security events and forward them centrally? | Firewall configuration; syslog configuration; test report; Packet Tracer | Passes if logging is enabled for relevant firewall and administrative events and evidence shows forwarding to the central logging service, or the inability to demonstrate this is recorded as not verifiable. |  |  |
| **LM-03** | GDPR Article 5(1)(e): storage limitation; Article 32 where logs contain personal data | ISO/IEC 27001:2022 A.8.15 — Logging | Is a justified retention period and protected disposal process documented for security logs? | Logging policy; data-retention policy; design dossier | Passes if the retention period, justification, access restrictions and deletion or archival process are documented. A generic example such as “90 days” without justification does not pass. |  |  |

---

## Area 4 — Data protection and incident readiness

| ID | Requirement | Control reference | Yes/No question | Evidence source | Pass condition | Result | Evidence citation / notes |
|---|---|---|---|---|---|---|---|
| **DP-01** | GDPR Article 30 where a record of processing is required; Article 5(2): accountability | ISO/IEC 27001:2022 A.5.9 — Inventory of information and other associated assets; A.5.34 — Privacy and protection of PII | Are the categories, purposes, storage locations and responsible owners of personal data within the audited design documented? | Asset inventory; data-flow or processing records; RFQ; design dossier | Passes if personal-data categories, purposes, systems or locations, responsible owners, recipients and relevant retention information are documented. Identifying only a server VLAN is insufficient. |  |  |
| **DP-02** | GDPR Article 32(1)(b)–(d): resilience, restoration and regular testing | ISO/IEC 27001:2022 A.8.13 — Information backup | Are critical systems and data covered by documented, protected and tested backup and recovery arrangements? | Backup architecture; recovery procedure; test evidence; design dossier | Passes if backup scope, frequency, storage separation, access protection, recovery objectives and restoration testing are documented. The presence of iSCSI storage alone does not demonstrate a backup. |  |  |
| **DP-03** | NIS2 Article 21(2)(b): incident handling; GDPR Articles 33–34 when notification conditions are met | ISO/IEC 27001:2022 A.5.24 — Incident management planning and preparation; A.5.26 — Response to information security incidents | Is there a documented incident-response and regulatory-notification procedure with roles, escalation paths and applicable deadlines? | Incident-response plan; notification procedure; contact list; exercise evidence | Passes if roles, escalation, evidence preservation and regulator contacts are defined, including NIS2 stages (24 hours, 72 hours and one month) and GDPR notification within 72 hours when applicable, plus communication to individuals when high risk is established. |  |  |

---

## Area 5 — Availability and supply chain

| ID | Requirement | Control reference | Yes/No question | Evidence source | Pass condition | Result | Evidence citation / notes |
|---|---|---|---|---|---|---|---|
| **HA-01** | NIS2 Article 21(2)(c): business continuity and disaster recovery | ISO/IEC 27001:2022 A.8.14 — Redundancy of information processing facilities | Does the routing design avoid a single point of failure and provide tested failover? | Network topology; routing configuration; high-availability design; Packet Tracer tests | Passes if redundant routing components and paths are documented and a test demonstrates that required connectivity continues after failure of one routing component or path. |  |  |
| **HA-02** | NIS2 Article 21(2)(c): business continuity and disaster recovery | ISO/IEC 27001:2022 A.8.14 — Redundancy of information processing facilities | Is loss of the primary internet gateway addressed by a documented and testable continuity mechanism? | Network topology; edge-router/firewall design; continuity documentation; Packet Tracer | Passes if a second gateway or another documented continuity arrangement exists and failover can be evidenced. If the design contains only one unmitigated gateway, the check fails. |  |  |
| **SC-01** | NIS2 Article 21(2)(d): supply-chain security | ISO/IEC 27001:2022 A.5.19 — Information security in supplier relationships; A.5.20 — Addressing information security within supplier agreements | Are relevant suppliers and external services identified, with security obligations included in their agreements? | RFQ; supplier list; contracts; Bill of Materials; service documentation | Passes if relevant suppliers and dependencies are listed and agreements define proportionate security requirements, incident notification, responsibilities and review or assurance rights. A vendor list alone is insufficient. |  |  |

---

## Summary

| Area | Checks | Pass | Fail | Not verifiable |
|---|---:|---:|---:|---:|
| Network segmentation | 3 |  |  |  |
| Access control | 3 |  |  |  |
| Logging and monitoring | 3 |  |  |  |
| Data protection and incident readiness | 3 |  |  |  |
| Availability and supply chain | 3 |  |  |  |
| **Total** | **15** |  |  |  |

---

## Audit note

A blank result means that the check has not yet been executed. A control must not be marked `Pass` solely because it is described in a document: the cited evidence must satisfy the complete pass condition. When Packet Tracer cannot reproduce a production feature, record the limitation and use `Not verifiable / Insufficient evidence` unless other reliable evidence is available.
