# 05 · Post-Incident Review

**Evidence status:** Verified for the review; **recovery state is Partial by design** · **Source:** workbook sheet 5, lab report

## Phase 1 — Backup analysis and restoration

Four backups analyzed: **BK-A, BK-B, BK-C, BK-D**. Restored: **BK-B**.

**Justification:** BK-B corresponds to Day 4, is completely clean, has no accounts or scheduled tasks outside the approved inventory, shows no discrepancies in domain accounts (246/246) or scheduled tasks (89/89), and is free of the anomalous `SVC-REPORT-11` account or any task created during the response.

## Phase 2 — Residual-risk sweep

**Areas swept:** accounts, persistence, network, credentials, access
**Filters used:** `cuentas:sin-mfa`, `red:ventana-incidente`, `persistencia:sin-registro`, `credenciales:sin-rotar`

### Active-risk findings

| Finding | Detail | Why it matters |
|---|---|---|
| `svc-legacy` | Administrator privileges; last authentication 311 days ago; credentials not updated in 311 days; no MFA. Belonged to a shift-scheduling system migrated in 2024; no process depends on it. | A dormant privileged account is a ready-made entry point |
| `SRV-FILES-07` | Host created Day 9, 03:12 by the attacker | Attacker-controlled infrastructure left in place |
| Three firewall rules | `ANY → SRV-FILES-02:3389`, `ANY → SRV-FILES-07:3389` (Day 11, 08:22) and `ANY → SRV-FILES-09:3389` (08:24) opened during the response and never closed | Open RDP from anywhere |
| Public files | Files exposed to anyone with access to the server | Data exposure |
| Unrotated credentials | Most credentials not changed | Stolen credentials remain valid |

### Final state recorded by the lab

| Item | State |
|---|---|
| Service | Open |
| Firewall | Partial |
| Credentials | Partial |
| `svc-legacy` | Partial |
| `svc-report` | Partial |

## Prediction exercise

Predicted whether the attack would have been blocked had a control been applied at Day 1, 2, 5, 9 and 11. The lab recorded Day 1, 2, 5 and 9 as **not blocked** and Day 11 as **blocked**, and marked my predictions as matching.

**What stayed open:** initial access, use of stolen credentials and early lateral movement. The response prioritized neutralizing imminent threats on affected servers over invalidating domain credentials broadly.

**Different approach with the same 600-minute window:** early domain credential rotation and preventive isolation of compromised identities from the first signals, instead of focusing only on downstream effects.

**Day 1 control gap:** the entry vector (phishing) needs mail-layer controls (secure email gateway, link filtering, mailbox sanitization) and MFA, which sit outside what a host-level response console can do.

## Reviewer notes (limitations)

- The final state is intentionally unfinished (service open, several items "partial"). I do not present the recovery as complete.
- The text of the lab's "contrast" paragraph and my recorded prediction are not perfectly aligned; the match flag in the lab is the authoritative result.

## Lesson

Recovery is not closure. The review found that the dangerous leftovers were mundane: an unused admin account, rules opened in a hurry, and credentials nobody rotated.
