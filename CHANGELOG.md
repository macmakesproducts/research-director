# Changelog

## v1.0.1 — 2026-04-23

### Fixed
- Shrunk YAML frontmatter description from 1,087 chars to 232 chars. Fits under Claude Code's 250-char skill-listing cap (introduced in Claude Code 2.1.86) so the skill loads cleanly when invoked by chat routing rather than manual `/skill` call. First-user report showed the skill throwing *"Tool result could not be submitted"* errors on invocation; root cause was description-length truncation mid-parse rather than runtime behavior.

All notable changes to Research Director will be documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) loosely and this project adheres to semantic versioning.

---

## [1.0.0-beta] — 2026-04-21

### Added
- Initial public release. v1 beta — early and incomplete by design.
- Four-phase flow: Intake → Plan → Execute → Deliver.
- Adaptive intake (0–3 questions) probing audience/decision, scope, and depth/time sensitivity.
- `run as-is` invocation bypass for users who want to skip intake (plan is still shown before execution).
- Scope discipline at the plan step — threshold check (temporal span, entity breadth, question density, source heterogeneity, recursive expansion) recommends decomposition for oversized topics instead of thinning single-pass output.
- Retry-with-splitting during execution for thin output, truncation, scope drift, or hallucination pressure.
- Structured output contract with five sections: executive summary, key findings (with source + confidence signal per finding), open questions/gaps, recommended next steps, sources.
- Four example invocations in SKILL.md covering market entry, tool-switching, professional-prep, and civic/policy.
- Skill-assisted feedback composition: after delivering a brief, the skill offers to compose a pre-filled feedback email block (question + summary + friction points + gaps + blank reaction line) that the user can copy, edit, and send with one click. Low-friction capture for v1 beta.
- Feedback channel: `macmakesproducts+research-director@gmail.com`.

### Known limits (v1 beta, by design)
- Single format (Claude Skill). GPT and GitHub-runnable versions are roadmap.
- Single output shape. No domain-specific modes.
- No persistence across sessions.
- Source quality inherited from whatever research tools the user's Claude session has available.
