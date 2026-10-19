# Challenge C — Identify and Remediate Compromised Compute

**Status:** Planned (evidence pending; the source capture showed the challenge as "not yet checked") · **Environment:** simulated lab

## 1. Scenario
The web-server tier was compromised through server-side request forgery (SSRF) against the EC2 instance metadata service, exposing instance credentials. Unauthorized instances (crypto-mining) are running, the launch template permits insecure metadata access, and the Auto Scaling group uses an outdated template version.

## 2. Objective
Investigate, preserve forensic images, isolate and terminate unauthorized instances, and fix the root cause by enforcing IMDSv2 with a hop limit of 1.

## 3. AWS components
Amazon EC2, Amazon Machine Images (AMIs), launch templates, EC2 Auto Scaling, Athena or CloudWatch Logs Insights over CloudTrail, instance metadata options.

## 4. Planned approach
1. Query CloudTrail (Athena / Logs Insights) for `RunInstances` made with the compromised role; correlate launch times, instance metadata and the known legitimate fleet.
2. **Preserve:** create AMIs of affected instances *before* isolation or termination, tagged `incidenttype=crypto-mining` and `sourceinstance=<instance-id>`.
3. Isolate, then terminate unauthorized instances; confirm none remain running, pending, stopping or stopped.
4. Create a new launch template version with `HttpTokens=required` and `HttpPutResponseHopLimit=1`.
5. Point the Auto Scaling group to the latest version; refresh so every running instance uses it.
6. Verify IMDSv2 enforcement on the template and on running instances.

## 5. Security rationale
- IMDSv2 (session-token-based) blocks the simple SSRF-to-credentials pattern; a hop limit of 1 keeps the token from being reachable by containers or proxied requests.
- Forensic AMIs preserve disk state for analysis.
- Updating only the template without changing the ASG reference or refreshing instances would leave vulnerable instances running.

## 6. Validation
- A forensic AMI exists with both tags.
- No attacker-created instance in running / pending / stopping / stopped state.
- Template enforces IMDSv2, hop limit 1.
- ASG references the latest version; all instances use it.

## 7. Outcome
*Pending evidence.*

## 8. Lessons demonstrated
Root-cause remediation requires changing configuration *and* the deployment reference *and* the running fleet.

## Illustrative reference commands (not evidence of execution)
```bash
aws ec2 create-image --instance-id <instance-id> --name forensic-<instance-id> --no-reboot \
  --tag-specifications 'ResourceType=image,Tags=[{Key=incidenttype,Value=crypto-mining},{Key=sourceinstance,Value=<instance-id>}]'
aws ec2 create-launch-template-version --launch-template-id <lt-id> --source-version <n> \
  --launch-template-data '{"MetadataOptions":{"HttpTokens":"required","HttpPutResponseHopLimit":1,"HttpEndpoint":"enabled"}}'
aws autoscaling update-auto-scaling-group --auto-scaling-group-name <asg-name> \
  --launch-template LaunchTemplateId=<lt-id>,Version='$Latest'
aws autoscaling start-instance-refresh --auto-scaling-group-name <asg-name>
aws ec2 terminate-instances --instance-ids <attacker-instance-ids>
```

## Evidence checklist
- [ ] CloudTrail query showing unauthorized launches
- [ ] Forensic AMI with tags
- [ ] Instance list after cleanup
- [ ] Launch template metadata options
- [ ] ASG version reference and refresh
- [ ] Lab "check" result
