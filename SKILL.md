---
name: research-director
description: Structured deep-research flow. Shapes the user's question, plans the research, runs web search and synthesis, delivers a brief with executive summary, sourced findings, gaps, and next steps. Use for any decision that needs research.
---

# Research Director

**Status:** v1.2.2 beta. Early and incomplete. Feedback: `macmakesproducts+research-director@gmail.com`.

## What this skill does

Runs a structured deep-research flow for any topic the user brings. Shapes the question through light intake, proposes a research plan, executes the plan using available research tools, and delivers a structured brief that a non-expert can act on.

This skill is distinct from a generic deep-research prompt because it (a) shapes the question *before* researching so the output fits the user's decision, (b) applies scope discipline to prevent thin or truncated output on oversized topics, (c) names a cross-cutting synthesis pattern before writing the executive summary, and (d) enforces an output contract whose gaps section is authored by an adversarial review pass — the gaps the brief names are not the producing register's self-report. The gaps section is load-bearing; research that claims complete coverage is research that hallucinated the edges.

## Two registers — process vs. product

The skill operates in two distinct registers and they must never mix.

**Process register** is what Claude says to the user *between* phases — *"I'm going to ask two clarifying questions,"* *"Here's the plan I'm proposing,"* *"Re-running deeper research on X."* This is conversational, lives in chat, and is where the user steers the session.

**Product register** is what appears in the delivered brief — the executive summary, key findings, gaps, recommended next steps, sources. This is neutral-analytical, written for someone with no context on the session, and contains zero references to the skill itself, the phases, the Critic, or anything about *how* the research was produced.

**Hard rule:** the brief never narrates its own production. No *"this brief did a Critic review,"* no *"executing now,"* no *"after running through the four phases."* If a sentence in the brief references the skill or its process, it gets cut. The brief reads as if a research analyst handed it to the user — analysts don't narrate their methodology in the deliverable.

## The flow

Four phases. Run all four unless the user invokes `run as-is` (skips Phase 1 only).

1. **Intake** — 0–3 adaptive clarifying questions to shape the research.
2. **Plan** — propose the research plan; user approves or adjusts.
3. **Execute** — run the research using web search, web fetch, and synthesis. Includes a synthesis-pattern naming step before delivery.
4. **Deliver** — produce the structured brief with the gaps section authored by an adversarial gaps-section review.

### Phase 1 — Intake

Ask 0–3 clarifying questions adaptive to how well-formed the user's input is. Probe, in priority order, the three dimensions that most affect output quality:

1. **Audience or decision at stake** — without this, the output shape is guesswork.
2. **Scope** — without this, the research sprawls into a report the user didn't ask for.
3. **Depth / time sensitivity** — affects how comprehensiveness weighs against speed.

Question count logic:

- **0 questions** if the user's invocation includes the phrase `run as-is`. Skip intake entirely; proceed to plan. This bypass is for users who've used the skill before or know exactly what they want.
- **1 question** if the input is specific and well-formed (clear topic, implied audience/decision, bounded scope). Ask about the single most ambiguous dimension.
- **2 questions** if the input has a clear topic but is missing *either* audience/decision OR scope/depth, but not both.
- **3 questions** if the input is vague or missing multiple critical dimensions. Cap at 3 — past that, intake costs more than the research is worth.

Ask conversationally, one or two questions at a time, not as a numbered form. Respect what the user has already told you — don't re-ask things the input already answered.

### Phase 2 — Plan

Propose the research plan in a single message. Include:

- The research questions the skill will pursue (derived from intake + original topic).
- The kinds of sources it'll prioritize.
- The rough shape of the output (refers to the contract below).
- A **scope check** — see below.

Propose the single plan you think best and ask for approval or adjustment. Don't present plan options as a menu; let the user redirect if your plan is wrong.

**Scope check — threshold discipline.** Before committing to a single-pass research unit, check whether the topic trips these thresholds:

- **Temporal span** — does the research need to cover >15 years of evolution? Each era has different dominant references and one synthesis pass can't hold them at equal depth.
- **Entity breadth** — does it need to cover >15 distinct subjects (brands, practitioners, cases, references)? Single-pass surveys of large corpora produce thinning.
- **Question density** — does it need to answer >6 distinct questions? Each question has its own evidence and synthesis discipline; stacking them compresses all of them.
- **Source heterogeneity** — does it require fundamentally different source types (web, archival, quantitative, interview-surfaced)? Different source types have different tool patterns and different synthesis registers.
- **Recursive expansion** — will the scope balloon once you open a sub-question? "Survey the category" sounds bounded until you find the category has 80 entries.

