# Lessons Learned

## From the Sprint 11 case

1. **Memory distorts time.** Fifteen minutes recalled vs. nine minutes recorded. Timestamps are evidence; recollection is not.
2. **Name the earliest *actionable* signal.** The earliest cause (phishing) was invisible; the earliest actionable detection was an out-of-place service account.
3. **Don't over-claim on a technique.** Mapping to MITRE only where the evidence confirms behavior keeps conclusions honest.
4. **Containment is a trade-off statement.** Spread risk vs. clinical continuity, with evidence preservation named explicitly.
5. **Decision memos need known / unknown / who-to-consult.** The "do not pay" recommendation rested on missing guarantees and confirmed exfiltration.
6. **Recovery is not closure.** The residual risks were mundane: dormant admin account, response-time firewall rules, unrotated credentials.

## From the AWS challenges (planned approach)

1. **Preserve before you delete** (A, B, C).
2. **Let the logs choose the targets** (A, C, E): query, don't guess, and don't block AWS service traffic.
3. **Fix configuration, reference and running fleet** (C).
4. **Remove every persistence path** (E).
5. **Segment by tier identity, not by address** (F).
6. **Convert each exploited misconfiguration into a detective rule** (D).

## Where I would improve

- Triage: state the quick check that would resolve a hedge (e.g., the impossible-travel login).
- Decision memo: add a quantified recovery path and a stated condition for "under control".
- AWS: add real evidence for each challenge and replace "planned" sections with performed steps.
