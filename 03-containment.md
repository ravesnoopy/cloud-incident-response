# 03 · Containment

**Evidence status:** Partial (workbook and lab report differ on completion state) · **Source:** workbook sheet 3, lab report screenshots

## Scenario

Encryption is underway. The DFIR analyst asked for about 20 minutes to capture evidence before isolating; the Incident Commander ordered immediate isolation. A 45-second decision timer ran during the exercise.

## Decision under the timer

- My choice: **look for an intermediate alternative**.
- Files encrypted while I decided: **147**.
- The Commander's actual choice (isolate immediately) is evaluated below.

## Evaluating the Commander's decision

**Trade-off:** *spread risk* vs. *operational risk*.

- Isolating blindly, without confirming that `SRV-FILES-02` and three other hosts in the same segment were compromised, could have disrupted clinical operations if the alert had been benign.
- Waiting risked further spread across a shared segment serving nine sites.
- The team did not choose purely either way: a partial time window outside the playbook was granted through **a delegation from the crisis group**, protecting against propagation while **preserving volatile memory** for later reconstruction.

**Assessment:** defensible because (1) the evidence of compromise was strengthening by the minute, (2) the outbound RDP pattern on other hosts pointed to an active intruder, and (3) the evidence concern was addressed instead of ignored. The weak point is the break from playbook, which is acceptable only because it was explicitly delegated and logged.

## Memory dump analysis

| Process | PID | Malicious? | Reason |
|---|---|---|---|
| services.exe | 612 | No | Standard OS process, valid signature, expected parent |
| svchost.exe | 1044 | No | Same |
| spoolsv.exe | 1220 | No | Same |
| wmiprvse.exe | 2015 | No | Same |
| encryptor.exe | 4471 | **Yes** | File-encryption artifact launched from legitimate parent processes and tools |
| vssadmin.exe | 4502 | **Yes** | Used to delete volume shadow copies, destroying local restore points |
| taskkill.exe | 4510 | **Yes** | Forcibly closed `sqlbackup.exe`, removing a protective process |

**Why `vssadmin` matters later:** deleting shadow copies removes local recovery options, so recovery depends on offline or off-host backups. That is why backup analysis in the post-incident review is decisive.

## Analyst inbox prioritization (SOC analyst)

| Order | Request | Reason |
|---|---|---|
| 1 | Commander needs confirmation of other hosts with the RDP pattern (3 min) | Direct input to the immediate containment decision |
| 2 | Nobody is recording timestamps of verbal decisions in the chat | Clock drift and later reconstruction depend on it |
| 3 | Continuity coordinator wants a one-line update in 10 minutes | Needs to be accurate and BLUF-style, so it can wait a few minutes |
| 4 | Colleague at the North site repeatedly asks if their site is safe | A short holding message takes under a minute |

*Note:* the author recorded that the course theory suggests a different ordering. The reasoning above is the author's.

## Containment plan

| System | Action | Rationale |
|---|---|---|
| `SRV-FILES-02` | Isolate | Confirmed source of lateral movement |
| `SRV-FILES-07 / 09 / 12` | Isolate | Same segment and same RDP pattern |
| `IMG-SRV-07` | Isolate | Affected imaging server |
| Shift scheduling system | Monitor | Low-intensity active monitoring; low clinical cost |
| ICU monitoring, South site | Monitor | Clinical continuity prioritized |

**Simulation outcome (as presented by the lab):** 0 additional hosts compromised; 100% of ICU monitoring maintained; volatile evidence preserved on contained hosts.

## Reviewer notes (limitations)

- The lab's final report listed some systems as "not completed" (including `SRV-FILES-07/09/12` and the monitoring items) while the console log reported those hosts isolated. The workbook records the plan as I submitted it. Treat the **plan** as verified and the **execution state** as partial.
- The lab described the submitted plan as "in essence" the one the team implemented at 08:13.
- The playbook-checkpoint section was not reviewed for this document.
- **Author to confirm:** the workbook cell for "evaluation of the Commander's decision" showed only the question. The assessment above is drafted from the lab's trade-off text and the reasoning in your notes; replace it with your submitted answer if it differs.

## Lesson

Containment is a trade-off statement, not a switch. Naming what you protect (clinical continuity), what you risk (spread) and what you preserve (memory) makes the decision auditable.
