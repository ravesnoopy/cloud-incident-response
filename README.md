# Cloud Incident Response Portfolio

**AWS Response & Detection labs, contextualized by a ransomware incident case study (Sprint 11)**

Author: **Leonardo Romero Garcia**

> **Simulated-environment notice.** Every exercise in this repository was performed in guided, simulated training laboratories (an interactive incident-response case and AWS Skill Builder challenges). Nothing here was executed in a production environment or a real AWS account, and no screenshot should be read as proof of production work.

---

## What this project is

A two-track portfolio that follows one incident-response story from decision-making to technical remediation:

| Track | Role in the portfolio | Folder |
|---|---|---|
| **AWS response labs (A–F)** | **Core track.** Technical response in AWS: evidence preservation, containment, eradication, hardening and audit controls. | [`aws-response-labs/`](aws-response-labs/) |
| **Sprint 11 case study** | **Context track.** A ransomware incident at a fictional 9-site healthcare network (*Red Salud Andina*): triage, attack-chain reconstruction, containment trade-offs, ransom decision and post-incident review. | [`sprint11-incident-context/`](sprint11-incident-context/) |

Conceptual workflow used throughout:

**Suspicious Activity → Detection → Investigation → Containment → Eradication → Hardening → Validation → Documentation**

## Competencies demonstrated

| Competency | Where to look | Evidence status |
|---|---|---|
| Alert triage and signal-vs-noise judgment | [`01-triage`](sprint11-incident-context/01-triage.md), [`02-attack-chain`](sprint11-incident-context/02-attack-chain.md) | Verified |
| MITRE ATT&CK mapping with explicit uncertainty | [`02-attack-chain`](sprint11-incident-context/02-attack-chain.md) | Verified |
| Containment trade-offs (spread vs. operational risk) | [`03-containment`](sprint11-incident-context/03-containment.md) | Verified |
| Decision-making under uncertainty (ransom decision) | [`04-ransom-decision`](sprint11-incident-context/04-ransom-decision.md) | Verified |
| Residual-risk identification after recovery | [`05-post-incident`](sprint11-incident-context/05-post-incident.md) | Verified |
| Forensic evidence preservation in AWS | [Challenges A, B, C](aws-response-labs/) | Planned (evidence pending) |
| Log analysis with Athena / CloudTrail | [Challenges A, C, E](aws-response-labs/) | Planned (evidence pending) |
| IAM least privilege and backdoor eradication | [Challenge E](aws-response-labs/challenge-E-iam-backdoors-least-privilege.md) | Planned (evidence pending) |
| Network segmentation (Security Groups, NACLs) | [Challenges A, F](aws-response-labs/) | Planned (evidence pending) |
| Audit and detective controls (CloudTrail, AWS Config) | [Challenge D](aws-response-labs/challenge-D-audit-and-compliance-controls.md) | Planned (evidence pending) |

### Evidence labels

- **Verified** – supported by a tutor-reviewed deliverable and/or screenshots in [`evidence/`](evidence/).
- **Partial** – some supporting evidence exists but it is incomplete or inconsistent.
- **Planned** – only the task statement exists. The approach is documented, but completion is **not** claimed.

Labels are updated as evidence is added. See [`docs/00-evidence-policy.md`](docs/00-evidence-policy.md).

## AWS challenge matrix

| ID | Challenge | Main AWS services | Response phase | Status |
|---|---|---|---|---|
| A | Contain S3 exfiltration and block attacker access | S3, Athena, CloudTrail, VPC Network ACLs | Preservation, containment | Planned |
| B | Secure the database and preserve forensic evidence | RDS, S3 | Preservation, hardening | Planned |
| C | Identify and remediate compromised compute (SSRF / IMDS) | EC2, AMI, Launch Templates, Auto Scaling, Athena | Investigation, containment | Planned |
| D | Implement audit and compliance controls | CloudTrail, S3, AWS Config | Detection, governance | Planned |
| E | Eradicate backdoor access, enforce least privilege | IAM, Systems Manager, Athena | Eradication | Planned |
| F | Recover with network segmentation | EC2 Security Groups, ALB, Session Manager | Recovery, hardening | Planned |

## How the two tracks connect

The Sprint 11 post-incident review found the same *classes* of weakness that the AWS challenges remediate: stale privileged service accounts, firewall rules opened during the response and never closed, unrotated credentials, and log gaps. See [`governance-and-lessons/cross-scenario-mapping.md`](governance-and-lessons/cross-scenario-mapping.md). The mapping is **conceptual**: the case study is a Windows/on-prem-style network, not an AWS environment.

## AWS credential context

Exact credential name, status and verification link: **pending** (see [`docs/02-aws-credential-context.md`](docs/02-aws-credential-context.md)). The portfolio is designed to stand on its technical reasoning and evidence, not on a badge.

## 5-minute reading path

1. This README
2. [`sprint11-incident-context/README.md`](sprint11-incident-context/README.md)
3. [`sprint11-incident-context/02-attack-chain.md`](sprint11-incident-context/02-attack-chain.md)
4. [`aws-response-labs/README.md`](aws-response-labs/README.md) and one challenge, e.g. [Challenge A](aws-response-labs/challenge-A-s3-exfiltration-containment.md)
5. [`governance-and-lessons/lessons-learned.md`](governance-and-lessons/lessons-learned.md)

## Repository map

```text
aws-ir-portfolio/
├── README.md
├── LICENSE
├── .gitignore
├── docs/                         # Evidence policy, credential context, source mapping, publishing guide
├── sprint11-incident-context/    # Ransomware case study (context track)
├── aws-response-labs/            # Challenges A–F (core track)
├── governance-and-lessons/       # Cross-mapping, residual risks, lessons learned
├── evidence/                     # Evidence index and screenshots
└── templates/                    # Reusable documentation templates
```

## Known limitations

- The AWS challenge documents currently describe the task and the intended approach; **completion evidence is being added**.
- The case study was originally written in Spanish and translated by the author. Technical meaning was preserved.
- Some exercise answers are judgment calls without a single correct answer; limitations are noted in each document.

## License

Documentation is released under the MIT License (see [`LICENSE`](LICENSE)). Third-party training content is not reproduced verbatim; scenarios are paraphrased.
