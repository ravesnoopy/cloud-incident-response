# Cross-Scenario Mapping: Sprint 11 Findings ↔ AWS Controls

The Sprint 11 case is not an AWS environment. This mapping is **conceptual**: it shows that the *classes* of weakness found in the case review are the same classes the AWS challenges remediate.

| Sprint 11 finding | Weakness class | AWS challenge | Corresponding AWS control |
|---|---|---|---|
| `svc-legacy`: admin account, 311 days unused, no MFA | Dormant privileged identity | E | List and remove unused IAM users and keys; least-privilege roles |
| `SVC-REPORT-11` created by attacker, no change ticket | Attacker-created identity | E | Detect `CreateUser` / `CreateAccessKey` in CloudTrail; delete users, keys, policies |
| Three RDP rules open to "anyone" left from the response | Emergency rules never closed | F, A | Security-group references between tiers; network ACL review; AWS Config rule for restricted access |
| Files exposed to anyone with server access | Overexposed data | A | S3 Block Public Access (bucket and account) |
| Outbound traffic to an uncatalogued IP for 40 minutes | Unblocked external destination | A | Identify external IPs in logs; `/32` network ACL deny rules |
| Log-sync failure on `SRV-FILES-02` (8 min) | Audit gap | D | CloudTrail log-file validation; AWS Config recording; log-bucket hardening |
| Credentials not rotated | Stolen credentials stay valid | E | Rotate or remove keys; avoid long-lived keys |
| Attacker host `SRV-FILES-07` left in place | Residual attacker infrastructure | C | Forensic image, then isolate and terminate unauthorized instances |
| Shared segment across nine sites | Flat network | F | Tiered segmentation |
| Evidence preserved before containment (volatile memory) | Preserve before remove | A, B, C | Copy to forensic bucket; AMI before termination |

## Caveats

- Windows/AD concepts (domain accounts, RDP) are not identical to IAM/VPC constructs.
- This table states analogies; it does not claim the Sprint 11 environment was fixed using these AWS controls.
