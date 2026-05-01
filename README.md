```
 ██▀███  ▓█████   ██████ ▓█████ ▄▄▄       ██▀███   ▄████▄   ██░ ██
▓██ ▒ ██▒▓█   ▀ ▒██    ▒ ▓█   ▀▒████▄    ▓██ ▒ ██▒▒██▀ ▀█  ▓██░ ██▒
▓██ ░▄█ ▒▒███   ░ ▓██▄   ▒███  ▒██  ▀█▄  ▓██ ░▄█ ▒▒▓█    ▄ ▒██▀▀██░
▒██▀▀█▄  ▒▓█  ▄   ▒   ██▒▒▓█  ▄░██▄▄▄▄██ ▒██▀▀█▄  ▒▓▓▄ ▄██▒░▓█ ░██
░██▓ ▒██▒░▒████▒▒██████▒▒░▒████▒▓█   ▓██▒░██▓ ▒██▒▒ ▓███▀ ░░▓█▒░██▓
░ ▒▓ ░▒▓░░░ ▒░ ░▒ ▒▓▒ ▒ ░░░ ▒░ ░▒▒   ▓▒█░░ ▒▓ ░▒▓░░ ░▒ ▒  ░ ▒ ░░▒░▒
  ░▒ ░ ▒░ ░ ░  ░░ ░▒  ░ ░ ░ ░  ░ ▒   ▒▒ ░  ░▒ ░ ▒░  ░  ▒    ▒ ░▒░ ░
  ░░   ░    ░   ░  ░  ░     ░    ░   ▒     ░░   ░ ░         ░  ░░ ░
   ░        ░  ░      ░     ░  ░     ░  ░   ░     ░ ░       ░  ░  ░
                                                  ░

▓█████▄  ██▓ ██▀███  ▓█████  ▄████▄  ▄▄▄█████▓ ▒█████   ██▀███
▒██▀ ██▌▓██▒▓██ ▒ ██▒▓█   ▀ ▒██▀ ▀█  ▓  ██▒ ▓▒▒██▒  ██▒▓██ ▒ ██▒
░██   █▌▒██▒▓██ ░▄█ ▒▒███   ▒▓█    ▄ ▒ ▓██░ ▒░▒██░  ██▒▓██ ░▄█ ▒
░▓█▄   ▌░██░▒██▀▀█▄  ▒▓█  ▄ ▒▓▓▄ ▄██▒░ ▓██▓ ░ ▒██   ██░▒██▀▀█▄
░▒████▓ ░██░░██▓ ▒██▒░▒████▒▒ ▓███▀ ░  ▒██▒ ░ ░ ████▓▒░░██▓ ▒██▒
 ▒▒▓  ▒ ░▓  ░ ▒▓ ░▒▓░░░ ▒░ ░░ ░▒ ▒  ░  ▒ ░░   ░ ▒░▒░▒░ ░ ▒▓ ░▒▓░
 ░ ▒  ▒  ▒ ░  ░▒ ░ ▒░ ░ ░  ░  ░  ▒       ░      ░ ▒ ▒░   ░▒ ░ ▒░
 ░ ░  ░  ▒ ░  ░░   ░    ░   ░          ░      ░ ░ ░ ▒    ░░   ░
   ░     ░     ░        ░  ░░ ░                   ░ ░     ░
 ░                          ░
```

`✦  one-man shop  ✦  est. 2026  ✦  v1.2.2 beta  ✦`

> Bring a question with real stakes. The shop shapes it, plans the research, reads the web, and brings you back a brief you can act on.

