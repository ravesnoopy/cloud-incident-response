# Challenge A — Contain S3 Exfiltration and Block Attacker Access

**Status:** Planned (evidence pending) · **Environment:** simulated lab

## 1. Scenario
Customer data was exfiltrated using compromised credentials. The attacker created S3 buckets to stage the stolen data, and the legitimate customer-data bucket is publicly accessible. CloudTrail history in the console is unavailable, so logs must be queried through Athena.

## 2. Objective
Preserve evidence, remove attacker-created storage, close public access (bucket and account level) and block the attacker's external IPs at the network layer.

## 3. AWS components
Amazon Athena (saved queries in the analysis workgroup), AWS CloudTrail logs in S3, Amazon S3 (forensic bucket, customer-data bucket, attacker buckets), S3 Block Public Access (bucket and account), VPC network ACLs.

## 4. Planned approach
1. Confirm Athena access and the forensic bucket; collect the resource names (forensic bucket, customer-data bucket, VPC ID).
2. **Preserve:** copy the two evidence files from the attacker's bucket(s) to the forensic bucket *before deleting anything*.
3. Identify attacker-created buckets from CloudTrail (`CreateBucket`) and delete them after evidence is preserved.
4. On the customer-data bucket: enable all four Block Public Access settings and remove any bucket policy that grants access to all principals.
5. Enable all four Block Public Access settings at **account** level.
6. Query CloudTrail for S3 activity from external IPs; exclude `*.amazonaws.com` sources and private ranges (10.x, 172.16–31.x, 192.168.x).
7. Add **DENY** entries to the VPC network ACL using `/32` CIDRs for confirmed attacker IPs.

## 5. Security rationale
- Evidence first: deleting attacker buckets first would destroy proof of what was taken.
- Block Public Access at two levels prevents both a repeat of this exposure and future accidental exposure.
- `/32` rules are precise; excluding AWS service addresses avoids breaking legitimate service calls.

## 6. Validation (what evidence should show)
- Forensic bucket contains the preserved evidence files.
- No attacker buckets remain.
- Four Block Public Access settings are on (bucket and account); no principal-wide bucket policy.
- Network ACL shows DENY rules for each confirmed attacker IP.

## 7. Outcome
*Pending evidence.*

## 8. Lessons demonstrated
Preserve → remove → close exposure → block at the network, in that order, with logs as the source of truth for what to block.

## Illustrative reference commands (not evidence of execution)
```sql
-- Athena: external S3 activity (adapt database/table names)
SELECT sourceipaddress, eventname, COUNT(*) AS events
FROM <database>.<cloudtrail_table>
WHERE eventsource = 's3.amazonaws.com'
  AND sourceipaddress NOT LIKE '%.amazonaws.com'
  AND sourceipaddress NOT LIKE '10.%'
  AND sourceipaddress NOT LIKE '192.168.%'
GROUP BY sourceipaddress, eventname
ORDER BY events DESC;
```
```bash
aws s3 cp s3://<attacker-bucket>/<evidence-file> s3://<forensic-bucket>/
aws s3api put-public-access-block --bucket <customer-bucket> \
  --public-access-block-configuration BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
aws s3api delete-bucket-policy --bucket <customer-bucket>
aws s3control put-public-access-block --account-id <account-id> \
  --public-access-block-configuration BlockPublicAcls=true,IgnorePublicAcls=true,BlockPublicPolicy=true,RestrictPublicBuckets=true
aws ec2 create-network-acl-entry --network-acl-id <acl-id> --rule-number <n> \
  --protocol -1 --rule-action deny --ingress --cidr-block <attacker-ip>/32
```
*Note: the 172.16.0.0/12 private range needs a proper CIDR check; `LIKE` patterns are an approximation.*

## Evidence checklist
- [ ] Athena query and results (IP list)
- [ ] Forensic bucket contents
- [ ] Block Public Access status (bucket and account)
- [ ] Network ACL rules
- [ ] Lab "check" result
