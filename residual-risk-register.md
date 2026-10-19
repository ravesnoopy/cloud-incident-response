# Residual Risk Register (Sprint 11 Case)

Residual risks recorded at the end of the post-incident review. The case ended with service open and several items "partial".

| ID | Risk | Evidence | Likelihood of abuse | Suggested action | Owner (per case) |
|---|---|---|---|---|---|
| R1 | `svc-legacy` admin account dormant 311 days, no MFA, credentials unchanged | Sweep filters `cuentas:sin-mfa`, `credenciales:sin-rotar` | High | Disable, then delete; confirm no dependency | Infrastructure |
| R2 | Attacker-created host `SRV-FILES-07` (Day 9, 03:12) | Persistence sweep | High | Preserve image, remove from network, rebuild | Security |
| R3 | RDP allowed from any source to `SRV-FILES-02/07/09` (Day 11) | Network sweep `red:ventana-incidente` | High | Remove rules; restrict RDP to jump hosts | Network / Security |
| R4 | Files exposed to anyone with server access | Access sweep | Medium | Reset permissions; review shares | Infrastructure |
| R5 | Most credentials unrotated | Filter `credenciales:sin-rotar` | High | Org-wide rotation; prioritize privileged and service accounts | Identity |
| R6 | Initial-access vector (phishing) not addressed by the response console | Prediction exercise, Day 1 | High | Mail gateway controls, link filtering, MFA | Security |

## Reading note

The case's own hint noted that not everything found is urgent. R1–R3 and R5 were the active-risk items; R4 and R6 are listed for completeness. Likelihood ratings here are my assessment, not from the source.
