# Module: Secret & Sensitive Data Detection

Part of [SIS MD](../SIS.md).

## Objective
Scan text, code, or config content for exposed secrets and sensitive data that shouldn't be public or committed to version control.

## What to Detect
- **Cloud provider keys:** AWS (`AKIA...`), GCP service account JSON, Azure keys/connection strings
- **API keys/tokens:** generic `api_key=`, `Authorization: Bearer ...`, Slack (`xox[baprs]-`), GitHub (`ghp_`, `gho_`, `github_pat_`), Stripe (`sk_live_`, `pk_live_`), etc.
- **Private keys/certs:** `-----BEGIN PRIVATE KEY-----`, `.pem`/`.pfx`/`.key` file contents
- **Hardcoded credentials:** `password=`, `passwd=`, database connection strings with embedded username/password
- **Contact/internal info:** email addresses, internal hostnames/IPs, internal-only URLs (`.internal`, `.corp`, `10.x.x.x`, `192.168.x.x`, `172.16–31.x.x`)
- **Session artifacts:** session tokens, JWTs — flag and decode claims only if trivially base64-decodable; never attempt to crack or forge signatures

## How to Report
For each finding:
| Type | Masked Preview | Location | Severity | Remediation |
|---|---|---|---|---|
| AWS Access Key | `AKIA****************MPLE` | `config.py:14` | Critical | Rotate immediately, revoke in IAM, move to secrets manager |

## Severity Guide
- **Critical:** live-looking cloud/infra credentials, private keys, admin passwords
- **High:** API tokens with write/delete scope, database connection strings
- **Medium:** internal URLs/IPs, low-privilege API tokens, session tokens
- **Low:** email addresses, generic internal hostnames

## Masking Rule
Never reprint a live-looking secret in full. Show enough to identify the type/location (first 4–6 chars + last 4, rest masked with `*`). The report itself must not become a new leak vector.

## Boundaries
- Do not attempt to validate whether a found key is actually live (no network calls, no auth attempts) — treat all matches as "potentially live" and let the user verify/rotate.
- Do not attempt to decrypt, brute-force, or crack anything found.
- False positives are acceptable and expected — flag anything pattern-matching a secret and let the human confirm.
