# Publishing Guide

## Before the first push

- [ ] Read [`publication-checklist.md`](../governance-and-lessons/publication-checklist.md) end to end.
- [ ] Fill in `docs/02-aws-credential-context.md`.
- [ ] Rename screenshots (see `evidence/screenshots/README.md`) and complete `evidence/evidence-index.md`.
- [ ] Confirm that no screenshot shows account IDs, ARNs, access keys, emails or tokens.

## Commands

```bash
cd aws-ir-portfolio
git init
git add .
git status            # review every file that will be committed
git commit -m "Initial commit: cloud IR portfolio structure"
git branch -M main
git remote add origin https://github.com/<your-user>/<repo-name>.git
git push -u origin main
```

## Suggested repository settings

- **Name:** `cloud-incident-response-portfolio`
- **Description:** "AWS incident response and detection labs with a ransomware case study. Simulated training environments."
- **Topics:** `aws`, `incident-response`, `cloud-security`, `dfir`, `mitre-attack`, `athena`, `cloudtrail`, `iam`, `portfolio`

## Commit discipline

Update one document and its evidence reference per commit, e.g. `Add Challenge A evidence and promote status to Verified`.
