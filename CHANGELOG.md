# Changelog

## v1.2.0 — 2026-04-30

Redesign based on v1 eval findings (see [research-director-evals](https://github.com/macmakesproducts/research-director-evals)).

### Changed
- **Two registers, never mixed.** Process language (what Claude says between phases — "I'm going to ask two questions," "Re-running deeper research") now lives explicitly in chat. Product language (what appears in the brief) is the deliverable. The brief never narrates its own production. Eliminates the v1.1 Phase 3 narration leak — *"Critic pass / Executing now"* tokens were bleeding into output and a v1 eval grader specifically flagged this as voice/format degradation.
- **Critic phase replaced by adversarial gaps-section review.** v1.1's standalone Critic phase reviewed the entire brief; v1.2 narrows the adversarial pass to authoring the gaps section specifically. v1 eval data showed `Honesty about gaps` was the only dimension where graders agreed (Krippendorff's α=0.84) and v1.1 directionally outperformed v1.0 (d=+0.75). Concentrating the adversarial review where measurement is reliable, removing it from dimensions where it produced noise without measurable benefit.
- **Synthesis-pattern naming added to Phase 3.** Before any brief is shaped, the skill names the cross-cutting pattern in 1-2 sentences — what the consolidated research is saying that no single source says. The pattern statement becomes the spine of the executive summary. Addresses the v1 eval directional signal that v1.1 may have lost synthesis depth versus v1.0 (Δ=-0.09, d=-1.07; α=0.41 so directional only).
- **Flow simplified from five phases to four.** Intake → Plan → Execute → Deliver. Adversarial gaps review is part of Phase 4 Deliver, not a separate phase.
- **Behavioral rules section streamlined** to reflect the new architecture.

### Removed
- **`/critic [1-5]` slash command and the dial system.** v1 eval did not test the dial; the architecture was theoretical scaffolding. Removed pending v2 eval evidence. If v2 demonstrates measurable differences across dial levels, the dial returns in v1.3 with empirical grounding.
- **Sub-agent execution language.** Inline-register-switch is the only pattern that works in current claude.ai web sessions; the sub-agent option added complexity without practical benefit.

### Fixed
- The exec summary now has an explicit construction recipe (build from the synthesis-pattern statement, anchor in strongest evidence, name contradictions). Prior versions left exec-summary construction implicit.
- Gaps section examples include explicit "flag-to-gap translation" showing how internal Critic-style flags become user-facing gap entries without process-language leak.

### v1 eval evidence supporting these changes
- **Inter-rater reliability** (Krippendorff's α): Honesty about gaps 0.84 (good); Synthesis depth 0.41, Source quality 0.31, Voice 0.03, Drift -0.12, Decision usefulness 0.01 (all weak/random). On dimensions where measurement is reliable, the Critic was directionally helping. On dimensions where graders disagreed, we couldn't tell. Strategy: concentrate the adversarial discipline where it provably matters.
- **Sonnet 4.6 grader feedback (Q5/v1.1.0/run3):** *"the inline 'Critic pass' and 'Executing now' process narration at the top, which is not content the reader needs"* — direct evidence of the process/product boundary problem.
- **Operational metrics:** v1.1.0 ran at parity cost with v1.0 (133s vs 134s avg), confirming the Critic phase wasn't a cost driver. The architectural decisions in v1.2 don't compromise the operational profile.

## v1.1.0 — 2026-04-29

### Added
- **Phase 4 — Critic Review.** New independent review pass between Execute and Deliver. Runs against a six-flag taxonomy (blind spot / hallucination / thin research / missed consultation / quality slippage / drift), each flag carrying severity (blocking / significant / minor) and a required actionable recommendation. Inspired by the Critic agent canon developed in Anthropic-adjacent multi-agent systems where producing agents have structural production bias and a dedicated reviewer role catches what the producer is too close to see.
- **Threshold-driven auto-retry.** When the Critic produces 3+ significant flags or 1+ blocking flag, the skill auto-triggers one round of deeper research targeted at the specific dimensions the flags named, then re-consolidates and re-Critics. Capped at one retry to prevent indefinite loops. If the second pass still trips threshold, the brief ships with an honest "did not fully clear Critic review" note in the executive summary.
- **`/critic [1-5]` slash command.** Five-level dial for the user to adjust the Critic's rigor. 1 = silent (skip review), 2 = light (blocking flags only), 3 = standard, 4 = default (full taxonomy, normal threshold), 5 = adversarial (1 significant flag triggers retry). Persists for the session unless changed. `/critic` with no number triggers an immediate review of work-so-far at current dial.
- **Critic flags construct the Gaps section.** Surviving flags at severity ≥ significant become the Open Questions / Gaps section of the brief — making the honesty layer structurally adversarial rather than producer-self-reported.
- **Sub-agent execution when available.** When the environment supports Task-tool or sub-agent invocation, the Critic runs as a separate Claude call with the consolidated research handed in as context. Falls back to inline-register-switch in claude.ai web sessions where sub-agents aren't available.

### Changed
- Flow expanded from four phases to five (Intake / Plan / Execute / Critic Review / Deliver).
- Behavioral rules updated to reference the Critic dial as a peer to `run as-is` for skip behavior.

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
