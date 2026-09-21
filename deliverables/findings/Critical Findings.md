# Key Audit Findings — NVIDIA Regional R&D Hub
 **Scale Applied:** 3×3=CRITICAL, 2×3=HIGH, 2×2=MEDIUM

---

## CRITICAL FINDINGS (Score 9)

### F-GAP-01: Centralized Logging Incomplete; Firewall and Switches Not Monitored

**Related Checks:** LM-01 (centralized logging), LM-02 (firewall logging)

**Observation:**  
Logging configuration is incomplete. Only Leaf-1 forwards events to the Syslog server (192.168.70.16). The ASA firewall does not send logs (logging commands rejected in simulation). Catalyst switches are not configured for Syslog (command not supported in Packet Tracer). As a result:
- Firewall access attempts are not audited.
- Switch configuration changes and port security events are not logged.
- DMZ attacks (if any occur) cannot be detected at the firewall level.

**Evidence:**
- Security Configuration §2.2: "only Leaf-1 currently configured"
- Security Configuration §3.2: "Syslog configuration… could not be fully realized"
- Packet Tracer Limitations §6–7: "logging command rejected… on Catalyst switches… on ASA Firewall"
- Testing Report: Edge Router test references Syslog IP 203.0.113.6 (which is the firewall's outside interface, not the Syslog server)

**Compliance Reference:**
- ISO 27001:2022 A.8.15 Logging (evidence of user actions)
- ISO 27001:2022 A.8.16 Monitoring of activities
- GDPR Article 32 (technical measures to detect breaches)
- NIS2 Directive Article 21(2)(b) (incident detection and response capability)

**Risk Rating:** **CRITICAL** (Likelihood 3 × Impact 3 = 9)
- *Likelihood 3:* No control prevents this. Logging can be deployed on any device; the contractor simply did not.
- *Impact 3:* Perimeter (firewall) and core (switches) events completely invisible. Incident detection depends on customer complaints, not proactive monitoring.

**Recommendation:**
1. **Production requirement:** Deploy Syslog agent on the ASA firewall and all 8 Layer 3 switches (2 Spines + 3 Leaves + Edge Router + ASA = 7 devices total sending to 192.168.70.16:514).
2. **Timeline:** Implement before production go-live.
3. **Owner:** Network/Security team. Contractor responsible for initial configuration.
4. **Verify:** `show logging` on ASA; `show run | inc logging` on Catalyst devices; confirm logs arrive at Syslog server with timestamps.
5. **Cost:** Included in current contract (no additional hardware).

---

## HIGH FINDINGS (Score 6)

### F-GAP-02: Department ACLs Configured but Not Enforced

**Related Check:** NS-02 (ACL enforcement)

**Observation:**  
Access Control Lists (ACLs) are created on Leaf-1 to segregate departments:
- VLAN10-ACL, VLAN20-ACL, …, VLAN60-ACL deny inter-department traffic; permit access to servers and DMZ.
- GUEST-ACL blocks guest access to internal networks.
- DMZ-ISOLATION blocks DMZ→internal traffic.

However, ACL enforcement on SVI (Switch Virtual Interface) interfaces is inconsistent:
- `show access-lists` confirms all ACL lines are present and correct.
- `show ip interface vlan X` shows "Inbound access list is not set" despite the `ip access-group` command being accepted.
- The GUEST-ACL is the exception: it **is** enforced and tested; guest pings to internal networks return "Destination host unreachable".
- All other SVI ACLs (VLANs 10–60, DMZ) are **not** enforced in the simulation.

**Evidence:**
- Security Configuration §1.4 (SVI ACL enforcement gap): "Inbound access list is not set"
- Security Configuration §1.4 (Packet Tracer limitation): "documented Catalyst 3650 SVI ACL enforcement limitation"
- Security Configuration §3.3 (DMZ ACL): "Configured correctly and verified… but not enforced"
- Packet Tracer Limitations §1–3: "PT Enforced: No" for department and DMZ ACLs; "Exception: GUEST-ACL is actively enforcing"

**Compliance Reference:**
- ISO 27001:2022 A.8.20 Network segregation controls
- ISO 27001:2022 A.8.22 Segregation of networks

**Risk Rating:** **HIGH** (Likelihood 2 × Impact 3 = 6)
- *Likelihood 2:* The control is documented and partially works (guest isolation). But enforcement is broken for 6 of 7 ACLs.
- *Impact 3:* Lateral movement between departments is not blocked. A compromised Production workstation can reach IT or Study networks.

**Recommendation:**
1. **Production requirement:** Verify on real Catalyst 3650 hardware that SVI ACL enforcement works (it should; this is a Packet Tracer simulator bug, not a device issue).
2. **Alternative:** If hardware testing shows the same gap, move ACL enforcement to the firewall (external ACLs on the DMZ interface) or implement a VLAN access control protocol (e.g., 802.1AE MAC security).
3. **Timeline:** Must be validated before production go-live. Decision required: accept hardware-based workaround or re-architect.
4. **Owner:** Network team (testing); Contractor (architecture review).
5. **Cost:** No cost if hardware works as expected. If workaround required, estimate ~10 days design + testing.

---

### F-GAP-03: DMZ Isolation Depends on Non-Enforced ACL; Two-Interface Firewall Limits Segmentation

**Related Check:** NS-03 (DMZ isolation)

**Observation:**  
The DMZ is separated from internal networks via:
1. VLAN 80 (Layer 2 isolation on Leaf-3).
2. DMZ-ISOLATION ACL denying DMZ→internal traffic (not enforced, per F-GAP-02).
3. ASA firewall with two interfaces: outside (203.0.113.0/30) and inside (10.0.3.0/30).

However, the firewall is **not positioned between the DMZ and internal networks**. Instead:
- Leaf-3 connects to the ASA's inside interface.
- All internal traffic (VLANs 10–60, 70, 90) flows through Leaf-3.
- DMZ (VLAN 80) also sits on Leaf-3 and can reach internal subnets without passing through the firewall.

Result: DMZ↔Internal traffic is **not inspected**. Only Internet↔DMZ traffic is firewalled.

**Evidence:**
- Security Configuration §10 (Network Architecture): "two-interface ASA… all internal and DMZ subnets converge on Leaf-3 SVI"
- Device Configuration §5 (Leaf-3): "Gig1/0/3 → ASA Firewall Gig1/2 (inside)"
- VLAN Worksheet: DMZ (VLAN 80) and internal VLANs (10–70, 90) all on Leaf-3
- Packet Tracer Limitations §3: DMZ-ISOLATION ACL not enforced

**Compliance Reference:**
- ISO 27001:2022 A.8.20 Network segregation
- ISO 27001:2022 A.8.22 Segregation of networks (DMZ from internal)
- NIS2 Directive Article 21 (network security, defense in depth)

**Risk Rating:** **HIGH** (Likelihood 2 × Impact 3 = 6)
- *Likelihood 2:* The DMZ is on the same switch (Leaf-3) as the routing to internal networks. A VLAN hop or ACL bypass allows direct DMZ→internal access.
- *Impact 3:* If a DMZ server (e.g., web server) is compromised, attacker gains direct access to all internal subnets (servers, IT, production).

**Recommendation:**
1. **Production requirement:** Place the ASA with a dedicated DMZ interface (e.g., Gig1/3) between Leaf-3 (DMZ) and the internal network, creating a three-interface firewall topology: outside, DMZ, inside.
   - Alternative (lower cost): Use Leaf-3 as a trunk-only device and place DMZ on a separate access switch with firewall in-line.
2. **Cost estimate:** ~€5–10k for three-interface ASA upgrade or additional switching hardware.
3. **Timeline:** Before production go-live; requires contract revision and hardware procurement (5–10 days).
4. **Owner:** Contractor (re-architecture); NVIDIA (budget approval).
5. **Verify:** Confirm DMZ traffic must pass through firewall (protocol analyzer test).

---

### F-GAP-04: SSH Management Not Deployed to 5 of 8 Layer 3 Devices

**Related Check:** AC-01 (encrypted management)

**Observation:**  
SSH is configured on only 3 of 8 Layer 3 devices:
- ✅ Leaf-1 (COMPUTE): SSH enabled (Device Config §3.7)
- ✅ Leaf-2 (SERVICES): SSH enabled (Device Config §4)
- ✅ Edge Router: SSH enabled (Device Config §7)
- ❌ Spine-1: NO SSH config
- ❌ Spine-2: NO SSH config
- ❌ Leaf-3 (EDGE): NO SSH config
- ❌ ASA Firewall: NO SSH config
- ❌ Access switches (6×): NO SSH/VTY config

Telnet is not explicitly disabled on the devices without SSH, so it remains the default management protocol.

**Evidence:**
- Device Configuration §1 (Spine-1): No mention of SSH, VTY, or crypto key generation.
- Device Configuration §2 (Spine-2): Same—no SSH configuration.
- Device Configuration §5 (Leaf-3): No SSH configuration.
- Device Configuration §8 (ASA Firewall): No SSH or VTY lines configured.
- Device Configuration §6 (Access Switches): "No SSH/VTY configuration" noted in the source document.

**Compliance Reference:**
- ISO 27001:2022 A.8.5 Access to networks (secure authentication, encrypted channels)
- GDPR Article 32 (encryption of data in transit)
- NIS2 Directive Article 21(2)(h) (cryptographic techniques)

**Risk Rating:** **HIGH** (Likelihood 2 × Impact 3 = 6)
- *Likelihood 2:* Telnet is a known-vulnerable protocol (plaintext credentials). An attacker on the network can capture passwords.
- *Impact 3:* Compromised network device credentials can lead to network re-configuration, route hijacking, or complete infrastructure takeover.

**Recommendation:**
1. **Production requirement:** Enable SSH version 2 on all 8 Layer 3 devices. Disable Telnet (line vty 0 15 transport input ssh).
2. **Configuration template:** Use the Leaf-1/Leaf-2 SSH setup as a template for the Spines and Leaf-3.
3. **Timeline:** 2–3 hours configuration + testing. Can be done post-deployment if needed.
4. **Owner:** Network team.
5. **Cost:** No additional hardware. Included in support contract.
6. **Verify:** `show ip ssh` or equivalent; confirm Telnet is disabled; test SSH login from management VLAN (VLAN 99).

---

### F-GAP-05: RADIUS Authentication Non-Functional; Local Credentials as Fallback; Weak Key Material

**Related Check:** AC-02 (centralized authentication)

**Observation:**  
The AAA (Authentication, Authorization, and Accounting) framework is configured with RADIUS as the primary method and local usernames as fallback. However:

1. **RADIUS does not work:** The `radius-server host 192.168.70.12 auth-port 1645 key Cisco123` command is rejected on Leaf-1 with "Invalid input detected" (Packet Tracer limitation). As a result, the RADIUS pool is empty and all authentication falls back to local usernames.

2. **Local credentials are weak:**
   - Username: `admin` (default).
   - Password: `Cisco123` (8 characters, dictionary word, same as the RADIUS key in the configuration examples).
   - RSA key: 1024 bits (deprecated; industry standard is 2048+).
   - No account lockout, no session timeout on console.

3. **Only 3 of 8 devices have SSH/AAA configured** (see F-GAP-04), so the Spines and Leaf-3 have no documented authentication at all (assume default factory login or no login).

**Evidence:**
- Security Configuration §7.4: "RADIUS… does not function. Local fallback is the only working authentication path."
- Packet Tracer Limitations §4: "radius-server host command rejected… not supported in PT."
- Device Configuration §3.7 (Leaf-1 SSH): "username admin privilege 15 secret Cisco123; …crypto key generate rsa 1024"
- Device Configuration §8 (ASA): No username or crypto config shown.

**Compliance Reference:**
- ISO 27001:2022 A.5.17 Authentication information (strength of authentication)
- ISO 27001:2022 A.8.2 Privileged access (MFA where appropriate)
- ISO 27001:2022 A.8.5 Access control (encryption of credentials in transit)
- GDPR Article 32(1)(b) (ensuring ongoing confidentiality of credentials)
- NIS2 Directive Article 21(2)(i) (strong authentication for privileged users)

**Risk Rating:** **HIGH** (Likelihood 2 × Impact 3 = 6)
- *Likelihood 2:* Weak local credentials can be cracked (8 chars, dictionary word, same value used in code examples).
- *Impact 3:* Full administrative access to the network infrastructure.

**Recommendation:**
1. **Immediate (production):** Deploy a real RADIUS server (not PT simulation) and test authentication chain (device → RADIUS → user database).
2. **Short-term (weeks):** Enforce strong local credentials as backup only:
   - Minimum 12 characters.
   - Mix of upper/lowercase, digits, special characters.
   - Unique per device (no copy-paste).
   - Different from any default or documentation example.
3. **Crypto:** Update RSA key generation to 2048+ bits.
4. **Access:** Consider Multi-Factor Authentication (MFA) for privileged users (SSH certificate + RADIUS token).
5. **Timeline:** RADIUS deployment required before production. Local credential upgrade can follow within 30 days.
6. **Owner:** Security team (credential policy); Network team (implementation).
7. **Cost:** RADIUS server hardware/license already budgeted. MFA would require additional ~€5–10k (tokens, server).
8. **Verify:** RADIUS authentication test; failed login attempt; audit Syslog for auth failures.

---

## SUMMARY TABLE

| Finding | Check(s) | Risk | Likelihood | Impact | Score | Status |
|---------|----------|------|-----------|--------|-------|--------|
| **F-GAP-01** Logging incomplete | LM-01, LM-02 | CRITICAL | 3 | 3 | **9** | Must fix before production |
| **F-GAP-02** ACLs not enforced | NS-02 | HIGH | 2 | 3 | **6** | Verify on real hardware |
| **F-GAP-03** DMZ topology weak | NS-03 | HIGH | 2 | 3 | **6** | Requires re-architecture |
| **F-GAP-04** SSH partial | AC-01 | HIGH | 2 | 3 | **6** | Quick win (2–3 hrs config) |
| **F-GAP-05** RADIUS + weak credentials | AC-02 | HIGH | 2 | 3 | **6** | Deploy real RADIUS server |

---

## Evidence Quality Notes

The dossier contradicts itself in several places:
- Syslog server IP: listed as both 192.168.70.16 (correct per VLAN worksheet) and 203.0.113.6 (which is the firewall's outside IP).
- RADIUS status: "PASS" in Testing Report §9.2 vs. "does not function" in Security Configuration §7.4.
- Department count: "9 VLANs" in some places, "10 VLANs" in others (VLAN 1 confusion from the hardening gap).

These inconsistencies should be resolved before production validation.