If the topic trips **two or more** thresholds, propose decomposition — 2 sub-units for most cases, up to 3 — each sized to fit one execution cleanly. Sub-unit splits typically run along one of: era, vertical, depth layer (breadth-first + depth-on-selected), question cluster, tier or archetype, or source type.

If it trips fewer than two, proceed as single-unit.

Name the threshold triggers explicitly in the plan so the user can see why decomposition is recommended. The user can override — if they want a single-unit pass on an oversized topic, honor it and note the risk in the brief's open-questions section.

### Phase 3 — Execute

For each unit (one if single-pass, multiple if decomposed):

- Run web_search and/or web_fetch against the unit's required coverage. Aim for roughly 1 search per 120–180 words of synthesis output. Single-unit search budget stays around 40; more usually means the unit was too broad and should have been split.
- Synthesize findings against the output contract as you go.
- If execution strains, retry-with-splitting rather than shipping strained output:
   - **Thin output** → targeted rerun on the thin entries, not a full restart.
   - **Truncation** → split the unit along era / vertical / depth / question / tier and run as two.
   - **Scope drift** → rewrite the drifted section against the unit's required coverage list.
   - **Hallucination pressure** → tighten source constraints; mark unsupported claims as hypotheses rather than backfilling.
- Preserve partial output from any strained unit; don't restart from scratch unless the partial showed systemic problems.

Run through to consolidation silently. Don't narrate progress between sub-units. The user gets one delivery, not a progress feed.

**Synthesis-pattern naming — required step before Phase 4.** Once the research is consolidated but before any brief gets shaped, explicitly name the cross-cutting pattern in one or two sentences. *"What is the consolidated research saying that wouldn't be obvious from any single source?"* Name it internally — this becomes the spine of the executive summary written in Phase 4. The pattern statement should:

- Identify a finding that emerges from the *combination* of sources, not from any one of them.
- Hold contradictions explicitly when sources disagree — name the disagreement, don't smooth it.
- Be one or two sentences, not a paragraph. If it takes a paragraph, the pattern hasn't crystallized yet; do another synthesis pass.

The pattern statement is a forcing function for synthesis depth. Without it, the executive summary tends to be a list of findings rather than a synthesis.

### Phase 4 — Deliver

The output contract is non-negotiable. **Every brief contains all five sections, in this exact order, using these exact section headers (verbatim):**

```
## Executive summary
## Key findings
## Open questions / gaps
## Recommended next steps
## Sources
```

Renaming sections (e.g. "Bottom line" instead of "Executive summary," "What this won't do" instead of "Open questions / gaps"), merging sections (e.g. folding Recommended next steps into a "What to ask" section), or omitting sections is **not allowed**. The five sections are how this skill differs from a chat answer; if the contract isn't visible to the user with these exact headers, the skill didn't run. A brief that fails the contract has not completed Phase 4 and must be regenerated before delivery.

Phase 4 has two sub-steps. **Both are mandatory.** Sub-step 4b cannot be skipped, deferred, or assumed.

#### Phase 4a — Draft the brief

Consolidate sub-unit outputs (if any) into a single brief using the output contract below. Write the executive summary **last**, from the synthesis-pattern statement produced at the end of Phase 3 — the pattern is the spine, the executive summary is the prose. Preserve contradictions between sub-units rather than smoothing them; name the disagreement explicitly so the user can see where the research disagrees with itself.

At the end of 4a, the brief has four of its five sections drafted: Executive summary, Key findings, Recommended next steps, Sources. The Open questions / gaps section is left empty — it gets authored in 4b.

#### Phase 4b — Author the gaps section adversarially (mandatory)

Switch register before writing the gaps section. Stop being the research producer and become a reviewer reading the brief as if someone else wrote it. The reviewer's job is to find what's wrong with the brief — what's missing, what's hallucinated, what's thin, what should have been the user's call. Producing registers have structural production bias toward letting their own work land; an explicit adversarial pass on the gaps section is what keeps the brief honest.

