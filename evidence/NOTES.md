# External Evidence References

**Project:** GRC-Audit (NVIDIA Regional R&D Hub)  
**Purpose:** Links to vendor documentation, standards, and regulatory references  
**Status:** Final

---

## Table of Contents
1. [Cisco Vendor Documentation](#cisco-vendor-documentation)
2. [Regulatory References](#regulatory-references)
3. [Industry Standards & Frameworks](#industry-standards--frameworks)
4. [Packet Tracer Limitations](#packet-tracer-limitations)
5. [Cryptography & Security Protocols](#cryptography--security-protocols)

---

## Cisco Vendor Documentation

These resources provide configuration guidance and best practices for Cisco network devices used in this project.

### Catalyst Switches
- **Catalyst 9300 Series Documentation:** https://www.cisco.com/c/en/us/support/switches/catalyst-9300-series/products-support.html
- **Switch Configuration Guide:** https://www.cisco.com/c/en/us/td/docs/switches/catalyst9300/cat9300_xe_316/configuration/guide/
- **Spanning Tree Protocol (STP) Configuration:** https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol/
- **VLAN Configuration Best Practices:** https://www.cisco.com/c/en/us/support/docs/lan-switching/vlan/

### ASA Firewall
- **ASA 5500-X Series Firewall:** https://www.cisco.com/c/en/us/support/security/asa-5500-series-next-generation-firewalls/
- **ASA Configuration Guide:** https://www.cisco.com/c/en/us/td/docs/security/asa/asa-command-reference/
- **Firewall ACL Configuration:** https://www.cisco.com/c/en/us/support/docs/security/asa-5500-series-next-generation-firewalls/
- **NAT/PAT on ASA:** https://www.cisco.com/c/en/us/support/docs/security/asa-5500-series-next-generation-firewalls/

### Routing & OSPF
- **OSPF Routing Protocol:** https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/
- **ECMP Load Balancing:** https://www.cisco.com/c/en/us/support/docs/ip/open-shortest-path-first-ospf/

### Management & Logging
- **Syslog Configuration on Cisco Devices:** https://www.cisco.com/c/en/us/support/docs/networking_solutions/ip_communications/

### Packet Tracer
- **Cisco Packet Tracer Download:** https://www.netacad.com/cisco-packet-tracer
- **Packet Tracer Tutorial & Documentation:** https://www.netacad.com/courses/packet-tracer
- **Known Limitations Discussion:** https://learningnetwork.cisco.com/s/packet-tracer-forum

---

## Regulatory References

These documents define compliance requirements for the NVIDIA Regional R&D Hub project.

### GDPR (EU General Data Protection Regulation)
- **GDPR Official Text:** https://gdpr-info.eu/
- **Article 32 (Security Measures):** https://gdpr-info.eu/art-32-gdpr/
  - Requires implementation of pseudonymization, encryption, ability to restore availability, and audit trails
- **Article 33 (Notification of Breach):** https://gdpr-info.eu/art-33-gdpr/
  - 72-hour notification requirement to competent authority
- **Article 34 (Communication of Breach):** https://gdpr-info.eu/art-34-gdpr/
  - Notification to data subjects when high risk is present
- **Recital 83 (Technical Measures):** https://gdpr-info.eu/recitals/recital-83/
  - Guidance on appropriate technical and organizational measures

### NIS2 (Network & Information Security Directive 2)
- **Official NIS2 Directive Text:** https://eur-lex.europa.eu/eli/dir/2022/2555/oj
- **Article 23 (Incident Notification):** https://eur-lex.europa.eu/eli/dir/2022/2555/oj
  - 24-hour early warning to CSIRT (Article 23(1))
  - 72-hour incident notification requirement (Article 23(3))
- **Article 21 (Cybersecurity Risk Management):** https://eur-lex.europa.eu/eli/dir/2022/2555/oj
  - Requirement to manage cybersecurity risks including logging & monitoring
- **Essential Cybersecurity Measures:** https://eur-lex.europa.eu/eli/dir/2022/2555/oj
  - Network segmentation, access control, incident response

### CyFun (Belgian Cyber Fundamentals)
- **CCB Framework (Belgium):** https://atwork.safeonweb.be/cyberfundamentals-framework
- **Incident Reporting Portal (SafeOnWeb):** https://notif.safeonweb.be
- **Belgian Cybersecurity Requirements:** https://www.safeonweb.be/

### Belgium Data Protection Authority (APD/GBA)
- **APD Official Site:** https://www.autoriteprotectiondonnees.be/
- **Guidance Documents:** https://www.autoriteprotectiondonnees.be/ressources

---

## Industry Standards & Frameworks

### ISO/IEC 27001:2022 (Information Security Management)
- **ISO Standard Reference:** https://www.iso.org/standard/27001
- **Key Controls Used in This Audit:**
  - **A.9.2.1** - User access management (authentication, access control)
  - **A.12.4.1** - Event logging (security event logging and monitoring)
  - **A.13.1.3** - Network isolation (VLAN segmentation, DMZ architecture)
  - **A.14.1** - Information security incident management

### NIST Cybersecurity Framework 2.0
- **Official Framework:** https://www.nist.gov/cyberframework
- **Mapping to This Project:**
  - **PR.AC-5** - Access control and identity management (VLAN, SSH, AAA)
  - **PR.PT-3** - Unauthorized physical access prevention (network segmentation)
  - **DE.AE-1** - Anomalies detected (Syslog monitoring, event logging)

### CIS Benchmarks
- **CIS Cisco IOS Benchmark:** https://www.cisecurity.org/benchmark/cisco_ios
- **CIS Catalyst Switches:** https://www.cisecurity.org/cis-benchmarks/
- **Key Controls:**
  - 3.1 - Disable unused network interfaces
  - 3.3 - Address unauthorized network access
  - 1.1 - Use strong authentication and encryption
  - 4.1 - Configure logging and monitoring

### OWASP (Open Web Application Security Project)
- **OWASP Top 10 Network Security:** https://owasp.org/
- **Network Segmentation:** https://cheatsheetseries.owasp.org/cheatsheets/Network_Segmentation_Cheat_Sheet.html

---

## Packet Tracer Limitations

Understanding simulator constraints is critical for production validation.

### Known Limitations in This Project

1. **ACL Enforcement**
   - Partial ACL enforcement inconsistencies observed
   - Interface ACL bindings may not display after project reopen
   - Workaround: Verify ACL configuration with 'show access-lists' command

2. **VLAN Hopping Simulation**
   - Does not simulate actual 802.1Q double-tagging attacks
   - Native VLAN hardening can be verified via configuration, not functional testing
   - **Production Validation:** Use penetration tools (yersinia, VLANHoppers)

3. **RADIUS/AAA**
   - Full AAA/RADIUS simulation is limited
   - Local fallback can be tested; RADIUS can be partially validated
   - **Production Validation:** Test against actual RADIUS server in staging

4. **Spanning Tree Protocol (STP)**
   - Basic STP operation simulated
   - BPDU Guard and Root Guard can be configured but not fully tested
   - Real-time STP recalculation may differ from physical switches
   - **Production Validation:** Test STP failover with link failures

5. **Port Security**
   - Port security features not fully simulated
   - Configuration accepted but enforcement inconsistent
   - **Production Validation:** Configure and test MAC limiting in production

6. **Dynamic ARP Inspection (DAI)**
   - DAI is not supported in Packet Tracer
   - **Production Requirement:** Implement DAI separately in production

7. **Firewall (ASA) Limitations**
   - Basic firewall functions simulated (interface, ACL, basic NAT)
   - Advanced threat protection features limited (IPS, DPI, URL filtering)
   - Control Plane Policing (CoPP) not fully supported
   - **Production Requirement:** Implement advanced security features in production

8. **SIEM/Real-time Monitoring**
   - Syslog collection works, but no real-time SIEM simulation
   - No automated alerting or response simulation
   - **Production Requirement:** Deploy SIEM tool for real-time monitoring

### Mitigation Strategy

All findings in this audit account for Packet Tracer limitations:
- **Configuration validation** = Verified in PKT (reliable)
- **Functional testing** = Partial in PKT; full validation in production
- **Security attack simulation** = Limited in PKT; requires penetration testing in production

**Recommendation:** Treat Packet Tracer results as proof-of-configuration, not proof-of-security.

---

## Cryptography & Security Protocols

### SSH (Secure Shell)
- **RFC 4251 - SSH Protocol Architecture:** https://tools.ietf.org/html/rfc4251
- **SSH Version 2 Best Practices:** https://www.ietf.org/rfc/rfc4253.html
- **Cisco SSH Configuration:** https://www.cisco.com/c/en/us/support/docs/security/ssh/

### OSPF (Open Shortest Path First)
- **RFC 2328 - OSPF Version 2:** https://tools.ietf.org/html/rfc2328
- **OSPF Security:** https://tools.ietf.org/html/rfc7474

### VLANs & Spanning Tree
- **IEEE 802.1Q - VLANs:** https://standards.ieee.org/standard/802_1Q-2022.html
- **IEEE 802.1D - Spanning Tree Protocol:** https://standards.ieee.org/standard/802_1D-2004.html

### RADIUS (Authentication)
- **RFC 2865 - RADIUS Protocol:** https://tools.ietf.org/html/rfc2865
- **RFC 2866 - RADIUS Accounting:** https://tools.ietf.org/html/rfc2866

### Syslog
- **RFC 5424 - The Syslog Protocol:** https://tools.ietf.org/html/rfc5424
- **RFC 3164 - BSD syslog Protocol:** https://tools.ietf.org/html/rfc3164

---

## How to Use This File

### For Auditors
- Use these links as **reference material** when reviewing findings
- Do **NOT cite external links directly** in findings (use committed evidence instead)
- Example: In findings, cite **PKT files or test reports** as primary evidence
- Use standards references in compliance mapping section only

### For Operations
- Use vendor documentation when implementing production changes
- Follow CIS Benchmarks for hardening production infrastructure
- Implement features noted as "Production Requirement" in Packet Tracer Limitations section

### For Compliance
- Map findings to regulatory requirements using GDPR, NIS2, CyFun references
- Use ISO 27001 control numbers for control assessment
- Reference NIST CSF for risk management framework alignment

---

## Questions or Updates?

If any links are broken or outdated:
1. Check vendor websites directly (Cisco.com, ISO.org, etc.)
2. Verify URLs are current with site search functionality
3. Update this file with working URLs
4. Commit changes with note: "refactor: update external reference links"

**Last Updated:** September 2026

