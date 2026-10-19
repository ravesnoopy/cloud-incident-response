# 02 · Attack Chain Reconstruction

**Evidence status:** Verified · **Source:** workbook sheet 2, screenshots (`Reconstruccion_de_cadena_*`)

## Scenario

Twenty SIEM alerts from the 11 days before encryption. Some form the attack chain; others are normal activity in a clinic network. Task: separate them, map the relevant ones to a stage and a MITRE ATT&CK technique, then choose the earliest effective intervention point.

## Step 1 — Signal vs. noise

**Relevant (10):** B01, B04, B05, B09, B13, B15, B17, B18, B19, B20
**Noise (10):** B02 (scheduled pentest scan), B03 (failed logins then success from usual location), B06 (AV signature update), B07 (TLS cert expiring), B08 (user-initiated password change), B10 (scheduled backup), B11 (patching in maintenance window), B12 (contractor VPN config download), B14 (VPN rejected, expired cert), B16 (license expiring).

## Step 2 — Chain and techniques

| ID | Stage | Technique | Reasoning |
|---|---|---|---|
| B01 | Initial access | T1566.002 Spearphishing Link | Fake billing portal collected credentials; no alert at the time, identified retrospectively from mail logs |
| B04 | Initial access | T1078 Valid Accounts | Service account `SVC-BACKUP-04` authenticated from unauthorized host `WKS-2231` |
| B05 | Discovery | T1087.002 Domain Account Discovery | Query for "Domain Admins" members from the same service account |
| B09 | Privilege escalation | T1136.002 Create Domain Account | `SVC-REPORT-11` created with no change ticket |
| B13 | Privilege escalation | T1078 Valid Accounts | `SVC-REPORT-11` authenticated on the secondary DC outside its usual reporting window |
| B15 | Lateral movement | T1021.001 Remote Desktop Protocol | `SRV-FILES-02` connected by RDP to four unrelated servers within 20 minutes |
| B17 | Lateral movement | *No specific technique* | 40 minutes of outbound traffic from `SRV-FILES-02` to an uncatalogued external IP; no evidence confirms C2 or exfiltration |
| B18 | Impact | *No specific technique* | 8-minute log-sync failure on `SRV-FILES-02`; cause not evidenced |
| B19 | Impact | T1486 Data Encrypted for Impact | Mass file modification across all 9 sites simultaneously |
| B20 | Impact | T1486 Data Encrypted for Impact | Ransom note file in multiple shared folders |

**Method note:** a technique describes attacker *behavior*, not a tool. Where the evidence did not confirm channel or method, I chose "no specific technique" instead of guessing (B17, B18).

## Step 3 — Earliest intervention point

**Chosen: B04** (service account authenticated from an unauthorized host).

**Reasoning:** earlier stages (phishing, silent credential theft) generate little or no visible signal for traditional monitoring. B04 plus B05 is the first anomaly with enough visibility for the SOC to act on: a privileged service account used from the wrong host, followed by an immediate domain-admin enumeration. Acting there means revoking the credential before account creation, lateral movement and encryption.

## Reviewer notes (limitations)

- B01 is the true earliest *event* but was undetectable at the time. The honest framing is: B01 is the earliest cause, B04 is the earliest **actionable** detection.
- Mapping B17 to "lateral movement" by stage is a structural choice; the evidence supports "suspicious outbound traffic", not a confirmed technique.

## Lesson

Good reconstruction is as much about stating what the evidence does *not* confirm as about the mapping itself.