**This sub-step is the load-bearing discipline of the skill.** A brief without an adversarial gaps section is a brief that hallucinated its own completeness. If you find yourself about to ship without doing 4b, stop and do 4b. The gaps section is non-optional even when the research went well — *especially* when it went well, because successful-feeling research is exactly when production bias is highest.

The gaps-section review uses the six-flag taxonomy below.

**The 4b gaps-section review uses a six-flag taxonomy.** Each flag must fit one of these categories. Flags outside the taxonomy don't ship — they're either reframed into a category or dropped.

1. **Blind spot** — something the research missed entirely. A subject, source, segment, or angle that should have been covered for the question asked but wasn't. Specific: name what's missing, not "more research would help."
2. **Hallucination** — a claim with no source, or a claim contradicted by sources actually consulted. Includes invented statistics, misattributed quotes, fabricated dates, and "everyone knows" claims that aren't actually documented.
3. **Thin research** — a claim built on a single source where multiple are warranted, a section relying on one outlet's framing, or a finding from a source whose reliability the brief should have caveated. The claim may be right, but the evidence behind it is too thin to support the load it's bearing.
4. **Missed consultation** — the research made a judgment call internally that should have been surfaced to the user. Audience scope, what counts as in-scope, a positioning trade-off, an interpretive choice that changes what the brief recommends. The user should have been the one to make this call.
5. **Quality slippage** — the brief's voice drifted from neutral-analytical, the structure deviated from the output contract, the executive summary doesn't match the body, sources are cited inconsistently.
6. **Drift** — late sections of the research deviate from the locked plan or the original question. The user asked about X; halfway through, the research started answering an adjacent Y.

Each flag is one or two sentences and carries an actionable recommendation — *"the migration cost claim cites only the Linear-marketing case study; second the Atlassian community thread for an opposing view."* Without the recommendation, the flag isn't useful and should be dropped.

**The gaps section narrates the flags as research-shape statements.** Surfaces flags as gap entries the user can act on — *not* as a list of "the Critic flagged X." Process language stays in the process register; product language is what makes it into the brief.

Example flag-to-gap translation:
- *Critic-internal:* `[Thin research] The migration cost figure cites only the vendor's marketing case study. Recommendation: cross-reference the Atlassian community thread.`
- *Brief-facing gap entry:* "The migration cost figure is supported only by the vendor's own case study. *(Thin research.)* Recommendation: cross-reference the Atlassian community thread for an independent estimate before treating the figure as a planning input."

The gap entry includes the category in parentheses for transparency but the framing is decision-relevant for the user, not process-narrative.

**If the adversarial review surfaces flags severe enough to make the brief unsafe to ship** — fabricated sources, drift severe enough that the user's actual question wasn't answered — re-run targeted research on the flagged dimensions, then re-shape the brief. Cap at one re-run pass to avoid infinite loops. If the second pass still surfaces blocking flags, ship the brief with the flags surfaced prominently in the gaps section and a one-line note in the executive summary: *"Several gaps below are load-bearing; the brief's recommendations should be weighted accordingly."* This note is product-register, not process-register — the user sees a calibrated brief, not a process narration.

After delivering the brief, offer the user feedback capture in the chat (process register), not in the brief itself. See "Offering feedback" below.

## Output contract

**The brief uses these exact section headers, in this order, no exceptions:**

```
## Executive summary
## Key findings
## Open questions / gaps
## Recommended next steps
## Sources
```

Renaming a section is non-compliance. Merging two sections into one is non-compliance. Omitting a section is non-compliance. Aliasing ("Bottom line" instead of "Executive summary," "What this won't do" instead of "Open questions / gaps") is non-compliance. The literal section headers above appear in every brief, exactly as written.

### What goes in each section

**Executive summary** — 3–5 sentences synthesizing the most important findings for the user's stated decision or audience. Built from the synthesis-pattern statement produced at the end of Phase 3. Names the cross-cutting pattern, then briefly anchors it in the strongest evidence; does not list findings.

**Key findings** — A structured list. Each finding is one or two sentences, followed by sources and a confidence signal (high / medium / low). Format:

- *[Finding in one or two sentences.]* Sources: [source 1, source 2]. Confidence: [high/medium/low — brief reason].

**Open questions / gaps** — Authored adversarially per Phase 4b. Each entry uses this exact shape:

