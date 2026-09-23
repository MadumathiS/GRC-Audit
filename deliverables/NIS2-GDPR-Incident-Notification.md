# Incident Notification — NVIDIA Regional R&D Hub

**Exercise date:** Tuesday 12 May 2026  
**Status:** Completed team exercise  
**Authorities:** CCB/CERT.be (NIS2) and APD/GBA (GDPR)

---

## Part A — Obligations and clocks

| Question | Answer |
|---|---|
| Is the entity in scope of NIS2, and as what? | **Yes, as an important entity for this exercise.** The corrected scenario identifies a Belgian subsidiary manufacturing computer, electronic and optical products, an Annex II NIS2 sector. Although the subsidiary has 48 employees and EUR 7 million turnover, it is wholly owned by the NVIDIA group; the size assessment uses consolidated linked-enterprise data, and the group exceeds the applicable thresholds. |
| Which NIS2 sector places it in scope? | Annex II manufacturing: manufacture of computer, electronic and optical products. The network's "Study", "Production" and similar VLAN sectors are unrelated to this legal sector classification. |
| Is this a significant incident? | **Yes, on the preliminary facts.** Criterion 1 is met because external use of the FTP service account and large outbound transfers indicate a malicious compromise of confidentiality. Criterion 3 is also reasonably triggered because the affected contents include R&D project files and production artefacts, creating suspected loss of intellectual property or trade secrets. The availability criterion is not claimed: service was degraded, but the facts do not establish the quantified unavailability threshold. Harm to others and recurrence are not yet established. |
| Is a second obligation triggered? | **Yes.** The likely exfiltration includes names, private addresses, national register numbers and salary bands for 48 employees. This is a personal-data breach requiring notification to the Belgian APD/GBA under GDPR Article 33 unless it is unlikely to create risk; these facts clearly indicate risk. The presence of national register numbers and employment data also makes high risk plausible, so communication to affected employees under Article 34 is required unless a documented exception applies. |
| NIS2 awareness | **12 May 2026 at 07:55.** At that point the administrator identified repeated authentication from an external IP using the FTP service account. The earlier outage at 07:40 was not, by itself, enough to identify a security incident. |
| GDPR awareness | **12 May 2026 at 10:05.** At that point NVIDIA had reasonable certainty that the affected server held identifiable employee data and that its contents were likely included in the outbound transfer. A more conservative organisation may use 07:55 for both clocks. |
| NIS2 deadlines | Early warning: **Wednesday 13 May 2026, 07:55**. Incident notification: **Friday 15 May 2026, 07:55**. If the 72-hour notification is filed at that deadline, final report: **Monday 15 June 2026, 07:55**; if filed earlier, the one-month deadline runs from the actual notification time. |
| GDPR/APD deadlines | Article 33 notification: **Friday 15 May 2026, 10:05**. Under the Belgian exercise template, Part 2: **Tuesday 2 June 2026, 10:05**. Affected employees should be informed **without undue delay** if the high-risk conclusion remains. |
| Which is filed first? | File the short NIS2 early warning first because it has the earliest deadline. Begin the APD/GBA notification in parallel; do not wait for the investigation to finish. |

---

## Part B — NIS2 notification (CCB / CERT.be)

### B1 — Early warning within 24 hours

- **Entity, site, sector, registration reference:** Belgian NVIDIA subsidiary; Regional R&D and Production Hub; Annex II manufacture of computer, electronic and optical products; registration reference to be supplied by Legal/Compliance.
- **Contact person:** Incident manager / CISO contact — name, telephone and email to be supplied.
- **Time of awareness:** Tuesday 12 May 2026, 07:55.
- **Suspected malicious acts:** **Yes.** The FTP service account was repeatedly used from an external IP during the night, followed by unusually large outbound transfers. The credential-acquisition method is not yet known.
- **Possible cross-border impact:** **Unknown.** The external IP's location, the destination of the transferred data and any impact on NVIDIA operations outside Belgium have not yet been established. IP attribution and group-impact analysis are in progress.

### B2 — Incident notification within 72 hours

**Severity assessment**  
Confirmed unauthorised external authentication and outbound transfer activity indicate a malicious confidentiality compromise. The server contained R&D project files and production build artefacts, creating a reasonable preliminary basis for suspected intellectual-property or trade-secret loss. The service remains degraded for the Production and Study sectors. No quantified availability criterion is claimed because complete loss of access for the required number of users and duration has not been established.

**Initial impact assessment**  
The FTP service used by the Production and Study sectors was unresponsive at 07:40 and returned in a degraded state after restart. It supports exchange of build artefacts. The server also stored R&D files, production artefacts and an uncatalogued HR export relating to 48 employees. The exact number of files transferred, operational loss and financial impact are not yet known.

