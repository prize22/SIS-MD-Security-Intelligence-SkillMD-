# SIS MD — Security Intelligence Skill

> A portable, model-agnostic Skill file that gives any LLM (Claude, ChatGPT, coding agents, etc.) structured security intelligence capabilities — metadata analysis, secret detection, and tech stack fingerprinting for developers, pentesters, and bug bounty hunters.

A portable, model-agnostic Skill file that turns any capable AI assistant (Claude, ChatGPT, OpenCode, or similar) into a structured **security intelligence processor**.

Instead of open-ended chat responses, SIS MD makes the AI apply consistent, repeatable analysis routines to files, source code, and web artifacts — returning structured findings a developer, pentester, or bug bounty hunter can immediately act on.

## What it does

SIS MD v1 covers three modules:

| Module | Purpose |
|---|---|
| [Metadata Intelligence](modules/metadata.md) | Extracts embedded metadata (authors, paths, GPS, timestamps, hidden revisions) and flags privacy/security exposure |
| [Secret & Sensitive Data Detection](modules/secrets.md) | Detects exposed API keys, credentials, private keys, internal URLs/IPs |
| [Technology & Infrastructure Fingerprinting](modules/fingerprinting.md) | Passively identifies frameworks, servers, cloud providers, CMS, and dependency versions |

All output follows a fixed report template (see [SIS.md](SIS.md#4-output-format)) so results are consistent regardless of which AI is running the skill.

See [`examples/sample-report.md`](examples/sample-report.md) for a full worked example.

## Who it's for

- Developers doing a pre-publish/pre-release security & privacy sanity check
- Penetration testers and red teamers documenting recon (authorized engagements only)
- Bug bounty hunters analyzing in-scope targets
- Security/DevSecOps teams running hygiene checks across repos and assets

## What it deliberately does NOT do

- No exploit generation, malware, or attack payloads
- No active scanning, brute-forcing, or unauthorized access attempts
- No reprinting of live secrets in full (all sensitive findings are masked in output)
- No fabricated CVE IDs — version risk is flagged in general terms only

This is a **passive, defensive analysis skill**. Authorization for the target/asset is the user's responsibility.

## How to use it

SIS MD is plain Markdown with no platform-specific syntax, so it works with any LLM or agent that can read instructions from text/files. General pattern: give the model `SIS.md` (plus relevant `modules/*.md` files) as context or instructions, then provide the file/text/URL you want analyzed.

### Chat-based assistants (Claude, ChatGPT, Gemini, and similar)
Upload the repo or paste the contents of `SIS.md` (and any modules you need) at the start of a session — as custom instructions, a system prompt, project/knowledge files, or a pinned message, depending on what the platform supports. Then provide the target file/text/URL.

### Coding agents & CLI tools (OpenCode, Claude Code, Cursor, Aider, Cline, and similar)
Reference `SIS.md` in the agent's system prompt, config file, or project-level rules/instructions file (e.g., `.cursorrules`, `AGENTS.md`, or the tool's equivalent), following that tool's convention for loading custom instructions or skills.

### API-based / custom integrations
Load `SIS.md` (and needed modules) as a system prompt or prepended context in your API calls to any LLM provider (Anthropic, OpenAI, Google, open-source models via vLLM/Ollama, etc.). Since it's just Markdown text, it works anywhere you can inject instructions ahead of a user's input.

### Anything else that reads Markdown instructions
If a platform supports custom instructions, system prompts, knowledge files, or project-level rules in any form, SIS MD will work — there's no dependency on a specific vendor's tooling or format.

## Repo structure
