# Challenge D — Implement Audit and Compliance Controls

**Status:** Planned (evidence pending) · **Environment:** simulated lab

## 1. Scenario
CloudTrail runs without log-file validation, so an attacker could alter logs undetected. The CloudTrail log bucket lacks versioning, encryption and retention policy. AWS Config is not enabled, so there is no continuous compliance monitoring or change tracking.

## 2. Objective
Make audit records tamper-evident, harden the log bucket, enable configuration recording and deploy detective rules for the misconfigurations exploited in this incident.

## 3. AWS components
AWS CloudTrail, Amazon S3 (log bucket), AWS Config (recorder, delivery channel, managed rules), pre-provisioned IAM role.

## 4. Planned approach
1. Enable log-file validation on the trail and confirm it is actively logging.
2. On the log bucket: confirm encryption and add a lifecycle policy with a 90-day transition or expiration.
3. Create an AWS Config recorder named `default` using the provided role and bucket, recording all resource types, and start it.
4. Deploy managed rules covering: public S3 read, public S3 write, public RDS, restricted SSH, VPC flow logs, CloudTrail enabled.
5. Review the compliance dashboard to find resources still non-compliant from earlier challenges.

## 5. Security rationale
- Log validation makes tampering detectable.
- Encryption and lifecycle protect and bound log retention.
- Config provides continuous change tracking; rules turn each exploited misconfiguration into an alert.
- The compliance dashboard acts as a feedback loop on the earlier remediation.

## 6. Validation
- Trail shows validation on and logging active.
- Log bucket has encryption and the 90-day lifecycle rule.
- Recorder `default` is recording all resource types.
- Six managed rules exist; non-compliant resources reviewed.

## 7. Outcome
*Pending evidence.*

## 8. Lessons demonstrated
Detection controls should map one-to-one to the weaknesses an incident exploited.

## Illustrative reference commands (not evidence of execution)
```bash
aws cloudtrail update-trail --name <trail-name> --enable-log-file-validation
aws cloudtrail get-trail-status --name <trail-name>
aws configservice put-configuration-recorder \
  --configuration-recorder name=default,roleARN=<role-arn> \
  --recording-group allSupported=true,includeGlobalResourceTypes=true
aws configservice put-delivery-channel --delivery-channel name=default,s3BucketName=<config-bucket>
aws configservice start-configuration-recorder --configuration-recorder-name default
```
Managed rule identifiers commonly used for these controls: `S3_BUCKET_PUBLIC_READ_PROHIBITED`, `S3_BUCKET_PUBLIC_WRITE_PROHIBITED`, `RDS_INSTANCE_PUBLIC_ACCESS_CHECK`, `INCOMING_SSH_DISABLED`, `VPC_FLOW_LOGS_ENABLED`, `CLOUD_TRAIL_ENABLED`.

## Evidence checklist
- [ ] Trail settings
- [ ] Log bucket encryption and lifecycle
- [ ] Config recorder status
- [ ] Rules and compliance dashboard
- [ ] Lab "check" result
