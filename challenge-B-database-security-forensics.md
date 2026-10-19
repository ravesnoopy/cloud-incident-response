# Challenge B — Secure the Database and Preserve Forensic Evidence

**Status:** Planned (evidence pending) · **Environment:** simulated lab

## 1. Scenario
An Amazon RDS instance is publicly accessible, has no backup retention and no deletion protection. Database records were exfiltrated to an attacker-created S3 bucket.

## 2. Objective
Preserve the exfiltrated dump as evidence, remove the attacker's bucket, and harden RDS so access is limited to the VPC, with recoverability and protection against deletion.

## 3. AWS components
Amazon RDS, Amazon S3 (attacker bucket, forensic bucket), VPC private connectivity.

## 4. Planned approach
1. Identify the attacker's bucket holding the database dump files.
2. Copy the SQL and JSON dump files to the forensic bucket **before** any deletion.
3. Delete the attacker's exfiltration bucket.
4. Modify the RDS instance: disable public accessibility, set backup retention to at least 7 days, enable deletion protection.
5. Allow 5–10 minutes for the modification to apply; continue other work meanwhile.
6. Verify that the application still reaches the database through the private VPC path.

## 5. Security rationale
- Removing public access closes the direct internet path.
- Backup retention supports investigation and recovery.
- Deletion protection mitigates accidental or malicious deletion.
- Verification prevents a hardening change from causing an outage.

## 6. Validation
- Forensic bucket holds both dump files.
- Attacker bucket is gone.
- RDS shows not publicly accessible, retention ≥ 7 days, deletion protection on.
- Application connectivity confirmed over the private path.

## 7. Outcome
*Pending evidence.*

## 8. Lessons demonstrated
The same "preserve before remove" principle as Challenge A, plus: hardening must be verified against application availability.

## Illustrative reference commands (not evidence of execution)
```bash
aws rds modify-db-instance --db-instance-identifier <db-id> \
  --no-publicly-accessible --backup-retention-period 7 \
  --deletion-protection --apply-immediately
aws rds describe-db-instances --db-instance-identifier <db-id> \
  --query 'DBInstances[0].[PubliclyAccessible,BackupRetentionPeriod,DeletionProtection]'
```

## Evidence checklist
- [ ] Forensic bucket contents
- [ ] RDS configuration before and after
- [ ] Application connectivity check
- [ ] Lab "check" result