> **Two doors in.**  
> The **walk-in** is this skill — free, runs in claude.ai, decent across most jobs. Catches what the room can hold.  
> The **back room** is [`research-director`](https://pypi.org/project/research-director/) on PyPI — the same shop, structurally guaranteed output, JSON sidecar, scriptable. For when the brief has to come out the same way every time.  
>  
> Most folks start with the walk-in. Some never need anything else. Read on if you want to know what's behind the curtain.

---

## ═══╡ THE FLOW ╞═══

```
   ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
   │  INTAKE  │───▶│   PLAN   │───▶│ EXECUTE  │───▶│ DELIVER  │
   └──────────┘    └──────────┘    └──────────┘    └──────────┘
    0–3 adaptive    scope check     web search +    5-section brief
    questions       + approval      synthesis +     with adversarial
                                    pattern naming  gaps section
```

---

## ═══╡ TAKE IT HOME ╞═══

**Walk-in** — the free skill, runs in claude.ai:

1. Grab [`research-director.skill`](./research-director.skill) from this repo.
2. Claude.ai → Settings → Capabilities → Skills → Upload skill.
3. Select the file. Done. About 30 seconds.

**Back room** — the CLI, structurally guaranteed output:

```bash
pip install research-director
export ANTHROPIC_API_KEY='sk-ant-...'
rd "your question with real stakes"
```

Two files land in your current directory: `brief-YYYY-MM-DD-HHMM.md` and `brief-YYYY-MM-DD-HHMM.json`. The markdown is the brief; the JSON is the same data structured for pipelines, dashboards, post-processing. Source: [`research-director-cli`](https://github.com/macmakesproducts/research-director-cli) · PyPI: [`research-director`](https://pypi.org/project/research-director/).

**Claude Code** — slash command wrapping the CLI:

```bash
mkdir -p ~/.claude/skills/research-director
# Drop the SKILL.md from the CLI repo's claude_code_skill/ dir
# Then in any Claude Code session: /research-director "your question"
```

**By appointment** — clone this repo if you want to read the SKILL.md or fork the pattern:

```bash
git clone https://github.com/macmakesproducts/research-director.git
```

---

## ═══╡ A TYPICAL SESSION ╞═══

```
╭─ you ────────────────────────────────────────────────────────╮
│                                                              │
│  my team is evaluating whether to switch from Jira to        │
│  Linear. what do actual users say about the switch? what     │
│  does migration cost look like? edge cases 6 months in?      │
│                                                              │
╰──────────────────────────────────────────────────────────────╯

╭─ research director ──────────────────────────────────────────╮
│                                                              │
│  Two questions to shape the brief:                           │
│                                                              │
│   1. Team decision or just your call?                        │
│   2. Rough team size + engineering intensity?                │
│                                                              │
╰──────────────────────────────────────────────────────────────╯

╭─ you ────────────────────────────────────────────────────────╮
│                                                              │
│  team decision, 15 engineers, heavy sprint usage.            │
│                                                              │
╰──────────────────────────────────────────────────────────────╯

╭─ research director ──────────────────────────────────────────╮
│                                                              │
│  Plan:                                                       │
│   • migration cost data from teams of similar size           │
│   • user sentiment on sprint workflows specifically          │
│   • 6-month retention findings (who regretted it?)           │
│   • deal-breaker edge cases (integrations, reporting)        │
│                                                              │
│  Confirm or adjust — executing on your go.                   │
│                                                              │
╰──────────────────────────────────────────────────────────────╯
```

---

## ═══╡ THE BRIEF ╞═══

Every brief has these five sections, in this order:

| # | Section | What's in it |
|---|---------|--------------|
| 1 | **Executive summary** | 3–5 sentences of synthesis |
| 2 | **Key findings** | Each with source(s) and confidence rating |
| 3 | **Open questions / gaps** | What the research *didn't* answer |
| 4 | **Recommended next steps** | What to do about what was learned |
| 5 | **Sources** | Consulted list with recency + context |

The gaps section is load-bearing. Most AI research reads like it covered everything. This one names the edges — that's what makes the rest worth trusting.

---

## ═══╡ THE GAPS, AUTHORED HONESTLY ╞═══

Every brief's gaps section is written adversarially — explicitly, by switching register from producer to reviewer.

The reviewer's job is to read the brief as if someone else wrote it and find what's wrong: what's missing, what's hallucinated, what rests on a single source it shouldn't, what should have been the user's call instead of an internal assumption, where the research drifted off the question. Each flag fits one of six categories — blind spot, hallucination, thin research, missed consultation, quality slippage, drift — and each carries an actionable recommendation, not just an observation.

What the user sees in the brief: gap entries framed for action, with the category in parentheses. Not "the Critic flagged X" — that's process language. The gap entry is the user's; the category is the audit trail.

If the review surfaces something severe enough to make the brief unsafe — fabricated sources, drift severe enough that the actual question wasn't answered — the shop runs targeted research on the flagged dimensions and re-shapes. Capped at one re-run. If flags still remain after that, the brief ships with them surfaced prominently in the gaps section and a calibration note in the executive summary so the user weights the recommendations accordingly.

The gaps section is what makes the rest of the brief trustworthy. Coverage that claims completeness is coverage that hallucinated the edges.

---

## ═══╡ THE FLASH SHEET ╞═══

What this skill is good at. Pick your piece:

```
┌─ № 01 ─ DECISIONS WITH REAL DOWNSIDE ────────────────────────┐
│ tool switches · market entry · vendor eval · hiring calls    │
└──────────────────────────────────────────────────────────────┘

┌─ № 02 ─ PREP FOR A CONVERSATION WITH AN EXPERT ──────────────┐
│ doctor · lawyer · financial advisor · inspector              │
│ walk in with the right three questions.                      │
└──────────────────────────────────────────────────────────────┘

┌─ № 03 ─ LIFE ADMIN YOU'VE BEEN AVOIDING ─────────────────────┐
│ the HOA letter · the insurance clause · the school policy    │
│ you want to push back on but can't quite articulate why.     │
└──────────────────────────────────────────────────────────────┘
```

A Google search won't quite do it. Hiring an analyst is overkill. That's the middle this shop fills.

---

## ═══╡ HOUSE RULES ╞═══

**What it ain't:**

- Not your lawyer, doctor, or financial advisor. Researches recommendations, helps you prep — doesn't give professional advice.
- Not a source of primary research. Synthesizes public sources only. No original studies, interviews, or data collection.
- Not a content generator. The brief is a research artifact. It's not a draft of your blog post.
- Not autonomous. Plan approval is always required. Intake usually is.
- Not persistent. Every session stands alone.

**What separates it from generic deep-research:**

1. **Shapes the question before researching.** Quick intake (0–3 questions) so the output matches the decision.
2. **Scope discipline.** Oversized topics get decomposed at the plan stage rather than thinned, truncated, or fabricated to fit.
3. **A real second look in the gaps section.** Before the brief ships, the gaps section is authored by an explicit reviewer pass against six failure categories — not the producer self-reporting on its own work. The producer doesn't grade its own paper.
4. **Honest about its own limits.** Gaps and per-finding confidence ratings are non-optional.

**Honest note about the skill version:** Single-turn Claude skills can't *mechanically* enforce the output contract. The skill follows it most of the time, but the model occasionally renames sections ("Bottom line" instead of "Executive summary") or skips structural elements. If you need every brief to come out the same way every time — for parsing, archives, evals, or anything programmatic — that's what the [CLI](https://pypi.org/project/research-director/) is for. The CLI splits research from rendering: Claude produces the data, Python deterministically renders the brief. The contract becomes mechanical.

---

## ═══╡ A NOTE ON "BETA" ╞═══

```
╔══════════════════════════════════════════════════════════════╗
║                                                              ║
║   "early and incomplete by design"                           ║
║                                                              ║
║   beta here doesn't mean "we're polishing."                  ║
║   it means the pattern works well enough to ship,            ║
║   and the point is to find out where it breaks               ║
║   for other people before sanding it down further.           ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

Known limits in v1:
- **Structural inconsistency.** Single-turn skills can't mechanically guarantee the output contract — the model interprets prose-spec as a strong suggestion. Most briefs come out shaped right. Some don't. If "every time, the same way" matters, use the [CLI](https://pypi.org/project/research-director/).
- Source quality inherits from whatever research tools your Claude session has.
- One output shape. No domain-specific modes.
- No persistence across sessions.

---

## ═══╡ DROP A LINE ╞═══

```
╭──────────────────────────────────────────────────────────────╮
│                                                              │
│   macmakesproducts+research-director@gmail.com              │
│                                                              │
╰──────────────────────────────────────────────────────────────╯
```

**Fastest path:** after the skill delivers a brief, say `feedback` and it composes a pre-filled email block — question, summary, gaps already formatted. You add one line of reaction and send.

Real examples beat abstract feedback every time. One line, ten lines, whatever.

*Feedback is the spec.*

---

## ═══╡ PAPERWORK ╞═══

License: MIT — see [`LICENSE`](./LICENSE).  
Changelog: [`CHANGELOG.md`](./CHANGELOG.md).  
CLI version: [`research-director-cli`](https://github.com/macmakesproducts/research-director-cli) · [`pip install research-director`](https://pypi.org/project/research-director/).

---

```
                    ─── signed, the shop ───
```

— [Mac](https://github.com/macmakesproducts) · built in Salt Lake City. Companion post: *[link pending]*.
