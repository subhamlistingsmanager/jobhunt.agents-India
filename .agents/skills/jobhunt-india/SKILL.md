---
name: jobhunt-india
description: Job-search co-pilot for Indian candidates -- evaluate roles, build India-format resumes (fresher/experienced), scan Indian portals, track applications
arguments: mode
user_invocable: true
user-invocable: true
argument-hint: "[scan | deep | pdf | latex | cover | oferta | ofertas | apply | batch | tracker | pipeline | contacto | training | project | interview-prep | interview | patterns | followup | update]"
license: MIT
---

# jobhunt-india -- Router

## Mode Routing

Determine the mode from `$mode`:

| Input | Mode |
|-------|------|
| (empty / no args) | `discovery` -- Show command menu |
| JD text or URL (no sub-command) | **`auto-pipeline`** |
| `oferta` | `oferta` |
| `ofertas` | `ofertas` |
| `contacto` | `contacto` |
| `deep` | `deep` |
| `interview-prep` | `interview-prep` |
| `interview` | `interview` |
| `pdf` | `pdf` |
| `latex` | `latex` |
| `training` | `training` |
| `project` | `project` |
| `tracker` | `tracker` |
| `pipeline` | `pipeline` |
| `apply` | `apply` |
| `scan` | `scan` |
| `batch` | `batch` |
| `patterns` | `patterns` |
| `followup` | `followup` |
| `update` | `update` |
| `cover` | `cover` |

**Auto-pipeline detection:** If `$mode` is not a known sub-command AND contains JD text (keywords: "responsibilities", "requirements", "qualifications", "about the role", "we're looking for", company name + role) or a URL to a JD, execute `auto-pipeline`.

If `$mode` is not a sub-command AND doesn't look like a JD, show discovery.

---

## Discovery Mode (no arguments)

Show this menu:

```
jobhunt-india -- Command Center

Available commands:
  /jobhunt {JD}      → AUTO-PIPELINE: evaluate + report + PDF + tracker (paste text or URL)
  /jobhunt pipeline  → Process pending URLs from inbox (data/pipeline.md)
  /jobhunt oferta    → Evaluation only A-F (no auto PDF)
  /jobhunt ofertas   → Compare and rank multiple offers
  /jobhunt contacto  → LinkedIn power move: find contacts + draft message
  /jobhunt deep      → Deep research prompt about company
  /jobhunt interview-prep → Generate company-specific interview prep doc
  /jobhunt interview    → Interactive profile/CV onboarding interview
  /jobhunt pdf       → PDF only, ATS-optimized CV
  /jobhunt latex     → Export CV as LaTeX/Overleaf .tex
  /jobhunt cover     → Cover letter: standalone JD paste or /jobhunt cover {slug}
  /jobhunt training  → Evaluate course/cert against North Star
  /jobhunt project   → Evaluate portfolio project idea
  /jobhunt tracker   → Application status overview
  /jobhunt apply     → Live application assistant (reads form + generates answers)
  /jobhunt scan      → Scan portals and discover new offers
  /jobhunt batch     → Batch processing with parallel workers
  /jobhunt patterns  → Analyze rejection patterns and improve targeting
  /jobhunt followup  → Follow-up cadence tracker: flag overdue, generate drafts
  /jobhunt update    → Update jobhunt-india system files with diff preview + compat check

Inbox: add URLs to data/pipeline.md → /jobhunt pipeline
Or paste a JD directly to run the full pipeline.
```

---

## Context Loading by Mode

After determining the mode, load the necessary files before executing:

### Modes that require `_shared.md` + their mode file:
Read `modes/_shared.md` + `modes/{mode}.md`

Applies to: `auto-pipeline`, `oferta`, `ofertas`, `pdf`, `contacto`, `apply`, `pipeline`, `scan`, `batch`

### Standalone modes (only their mode file):
Read `modes/{mode}.md`

Applies to: `tracker`, `deep`, `interview-prep`, `interview`, `latex`, `training`, `project`, `patterns`, `followup`, `cover`

### Modes delegated to subagent:
For `scan`, `apply` (with Playwright), and `pipeline` (3+ URLs): launch as Agent with the content of `_shared.md` + `modes/{mode}.md` injected into the subagent prompt.

```
Agent(
  subagent_type="general-purpose",
  prompt="[content of modes/_shared.md]\n\n[content of modes/{mode}.md]\n\n[invocation-specific data]",
  description="jobhunt-india {mode}"
)
```

Execute the instructions from the loaded mode file.
