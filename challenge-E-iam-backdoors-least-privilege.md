# Challenge E — Eradicate Backdoor Access and Enforce Least Privilege

**Status:** Planned (evidence pending) · **Environment:** simulated lab

## 1. Scenario
The attacker established persistence through backdoor IAM users and malicious AWS Systems Manager documents. Overly permissive EC2 instance-role policies contributed to the initial compromise and allowed access to S3 data and IAM enumeration.

## 2. Objective
Find and remove every persistence mechanism, then replace the instance role's broad permissions with a minimal policy.

## 3. AWS components
IAM (users, access keys, policies, roles), AWS Systems Manager documents, Athena or CloudWatch Logs Insights over CloudTrail.

## 4. Planned approach
1. Query CloudTrail for IAM and SSM changes (user creation, key creation, policy attachment, document creation).
2. For each unauthorized IAM user: delete access keys, detach (and remove inline) policies, remove login profile if present, delete the user.
3. List self-owned SSM documents; inspect for command-and-control content; delete unauthorized ones.
4. On the web-server instance role: detach overly broad policies (e.g., full S3 access, IAM read-only) and attach the pre-built minimal policy.

## 5. Security rationale
- Deleting users without removing keys and policies first fails or leaves residue; order matters.
- Malicious SSM documents can re-establish control even after instances are rebuilt.
- Least privilege on the instance role limits what any future credential theft can reach.

## 6. Validation
- No backdoor IAM users remain (including keys and policies).
- No unauthorized SSM documents remain.
- Instance role has only the minimal policy and none of the broad ones.

## 7. Outcome
*Pending evidence.*

## 8. Lessons demonstrated
Eradication means removing every persistence path, and least privilege is the control that shrinks the next incident.

## Illustrative reference commands (not evidence of execution)
```bash
aws iam list-users
aws iam list-access-keys --user-name <user>
aws iam delete-access-key --user-name <user> --access-key-id <key-id>
aws iam list-attached-user-policies --user-name <user>
aws iam detach-user-policy --user-name <user> --policy-arn <policy-arn>
aws iam delete-user --user-name <user>
aws ssm list-documents --filters Key=Owner,Values=Self
aws ssm delete-document --name <document-name>
aws iam detach-role-policy --role-name <role> --policy-arn <broad-policy-arn>
aws iam attach-role-policy --role-name <role> --policy-arn <minimal-policy-arn>
```

## Connection to Sprint 11
Compare with the attacker-created domain account `SVC-REPORT-11` and the dormant privileged account `svc-legacy` in [`../sprint11-incident-context/05-post-incident.md`](../sprint11-incident-context/05-post-incident.md). The identity classes are analogous, not identical.

## Evidence checklist
- [ ] CloudTrail query showing unauthorized IAM/SSM changes
- [ ] IAM user list before and after
- [ ] SSM document list before and after
- [ ] Instance role attached policies
- [ ] Lab "check" result
