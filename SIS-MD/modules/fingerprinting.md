# Module: Technology & Infrastructure Fingerprinting

Part of [SIS MD](../SIS.md).

## Objective
Passively identify the technology stack and infrastructure behind a target using only artifacts already provided (headers, HTML, manifests) — no active scanning or probing.

## What to Detect
- **HTTP headers:** `Server`, `X-Powered-By`, cookie naming conventions, security headers present/absent (`CSP`, `HSTS`, `X-Frame-Options`)
- **HTML source:** meta generator tags, JS framework signatures (React/Vue/Angular build artifacts), comments left in source
- **Cloud clues:** S3 bucket URL patterns, Azure Blob URLs, GCP Storage URLs, CDN hostnames
- **CMS/framework fingerprints:** WordPress paths (`/wp-content/`, `/wp-json/`), Laravel error pages, Django debug pages, common framework error signatures
- **Package manifests:** `package.json`, `requirements.txt`, `composer.json`, `Gemfile`, `go.mod` → language/framework/dependency version inventory
- **CDN/WAF indicators:** Cloudflare, Akamai, Fastly, AWS CloudFront headers

## How to Report
For each finding:
| Component | Version | Confidence | Note |
|---|---|---|---|
| WordPress | 6.2 (est.) | Medium | Detected via `/wp-content/` path structure and generator meta tag |

## Confidence Levels
- **High:** explicit version string found (e.g., generator meta tag, package manifest)
- **Medium:** inferred from structural/path patterns typical of that technology
- **Low:** weak circumstantial signal (e.g., generic header naming convention)

## Version Risk Notes
When a version is identified, note general risk in non-specific terms, e.g.:
> "This version is several major releases behind current; older releases in this line have had publicly disclosed vulnerabilities. Recommend checking the vendor's security advisories before relying on it in production."

Do not fabricate or guess specific CVE IDs. If the user wants a CVE cross-reference, direct them to check the NVD or vendor advisory page directly.

## Boundaries
- Passive analysis of provided artifacts only — no port scanning, no active requests to the target, no vulnerability scanning tools invoked on the AI's behalf.
- If the user wants active scanning, note that this falls outside SIS MD's scope and requires dedicated tooling (e.g., `nmap`, `whatweb`) run by the user themselves, with proper authorization.
