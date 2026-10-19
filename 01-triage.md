# 01 · Triage Exercise

**Evidence status:** Verified · **Source:** workbook sheet 1, lab screenshots (`Ejercicio_de_triaje_*`)

## Scenario

Before looking at the case, a calibration exercise: six events presented with the same urgency style as real alerts. Classify each as **routine** or **crisis** and state the criteria used.

## Criteria

| Criterion | Question |
|---|---|
| Scope | How many systems, users or sites are affected, or could be? |
| Reversibility | Can the effect be undone, or is damage accumulating? |
| Business function | Is a clinical or critical function impaired? |
| Time pressure | Does delay materially increase harm? |

## My classification

| Event | Class | Criteria applied |
|---|---|---|
| Specialist cannot access a patient's history; surgery in 20 minutes | **Crisis** | Time pressure |
| Unknown process renaming files on the Sede Norte imaging server; rate rose from 12 to 80 files/min in 5 minutes | **Crisis** | Time pressure, reversibility, scope |
| Physician account: successful logins from two cities 40 minutes apart; no other anomaly | Routine | Time pressure, scope |
| Prescription printer unresponsive; paper backup working | Routine | Business function |
| Macro attachment automatically quarantined; external unknown sender | Routine | Scope, reversibility |
| E-billing system showing intermittent error for one hour | Routine / Crisis (hedged) | Business function, time pressure |

## Time reconciliation

The Incident Commander recalled "about fifteen minutes" between the DFIR analyst's request and the Security Director's confirmation of isolation. The team chat timestamps show **9 minutes**.

**Why the gap matters (author's reasoning, edited for clarity):** recollection under pressure is unreliable, and post-incident conclusions, accountability and any legal exposure depend on exact timestamps. During containment the SOC analyst reported at 08:08 that she could confirm within 3 minutes which other hosts showed the same outbound RDP pattern. That fact points to an attacker already inside the environment, so decisions made in that window must be reconstructed from timestamps, not memory.

## Reviewer notes (limitations)

- **Impossible-travel login:** I classified it as routine because no other anomaly existed. A reviewer could reasonably argue it warrants investigation (credential theft is a common cause). The defensible position is "routine *pending* a quick account check", which I did not state.
- **E-billing event:** the hedged "Routine / Crisis" answer shows uncertainty rather than a decision. The lab's final state placed it under routine. A stronger answer names the single fact that would flip it (e.g., whether the failure correlates with other anomalies).
- The unknown-process and surgery events were the two crises in the lab's final state.

## Lesson

Criteria must be applied explicitly, and a hedge should name the fact that would resolve it.
