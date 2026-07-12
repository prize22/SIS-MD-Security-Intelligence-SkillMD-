# Example: SIS MD in Action

**Scenario:** A user is about to open-source a repo and pastes in `config.py` plus their site's homepage HTML for a pre-publish check.

---

# SIS MD Security Intelligence Report

## Target Summary
- Source: `config.py`, `index.html` (pasted content)
- Analysis date: 2026-07-12
- Modules run: Metadata, Secrets, Fingerprinting

## 1. Metadata Findings
| Field | Value | Risk Note |
|---|---|---|
| No embedded document/image metadata present in pasted text content | — | N/A |

## 2. Secrets & Sensitive Data
| Type | Masked Preview | Location | Severity | Remediation |
|---|---|---|---|---|
| AWS Access Key | `AKIA****************MPLE` | `config.py:14` | Critical | Rotate key in IAM immediately; remove from source; use environment variables or a secrets manager |
| Hardcoded DB Password | `postgres://admin:****@db.internal:5432` | `config.py:22` | Critical | Rotate DB credentials; move to secrets manager; restrict `.internal` hostname exposure |
| Internal Hostname | `db.internal` | `config.py:22` | Medium | Confirm this isn't publicly resolvable; consider abstracting in public repo |

## 3. Technology Fingerprint
| Component | Version | Confidence | Note |
|---|---|---|---|
| WordPress | Not detected | — | No CMS signatures found |
| Bootstrap | 4.3.1 | High | Version string found in linked CSS comment |
| Server | nginx | Medium | Inferred from response header pattern in provided source |

## Overall Risk Rating: Critical

## Top 3 Recommended Actions
1. Rotate the exposed AWS key and database credentials before publishing — treat both as compromised.
2. Move all credentials to environment variables or a secrets manager (e.g., AWS Secrets Manager, Vault); scrub them from git history with `git filter-repo` or BFG before making the repo public.
3. Update Bootstrap to the latest 4.x or 5.x release to pick up accumulated security fixes.