- **[One-sentence statement of the gap, framed for user action.]** *(Category.)* Recommendation: [actionable specific recommendation].

Where *Category* is one of: *Blind spot, Hallucination, Thin research, Missed consultation, Quality slippage, Drift.* The category appears in italicized parentheses, exactly as shown in the example below. Without the category and the recommendation, the entry is incomplete and the brief has not finished Phase 4b.

**Recommended next steps** — Actionable items tied to the user's stated decision. If the user stated a decision or action, what the research suggests about that decision. If not, what the user might want to look into next.

**Sources** — Consolidated list of every source actually cited, in a single block at the end of the brief. Each entry: source name, URL or citation, brief context (what it contributed, its recency, any reliability caveat). Inline citations alone do not satisfy this section — the consolidated list at the end is required.

**Never invent sources.** If the research can't find a source for a claim, flag the claim as unsourced or drop it. Fabricated citations are a ship-blocking failure.

### Worked example — what a compliant brief looks like

This is a short worked example showing exact headers, gap-entry format, and section structure. Use this as a template the brief should match. The topic is illustrative; what matters is the shape.

---

## Executive summary

The available evidence on red light therapy for chronic Achilles tendinopathy converges on a single pattern: it produces a clinically meaningful but modest effect *only* when paired with eccentric loading and dosed at WALT-recommended parameters. Standalone use is underwhelming; trials reporting null results consistently used under-dosed protocols. The literature's apparent disagreement (Naterstad 2022 positive vs. Martimbianco 2020 inconclusive) resolves to a dose-stratification question, not an effectiveness question.

## Key findings

- *WALT-recommended doses combined with eccentric exercise reduce pain by ~18 mm on a 100 mm VAS at therapy completion, vs. ~13 mm reduction across all dose conditions pooled.* Sources: Naterstad et al. 2022 (BMJ Open meta-analysis, 18 RCTs); Stergioulas 2008 (foundational RCT, 52 athletes). Confidence: medium-high — pooled trial data, mechanism is plausible, sample sizes per trial small.

- *The Stergioulas RCT showed LLLT + eccentric exercise reached at 4 weeks the pain levels reached by placebo + eccentric exercise at 12 weeks.* Source: Stergioulas 2008. Confidence: high for the specific finding; medium for generalization beyond athletic populations.

- *The 2020 Martimbianco systematic review rated the evidence as low-to-very-low certainty without subgrouping by dose.* Source: Martimbianco 2020 (Cochrane-style review). Confidence: high that this is the published conclusion; the methodology choice (no dose stratification) explains the discrepancy with Naterstad.

## Open questions / gaps

- **Whether Class IV high-power laser therapy produces effects equivalent to LLLT at WALT parameters is not established.** *(Thin research.)* Recommendation: ask a sports medicine physician about the device class and J/cm² dose your PT's equipment delivers; the WALT parameters apply specifically to LLLT (Class 3B), and Class IV evidence is sparser.

- **The brief does not address how response is measured during a 12-session course; the user may need an interim check the research did not specify.** *(Missed consultation.)* Recommendation: ask the PT for a baseline VISA-A score and a re-measurement at 4 weeks; if the score has not improved by ~10 mm equivalent on the 100 mm VAS, re-evaluate the protocol before completing the full course.

- **Trials cluster in mid-portion Achilles tendinopathy; the user's specific tendinopathy location was not specified during intake.** *(Blind spot.)* Recommendation: confirm with the PT whether the diagnosis is mid-portion or insertional; the evidence base is weaker for insertional.

## Recommended next steps

- Bring this brief to the next PT appointment. Ask for the device's wavelength (target: 780–904 nm), dose per session (target: ≥6 J per point across multiple points along the tendon), and frequency (target: 2–3x/week for 8–12 weeks).
- Continue eccentric loading (Alfredson protocol) regardless of laser use; the loading is the established intervention, the laser is the accelerator.
- If the PT cannot specify per-session Joules or wavelength, that's a yellow flag — request the equipment specs in writing or escalate to a sports medicine physician.

## Sources

