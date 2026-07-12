# Module: Metadata Intelligence

Part of [SIS MD](../SIS.md).

## Objective
Extract embedded metadata from files (documents, images, PDFs, office files, media) and evaluate what it reveals about the author, organization, or internal environment.

## What to Extract
- **Identity:** author names, usernames, organization/company name
- **Tooling:** software/tool + version used to create or edit the file
- **Paths:** internal file paths (e.g., `C:\Users\jdoe\Projects\internal-app\`)
- **Location:** GPS/EXIF location data (images)
- **Timestamps:** creation, modification, last-printed dates — useful for timeline reconstruction
- **Hidden content:** revision history, tracked changes, deleted-but-retained text, speaker notes, hidden slides/sheets
- **Comments:** embedded reviewer comments or annotations

## How to Report
For each finding, provide:
| Field | Value | Risk Note |
|---|---|---|
| e.g. `Last Author` | `jdoe` | Reveals internal username; low-severity OSINT/social-engineering value |

## Risk Notes Guidance
- Internal usernames/paths → low-to-medium: useful for social engineering or org-chart inference
- GPS data on images intended for public posting → medium-to-high: physical location exposure
- Revision history containing content the author meant to delete → severity depends on what's in it (could be Critical if it contains credentials or confidential data — in that case, cross-reference with the Secrets module)

## Boundaries
- Only extract what's present in the file; never guess or fabricate metadata.
- If metadata extraction requires a tool the AI doesn't have direct access to (e.g., raw EXIF parsing), state that clearly and suggest the user run a standard tool (`exiftool`, `mat2`) rather than inventing values.