**Indicators of compromise — confirmed facts**

- External source IP: exact address to be inserted from the logs; blocked at 10:30 on 12 May 2026.
- Account: FTP service account; disabled at 10:30.
- Suspicious authentication: repeated external authentications during the night before discovery.
- Outbound-transfer window: Monday 11 May 2026, 02:10–04:35.
- Volume: large and consistent with transfer of the bulk of the server contents; exact files and completeness are not yet confirmed.
- Hashes, filenames and additional indicators: not yet available.

**Update since the early warning**  
The external IP has been blocked and the FTP service account disabled. The server remains online in a degraded state while a controlled rebuild is planned. The investigation has confirmed the categories of information stored on the server but has not yet confirmed precisely which files left the network.

**Unknowns and actions to resolve them**

- Credential-acquisition method: preserve authentication evidence, review password reuse and examine relevant endpoints.
- Other DMZ compromise: review firewall, server and network logs and scan other DMZ assets.
- Publication or onward disclosure: conduct threat-intelligence and exposure monitoring.
- Persistence: isolate and forensically image the server, identify active sessions and indicators, and rebuild from trusted media.
- Exact transferred data: correlate FTP, host and network logs and compare source directories with transfer records.

### B3 — Final report placeholder

The final report cannot honestly be completed at 10:45 on 12 May. It must later document the confirmed incident chronology, threat and root cause, affected systems and data, mitigation and recovery measures, cross-border impact and consolidated severity. If the incident remains ongoing at the one-month deadline, submit a progress report and the final report within one month after resolution.

---

## Part C — Personal-data breach notification (APD / GBA)

### C1 — Part 1 within 72 hours

**General information**

- **Date and time of breach:** Best current estimate: Monday 11 May 2026, 02:10–04:35.
- **Date and time discovered:** Suspicious external authentication identified Tuesday 12 May at 07:55; personal-data involvement confirmed at 10:05.
- **How discovered:** Investigation of an unresponsive FTP server followed by authentication and outbound-transfer log review.
- **Nature:** ☒ Confidentiality ☐ Integrity ☒ Availability. Confidentiality is the main breach; temporary service degradation also affected availability. No unauthorised alteration has been confirmed.
- **Description:** An external IP repeatedly authenticated using an unchanged FTP service-account password. Logs show a large outbound-transfer volume consistent with the bulk of the server contents leaving the network. The server contained an HR export relating to all 48 site employees. Exact transferred files are still being established.

**Organisation**

- **Legal name, enterprise number and address:** Exact Belgian subsidiary details to be supplied by Legal; do not substitute "NVIDIA Corporation" without verifying the Belgian controller.

**Contact**

- **DPO or notification contact:** Name, email and telephone to be supplied.

### C2 — Part 2 within 21 calendar days

**Categories of data affected**  
Names, private home addresses, Belgian national register numbers and salary bands. No special-category data under GDPR Article 9 has been identified in the exercise facts.

**Individuals affected**  
One category: employees of the Belgian site. Approximate number: **48**.

**Likely consequences**  
Identity fraud, targeted phishing and social engineering, impersonation, unwanted disclosure of compensation information, discrimination or workplace harm, and loss of privacy. National register numbers are persistent identifiers and cannot be rotated like passwords.

**Risk assessment**  
☒ **High risk.** The combination of persistent government identifiers, home addresses and employment information creates credible and potentially serious consequences for the 48 employees. GDPR Article 34 communication should therefore occur without undue delay unless Legal/DPO documents that an Article 34(3) exception applies.

**Communication to employees**  
Send individual notices through verified work and personal contact channels without undue delay. Use clear language describing what happened, the data categories, likely consequences, steps NVIDIA has taken, protective steps employees can take, and DPO/contact details. Do not wait for the full forensic investigation; provide updates as facts develop.

**Root cause**  
Not yet established. Confirmed contributing condition: the FTP service account used a password set at deployment that had never been changed. How the attacker obtained it remains unknown.

**Measures taken and planned**

- Blocked the external IP and disabled the FTP service account at 10:30 on 12 May.
- Preserve logs and forensic evidence; isolate and rebuild the server from trusted media.
- Rotate related credentials and investigate reuse or exposure.
- Review all DMZ assets and monitor for persistence or data publication.
- Remove HR exports from inappropriate storage and complete the data inventory.
- Implement service-account password rotation, least privilege, stronger authentication where supported, and centralised logging and alerting.
- Provide support and practical guidance to affected employees.

**Attachments**

- Incident timeline and decision log.
- Relevant authentication and transfer-log extracts with integrity information.
- Data-category and affected-person assessment.
- Containment-action record.
- Preliminary risk assessment and employee-communication draft.
- Submit later forensic updates separately; do not delay the initial filing while waiting for them.