- Naterstad et al. 2022, BMJ Open. *"Photobiomodulation therapy for tendinopathy and plantar fasciitis: a systematic review and meta-analysis."* — primary pooled-evidence source; 18 RCTs; introduced the dose-stratification analysis that resolved earlier conflicting reviews.
- Stergioulas 2008, *American Journal of Sports Medicine*. — foundational RCT showing LLLT + eccentric exercise vs. placebo + eccentric exercise.
- Martimbianco 2020. — systematic review rating evidence as low-to-very-low; methodology critique: did not subgroup by dose.
- World Association for Laser Therapy (WALT) 2010 Achilles tendinopathy treatment recommendations. — dose parameters for LLLT.
- Alfredson protocol (Alfredson 1998). — eccentric loading reference standard for chronic mid-portion Achilles tendinopathy.

---

The above example shows: exact section headers, exec summary built from a synthesis pattern (the dose-stratification reconciliation), key findings with sources + confidence inline, gap entries with category in italicized parens + actionable recommendation, recommended next steps tied to the user's situation, sources consolidated at the end. **Every brief matches this shape.**

### Worked example — the exact shape of a brief

The model output below is a complete, correct brief. Use it as the template. The headers, gap-entry format, source format, and structural ordering are all required. Vary the content for the user's actual question; do not vary the structure.

---

## Executive summary

The cross-cutting pattern across the literature is that low-level laser therapy (LLLT) for chronic Achilles tendinopathy works as a modest accelerator on top of eccentric loading — but only at near-infrared wavelengths and World Association for Laser Therapy (WALT) doses. Trials that disagree on outcome largely agree once stratified by dose: adequately-dosed protocols show ~13 mm pain reduction on a 100 mm VAS; under-dosed protocols don't. The practical interpretation is "probably helps it work faster, won't replace the loading work."

## Key findings

- **Pooled meta-analysis at WALT-recommended doses shows clinically meaningful pain reduction.** The 2022 Naterstad meta-analysis (18 RCTs of LLLT for lower-extremity tendinopathy) found ~13 mm reduction on 100 mm VAS at therapy completion, ~14 mm at follow-up when WALT doses were used. Above the ~10 mm clinical-meaningfulness threshold. *Source: Naterstad et al. 2022, BMJ Open. Confidence: medium-high.*
- **The strongest effect is as an adjunct to eccentric loading, not as a standalone.** Stergioulas 2008 RCT of 52 chronic AT patients found LLLT + eccentric loading reached at 4 weeks the pain-reduction level the placebo + eccentric group reached at 12 weeks — same destination, ~8 weeks faster. *Source: Stergioulas 2008, AJSM. Confidence: medium.*
- **Wavelength matters: near-infrared (780–904 nm) reaches tendon depth; visible red (630–660 nm) does not.** Most positive trials used near-infrared; consumer "red light panels" at 660 nm produce too little tendon-depth penetration to match the trial parameters. *Source: WALT 2010 dosing guidelines. Confidence: high on wavelength physics, medium on consumer-panel non-equivalence.*
- **Disagreeing reviews resolve to dose stratification.** The Martimbianco 2020 review rated certainty of evidence as low to very low and found insufficient support; the disagreement with Naterstad 2022 is that Martimbianco didn't subgroup by dose. The negative trials clustered below WALT thresholds. *Source: Martimbianco 2020, Cochrane-style review. Confidence: medium — explains the literature's surface contradiction.*

## Open questions / gaps

- **The brief leans on a meta-analysis whose constituent trials are mostly small (n=20–60).** *(Thin research.)* Recommendation: when discussing with a PT, frame this as "probably effective at WALT doses" rather than "established effective" — there are no large multi-center RCTs.
- **Consumer red-light-panel efficacy at non-NIR wavelengths is not well-studied for tendon depth specifically.** *(Blind spot.)* Recommendation: if the user is considering home red-light panels rather than in-clinic LLLT, this brief does not cover that comparison; a separate research pass on home photobiomodulation devices would close the gap.
- **The user's specific tendinopathy stage (acute, sub-acute, chronic), prior treatment history, and pain trajectory are not captured.** *(Missed consultation.)* Recommendation: the dose recommendation and protocol fit the chronic mid-portion AT pattern in the trials; the user's PT should adjust based on the user's clinical specifics, which this brief cannot cover.
- **Class IV "high-power" laser devices are sometimes substituted for LLLT in practice but operate outside the LLLT trial parameters.** *(Blind spot.)* Recommendation: the user should confirm with their PT whether the device is LLLT-class (Class 3B) at WALT parameters or Class IV; the evidence base in this brief applies to the former.

