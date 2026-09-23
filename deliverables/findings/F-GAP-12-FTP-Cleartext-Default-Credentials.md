# F-GAP-12: Internet-Facing FTP Service Uses Cleartext Transfer and a Default-Style Account

**Status:** Additional finding, identified during evidence review of the Testing Report screenshots (pp. 28–29, 45). Not one of the original 15 checklist controls — added here as a supplementary finding rather than renumbering the existing checklist, so it does not disturb the "15 controls" counts used elsewhere in the audit. Raised by Sajjad Shahpoor (AAA / risk & reporting).

**Related area:** Data Protection / Perimeter Security (contract §2.3 requires "Secure FTP/SFTP services")

---

**Observation:**
The ASA firewall's outside-facing ACL permits FTP control and data traffic from the Internet directly to the DMZ-FTP server (192.168.80.11):

```
access-list OUTSIDE-IN extended permit tcp any host 192.168.80.11 eq ftp
access-list OUTSIDE-IN extended permit tcp any host 192.168.80.11 eq 20
```

FTP is a cleartext protocol: usernames, passwords and file contents are sent unencrypted. The FTP server's own user table shows an account named `cisco` with password `cisco`, holding **Read, Write, Delete, Rename and List** permissions, alongside a second account `nvidia_ftp` / `cisco` with Read, Write and List. A login test in the Testing Report succeeds using the `cisco` account. This account is reachable from the public Internet through the ACL above.

**Evidence:**
- `evidence/screenshots/ASA-OUTSIDE-IN-ACL-permits-FTP.jpg` — ASA `show access-list` output, confirming `ftp` (21) and `20` are permitted to 192.168.80.11 from any source.
- `evidence/screenshots/FTP-Server-User-Table-cisco-cisco-account.jpg` — FTP server Services tab, showing the `cisco` / `cisco` account with RWDNL permissions.
- `evidence/screenshots/FTP-Login-Using-cisco-Account.jpg` — a successful FTP login using the `cisco` account.
- Contract, §2.3 (Security Implementation): the contractor is required to deploy "Secure FTP/SFTP services."
- Packet Tracer Limitations, §10 (SFTP Service): "SFTP was unavailable in Packet Tracer server services. FTP was used as a substitute for file transfer simulation… SFTP would replace FTP for DMZ-FTP… instead of two unencrypted ports (20, 21)." — the dossier itself records this as a simulator substitution, not a deliberate design choice, which means the production system should not inherit it.

**Compliance Reference:**
- ISO/IEC 27001:2022 A.8.24 (Use of cryptography)
- ISO/IEC 27001:2022 A.5.17 (Authentication information)
- GDPR Article 32(1)(a) (encryption as an appropriate technical measure)
- NIS2 Directive Article 21(2)(h) (cryptography)

**Risk Rating:** **HIGH** (Likelihood 3 × Impact 2 = 6)
- *Likelihood 3:* The service is reachable directly from the Internet, uses a cleartext protocol, and one of its two accounts has a default-style, easily-guessed username. No control stands in the way of a credential-guessing or traffic-interception attempt.
- *Impact 2:* Limited to the DMZ-FTP service and whatever it holds, but the `cisco` account's delete and rename rights mean a successful login can destroy or replace files, not just read them.

**Recommendation:**
1. Replace FTP with SFTP (or FTPS) on the production DMZ-FTP server, as the contract already requires.
2. Remove the `cisco` account; issue each legitimate user a named account with a unique credential; remove delete/rename rights from any account that only needs to upload.
3. Update the ASA `OUTSIDE-IN` ACL to permit only the chosen secure port, and remove the `ftp` and `20` lines.
4. **Owner:** Network/Security team for the ACL and account changes; Contractor for enabling SFTP on the production FTP server.
5. **Timeline:** Before production go-live — this is a configuration change with no dependency on the other findings.
6. **Cost:** None. SFTP is supported on real production hardware; only the Packet Tracer simulator lacked it (Packet Tracer Limitations §10).
