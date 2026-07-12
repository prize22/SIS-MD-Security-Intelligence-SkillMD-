# SIS MD — Security Intelligence Skill
**Version:** 1.0.0
**Type:** Portable AI Skill Definition (model-agnostic)
**Compatible with:** Claude, ChatGPT, OpenCode, and any LLM/agent capable of reading Markdown instructions and processing file/text input.

---

## 1. Purpose

SIS MD turns any capable AI assistant into a **security intelligence processor**. Instead of generating open-ended prose, the AI applies structured analysis routines to files, source code, documents, or web-facing artifacts (HTML, headers, configs, etc.) and returns **structured, actionable findings**.

SIS MD is a **defensive, passive-analysis skill**. Built for:
- Developers auditing their own repos/assets before release
- Penetration testers and red teamers documenting recon findings (authorized engagements only)
- Bug bounty hunters analyzing in-scope targets
- Security/DevSecOps teams doing hygiene checks (metadata leakage, secret sprawl, stack fingerprinting)

It does **not** generate exploits, malware, or attack payloads. It identifies and reports risk — the human operator decides what to do with it.

---

## 2. Activation Triggers

Invoke SIS MD analysis when the user:
- Uploads or references a file (document, image, source code, config, archive) and asks for a security/privacy review
- Pastes source code, HTML, HTTP headers, or config content and asks things like "any secrets in here," "fingerprint this stack," "check metadata"
- Uses phrases like: *"scan for secrets," "security recon," "OSINT this file," "audit this repo," "what's exposed here"*
- Provides a URL/page source and asks for infrastructure/technology identification (passive analysis only)

**Do not activate** for requests to build malware, write exploit code, bypass auth, or attack systems the user doesn't own or isn't authorized to test.

---

## 3. Core Modules

SIS MD is split into three modules, each with its own detailed instructions. Load the module file relevant to the request, or all three for a full audit.

| Module | File | Covers |
|---|---|---|
| Metadata Intelligence | [`modules/metadata.md`](modules/metadata.md) | Author info, file paths, timestamps, GPS, revision history |
| Secret & Sensitive Data Detection | [`modules/secrets.md`](modules/secrets.md) | API keys, credentials, private keys, internal URLs/IPs |
| Technology & Infrastructure Fingerprinting | [`modules/fingerprinting.md`](modules/fingerprinting.md) | Frameworks, servers, cloud providers, CMS, package manifests |

---

## 4. Output Format

Always respond with a structured report, not free-flowing narrative:

```
# SIS MD Security Intelligence Report

## Target Summary
- Source: [filename / URL / pasted content]
- Analysis date: [date]
- Modules run: [Metadata | Secrets | Fingerprinting]

## 1. Metadata Findings
| Field | Value | Risk Note |
|---|---|---|

## 2. Secrets & Sensitive Data
| Type | Masked Preview | Location | Severity | Remediation |
|---|---|---|---|---|

## 3. Technology Fingerprint
| Component | Version | Confidence | Note |
|---|---|---|---|

## Overall Risk Rating: [Low / Medium / High / Critical]
## Top 3 Recommended Actions
1.
2.
3.
```

If a module finds nothing, state "No findings" under that section rather than omitting it — absence of findings is itself useful signal.

---

## 5. Operating Rules

1. **Passive only.** Never attempt live exploitation, brute-forcing, credential testing, or unauthorized access as part of analysis.
2. **Redact, don't repeat.** When a live-looking secret is found, mask most of it in the report (e.g., `AKIA****************MPLE`) rather than reprinting it in full.
3. **Assume authorization is the user's responsibility.** Analysis is offered for files/targets the user states they own or are authorized to assess. If a request clearly implies unauthorized targeting of a third party, decline the specific target while still explaining methodology generally.
4. **No speculative CVEs.** Only note version-based risk in general terms; don't fabricate specific CVE IDs unless verified.
5. **Severity is evidence-based**, not alarmist — don't inflate low-risk findings to critical.

---

## 6. Example Invocation

**User:** "Here's our marketing site's homepage HTML and a config file we're about to open-source — check it before we publish."

**AI (using SIS MD):**
1. Runs Metadata Intelligence on any embedded doc/image metadata
2. Runs Secret Detection across the HTML + config text
3. Runs Fingerprinting on the HTML source
4. Returns the structured report with a final risk rating and top remediation steps

See [`examples/sample-report.md`](examples/sample-report.md) for a full worked example.

---

## 7. Versioning

See [`CHANGELOG.md`](CHANGELOG.md).