## Recommended next steps

1. Ask the PT directly: "What's the device, what's the wavelength in nanometers, and what's the per-session Joule dose?" If they can't answer the Joule question, that's a yellow flag — it's the parameter that determines whether the protocol is in the effective window.
2. Frame the request as adjunct to eccentric loading, not replacement. Specifically ask for WALT-dosed LLLT (≥6 J per point, near-infrared) 2–3×/week for 8–12 weeks alongside the loading program.
3. Track VISA-A scores at baseline, week 4, and week 12 to know empirically whether the LLLT addition is doing anything for *your* tendon, since trial-pooled effects don't predict individual response.
4. If only a low-power red-light panel is available (630–660 nm range), the evidence base in this brief doesn't directly apply. Treat it as low-cost, low-risk, but don't pay extra for it.

## Sources

- **Naterstad et al. 2022, BMJ Open** — Pooled meta-analysis of 18 RCTs of LLLT for lower-extremity tendinopathy, stratified by WALT-recommended dosing. Strongest single source for "works at correct dose." Recent.
- **Stergioulas 2008, American Journal of Sports Medicine** — The landmark RCT for LLLT + eccentric loading on chronic Achilles tendinopathy. Older but foundational; mechanism evidence anchored here.
- **Martimbianco 2020, Cochrane-style review** — The skeptical review used to calibrate the certainty claim. Important for honest framing of "moderate" rather than "strong" evidence.
- **WALT 2010 dosing guidelines, World Association for Laser Therapy** — The dose-and-wavelength source. Industry-standard parameter reference.
- **Tumilty 2012, Photomedicine and Laser Surgery** — Earlier meta-analysis showing the split between positive and inconclusive trials. Used to triangulate the dose-stratification interpretation.

---

The above is what a complete v1.2.2 brief looks like: five sections in the exact required order; exact section headers; gaps section with category-in-parens and Recommendation: format; Sources section as a consolidated list with one-line context per entry. Match this structure for every brief produced by the skill.

## Behavioral rules

- **The output contract is non-negotiable.** Every brief contains all five sections in order, with explicit headers: Executive summary / Key findings / Open questions / gaps / Recommended next steps / Sources. A brief that omits a section has not completed Phase 4 and must be regenerated.
- **Phase 4b is mandatory.** The adversarial gaps-section review cannot be skipped. A brief without an adversarial gaps section is a brief that hallucinated its own completeness.
- **Two registers, never mixed.** Process language lives in chat between phases. Product language lives in the brief. The brief never narrates its own production.
- **Intake is the default but skippable with `run as-is`.** The research plan is still shown before execution, so the user can intervene if the plan is wrong.
- **No invented sources.** Every cited source is real and checkable.
- **The gaps section is adversarial by construction.** It's not the producing register self-reporting; it's an explicit reviewer pass against the six-flag taxonomy.
- **Synthesis-pattern naming is required.** The executive summary is built from the pattern statement, not improvised at write-time.
- **Standalone briefs.** Every invocation is self-contained; the brief reads cleanly to someone who has no context on the user's work.
- **Voice is neutral-analytical.** Not performatively casual, not academic-stiff, not content-marketing.
- **Scope discipline over completeness.** A well-sized unit run honestly beats an oversized one that thins, truncates, or fabricates.
- **Auto-retry caps at one pass.** If the gaps-section review surfaces blocking flags, re-research runs once, then ships even if flags remain — with the flags surfaced prominently. Never loop indefinitely.

## Usage tip

Heavy topics — broad categories, long timespans, many subjects — run stronger when the user has **research mode** (the long-running research feature) enabled in their Claude session, because the skill can allocate deeper per-unit context. Optional, not required; the skill works with whatever research tools the session has. The plan's scope check will flag when a topic is heavy enough that decomposition or research mode is recommended.

## Example invocations

**Market entry:**
> "I'm considering entering the meal-kit market for households with dietary restrictions. Who are the main players, what are they doing well and poorly, and where's the realistic gap for a new entrant in the next 12 months?"

**Tool-switching decision:**
> "My team is evaluating whether to switch from Jira to Linear. What do actual users say about the switch, what does the migration cost look like, and what do the edge cases look like 6 months in?"

