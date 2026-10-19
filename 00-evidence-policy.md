# Evidence Policy

This repository separates **what was done**, **what is supported by evidence**, and **what is only planned**.

## Labels

| Label | Meaning | Allowed wording |
|---|---|---|
| **Verified** | A screenshot, tutor-reviewed deliverable or lab result supports the statement. | "I classified…", "The lab result showed…" |
| **Partial** | Evidence exists but is incomplete or inconsistent with another source. | "The deliverable records…; the lab report shows…" |
| **Planned** | Only the task statement or intended approach exists. | "The intended approach is…", "Expected validation is…" |

## Rules

1. Nothing is described as completed without a matching evidence reference in [`evidence/evidence-index.md`](../evidence/evidence-index.md).
2. Reference commands in AWS challenge documents are **illustrative**. They are not a log of executed commands.
3. Simulated interfaces are never presented as production AWS activity.
4. A learning activity, a passed assessment, a digital credential and an AWS certification are different things and are named precisely (see [`02-aws-credential-context.md`](02-aws-credential-context.md)).
5. Where the source material contained a gap or inconsistency, it is stated in a *Reviewer notes* section instead of being silently resolved.

## Promoting a "Planned" item to "Verified"

1. Add the screenshot or result to `evidence/screenshots/`.
2. Add a row to `evidence/evidence-index.md`.
3. Replace the *Planned approach* section of the challenge with the steps actually performed.
4. Change the status in the challenge header, in `aws-response-labs/README.md` and in the root `README.md`.
