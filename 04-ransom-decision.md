# 04 · Ransom Decision

**Evidence status:** Verified · **Source:** workbook sheet 4, lab report screenshot

## Scenario

Patient records are encrypted and the attackers demand a ransom equal to roughly one-third of the cost of rebuilding everything. A decision memo recommends whether to pay.

## Recommendation: **Do not pay** · Confidence: **High**

## Fact base

| Factor | Finding |
|---|---|
| Recovery guarantee | None. There is no contractual or legal mechanism that obliges a criminal group to keep its promise. |
| Legal risk | Legal could not confirm whether the group is linked to a sanctioned entity. Estimated 2–6 weeks to get a reliable answer; no way to accelerate it within 72 hours. |
| Double extortion | The leak sample published on the dark web includes real patient names, confirming data exfiltration. The attacker keeps that data whether or not payment is made. |
| Future incentive | Paying does not prevent re-attack on this or similar organizations and reinforces the criminal business model. |

## Additional information that would help

- Whether the group is linked to a sanctioned entity
- Backup and memory evidence status (preserved volatile evidence, backup integrity)

## Consultations

Legal, Finance, external negotiator.

## Executive message (BLUF)

Patient records are locked and the attackers demand a payment equal to one-third of full reconstruction cost. This is not over, but it is under control.

## Reviewer notes (limitations)

- The recommendation rests on the *absence* of guarantees and on exfiltration already being confirmed; it does not claim recovery is cheaper. A stronger memo would also quantify the recovery path (restore time from clean backups) against the demand.
- "Under control" in the executive message should be backed by a named condition (e.g., "no further hosts affected in the last X hours"); it was not.

## Lesson

A decision memo should state what is known, what is unknown, who must be consulted and what would change the recommendation.