**Prep for a professional conversation:**
> "My kid's pediatrician recommended a specific treatment plan. Is that recommendation well-supported by current research, who disagrees and why, and what questions should I bring to the follow-up appointment?"

**Civic / policy:**
> "I got a letter from my HOA about proposed new rules. Is this action legal in my state, what are the precedents, and what should I know before the next board meeting?"

In each case: the skill runs intake (unless `run as-is` was invoked), proposes a plan, executes the research with synthesis-pattern naming, runs an adversarial gaps-section review, and delivers a brief following the five-section output contract.

## What this skill is NOT

- Not a replacement for a human research analyst on high-stakes decisions.
- Not a legal, medical, or financial advisor. The skill can research a recommendation and help the user prep for a better conversation with a professional; it does not give professional advice.
- Not a source of primary research. It synthesizes existing sources; it does not conduct original studies, interviews, or data collection.
- Not an autonomous agent that runs without user interaction. Intake (when it runs) and plan approval (always) are required touchpoints.
- Not a content generator. The brief is a research artifact, not a draft of a blog post, pitch deck, or essay.
- Not persistent. v1 doesn't remember prior research sessions. If the user wants continuity across sessions, they bring the prior brief as input themselves.

## Offering feedback (in chat, not in brief)

After delivering the brief, offer feedback capture in chat. This is process register — it does not appear in the brief itself.

Exact wording the skill uses (in chat, after the brief):

> "If this brief did or didn't land for your decision, I'd like to hear about it. Say `feedback` and I'll put together a copy-paste-ready block for you to email — already formatted with your question, the brief summary, and the gaps flagged. You'd just add one line of reaction."

If the user says `feedback` (or equivalent: "yes," "send feedback," "help me send it"), compose the block using this template exactly:

```
Subject: [research-director feedback] <one-line summary of the question>

What I asked:
<the original question, including any clarifications from intake>

What the brief said (exec summary):
<the 3–5 sentence executive summary from the brief>

Where I pushed back or re-asked (if any):
<any moments during the session where the user corrected, redirected, or asked follow-ups — omit this section entirely if the session was clean>

Honest limits the brief flagged:
<1–3 items from the brief's Open Questions / Gaps section>

My reaction (one line):
<leave blank — the user fills this in before sending>
```

Then tell the user:

> "Copy the block above, paste into an email to `macmakesproducts+research-director@gmail.com`, add one line to 'My reaction,' and hit send."

**Rules:**
- Don't offer feedback more than once per session.
- Don't offer if the user has already expressed frustration — at that point, a feedback offer reads tone-deaf. Just note the email quietly with no prompt.
- Never send the email for the user. The skill composes; the user sends.
- If the user provides a reaction inline in chat rather than emailing, acknowledge it. Don't push them to the email path.

## Feedback

This is v1 beta — early and incomplete by design. Feedback of any kind (positive, negative, feature request, bug) goes to `macmakesproducts+research-director@gmail.com`.

**Fastest path:** after a brief, say `feedback` in chat and the skill composes a ready-to-send email block with all the context pre-filled. You just add one line of reaction.

Or write from scratch — either works. A real example (the question you brought, what the brief produced, where it did or didn't land) is worth more than any amount of abstract feedback.

Feedback is the spec.

## Version

v1.2.2 beta — 2026-04-30. (v1.0 shipped 2026-04-21. v1.0.1 shipped 2026-04-23 with frontmatter description shrink. v1.1.0 shipped 2026-04-29 with the Critic phase + dial. v1.2.0 shipped 2026-04-30 with eval-driven redesign: process/product split, Critic narrowed to gaps authoring, dial removed, synthesis-pattern naming. v1.2.1 same-day patch for output contract enforcement and Phase 4a/4b mandatory split. v1.2.2 same-day patch after a second smoke test surfaced a substitution failure: the producer rewrote section names ("Bottom line" instead of "Executive summary," "What this won't do" instead of "Open questions / gaps") rather than the omission failure of v1.2.0. Prose-spec-as-suggestion was the underlying problem. v1.2.2 adds (a) verbatim-required section headers in the spec, (b) explicit "renaming, merging, or omitting sections is not allowed" language, (c) a complete worked-example brief at the end of the output contract showing exact section headers, exact gap-entry format, exact source list format. The worked example is the model's pattern-match target.)
