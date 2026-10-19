# 06 · Real-Case Comparison — Change Healthcare (February 2024)

**Evidence status:** Verified (author's reflections) · **Source:** workbook sheet 6, lab "Exhibit H"

## Reference case (summary of what the lab presented)

In February 2024 a ransomware attack on Change Healthcare, a U.S. company that processes billing and authorizations for pharmacies and hospitals, disrupted payments, authorizations and medication dispatch for weeks. The group known as ALPHV/BlackCat exfiltrated a large volume of medical and financial data before encrypting systems. A ransom in the tens of millions of dollars was reportedly paid. Weeks later a second group (RansomHub) claimed to hold a copy of the stolen data and demanded another payment — a widely cited case of cascading double extortion.

## My comparison

| Point | My reflection |
|---|---|
| Attacker business model | Ransomware-as-a-Service; an attack dedicated to this type of infrastructure |
| Double extortion | A possible actor reusing the first attack's stolen resources to generate a second demand |
| Care continuity impact | Possible effect on billing and financial authorization availability |
| Paying, knowing what we know | Nothing different: there is no guarantee of not being hit again by double extortion |

## Reviewer notes (limitations)

- The answers are short. Stronger framing for each point:
  - *Business model:* RaaS separates affiliates from developers, so a payment to one party does not bind the others.
  - *Double extortion:* in Red Salud Andina the leak sample already proved exfiltration before any payment decision.
  - *Care continuity:* the Red Salud Andina scenario threatened clinical monitoring directly, not only administrative functions.
- The "double identity" wording in reflection 2 is imprecise; the documented pattern is that a **different** group claimed to hold the same data.
- Verify current public figures and attributions against primary sources before publishing numbers beyond this summary.

## Lesson

Paying for silence does not guarantee exclusivity of the data.
