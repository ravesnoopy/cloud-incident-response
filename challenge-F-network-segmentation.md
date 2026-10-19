# Challenge F — Recover with Network Segmentation Hardening

**Status:** Planned (evidence pending) · **Environment:** simulated lab

## 1. Scenario
EC2 security groups allow SSH, application and database ports from the internet (0.0.0.0/0). The attacker used these rules to reach the database tier directly and move laterally, bypassing the Application Load Balancer.

## 2. Objective
Restore a least-privilege three-tier flow: internet → ALB, ALB → web tier, web tier → database tier. Provide administrative access without inbound SSH.

## 3. AWS components
EC2 security groups (ALB, web, database), Application Load Balancer, AWS Systems Manager Session Manager, CloudTrail / VPC Flow Logs for understanding the attack path.

## 4. Planned approach
1. Review CloudTrail or VPC Flow Logs to understand how the attacker traversed the network.
2. Web tier: remove rules that allow SSH, the application port and broad TCP ranges from the internet; allow the application port **only from the ALB security group**.
3. Database tier: remove internet-facing rules; allow the database port **only from the web-tier security group**.
4. Confirm Session Manager provides administrative access with no inbound SSH.
5. Verify the application via the ALB endpoint and that the web tier still reaches the database.

## 5. Security rationale
- Referencing security groups (not CIDRs) ties access to the tier's identity rather than to addresses.
- Removing SSH eliminates a brute-force and lateral-movement path.
- Verification ensures the segmentation did not break the application.

## 6. Validation
- Web tier: no SSH or application port from 0.0.0.0/0; no wide TCP range; app port allowed from ALB SG.
- Database tier: no database port from 0.0.0.0/0; database port allowed from web SG.
- Application reachable through ALB; web-to-database connectivity intact.

## 7. Outcome
*Pending evidence.*

## 8. Lessons demonstrated
Segmentation is a design property (tier-to-tier references), not a list of blocked addresses.

## Illustrative reference commands (not evidence of execution)
```bash
aws ec2 revoke-security-group-ingress --group-id <web-sg> --protocol tcp --port 22 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-id <web-sg> --protocol tcp --port <app-port> --source-group <alb-sg>
aws ec2 authorize-security-group-ingress --group-id <db-sg> --protocol tcp --port <db-port> --source-group <web-sg>
aws ssm start-session --target <instance-id>
```

## Connection to Sprint 11
The Sprint 11 post-incident review found three RDP rules open to "anyone" on file servers, left over from the response, in a segment shared by nine sites. See [`../governance-and-lessons/cross-scenario-mapping.md`](../governance-and-lessons/cross-scenario-mapping.md).

## Evidence checklist
- [ ] Security group rules before and after (web, database)
- [ ] Session Manager session
- [ ] ALB endpoint test
- [ ] Lab "check" result
