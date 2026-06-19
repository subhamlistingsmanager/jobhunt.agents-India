# How to Use Job Hunts Agent India

A complete, beginner-friendly guide — from zero to a tracked, tailored job application. No prior experience with AI tools needed.

**Contents**
1. [What this is](#1-what-this-is)
2. [Prerequisites](#2-prerequisites)
3. [Install (5 minutes)](#3-install-5-minutes)
4. [First run — setup by chatting](#4-first-run--setup-by-chatting)
5. [The files that matter](#5-the-files-that-matter)
6. [The commands](#6-the-commands)
7. [A day in the life (end-to-end walkthrough)](#7-a-day-in-the-life-end-to-end-walkthrough)
8. [Understanding the A–G evaluation](#8-understanding-the-ag-evaluation)
9. [Building your résumé (fresher vs experienced)](#9-building-your-résumé-fresher-vs-experienced)
10. [Indian job sources cheat sheet](#10-indian-job-sources-cheat-sheet)
11. [Customising the system (just ask)](#11-customising-the-system-just-ask)
12. [Your data & privacy](#12-your-data--privacy)
13. [Troubleshooting & FAQ](#13-troubleshooting--faq)

---

## 1. What this is

Job Hunts Agent India is a job-search co-pilot you run **inside an AI coding assistant** (Claude Code, Cursor, GitHub Copilot, Gemini CLI, etc.). You point it at a job; it:

- reads the job description against your CV and scores the fit (A–G report),
- builds an **India-format résumé** tailored to that role (fresher or experienced),
- drafts a cover letter and LinkedIn referral message,
- and logs everything in a tracker so you always know where you stand.

It runs on your own machine. **It never submits an application for you** — you always click Apply.

It works for **freshers/students** (campus placements, first job) and **experienced professionals** (switching companies).

---

## 2. Prerequisites

You need three things:

| Need | What / where | Notes |
|---|---|---|
| **Node.js 18+** | [nodejs.org](https://nodejs.org) | Runs the scripts. Check with `node --version`. |
| **An AI coding CLI** | e.g. [Claude Code](https://claude.com/claude-code) | This is the "brain" that reads the playbooks. Cursor, Copilot, Gemini CLI, OpenCode also work. |
| **Git** | [git-scm.com](https://git-scm.com) | To download the project. |

Optional: **Playwright Chromium** (`npx playwright install chromium`) — only needed to turn your résumé into a PDF, and to open live job URLs. You can skip it at first.

---

## 3. Install (5 minutes)

```bash
# 1. Download the project
git clone https://github.com/subhamlistingsmanager/jobhunt.agents-India.git
cd jobhunt.agents-India

# 2. Install dependencies
npm install

# 3. (Optional, for PDF résumés) install the browser
npx playwright install chromium

# 4. Check everything is ready
npm run doctor

# 5. Open your AI CLI in this folder
claude        # or: cursor / copilot / gemini / opencode
```

`npm run doctor` tells you what's still missing (your CV, profile, etc.). That's expected on a fresh install — the next step fills it in.

---

## 4. First run — setup by chatting

You don't edit config files by hand. Open your AI CLI in the folder and just say **hello** or **"help me set up"**. The agent walks you through it:

1. **Fresher or experienced?** This decides your résumé format and how roles are scored.
2. **Your CV.** Paste your existing résumé, paste your LinkedIn URL, or just describe yourself — the agent writes `cv.md` for you.
3. **Your profile.** Name, phone (+91), city, target roles, preferred cities, **expected CTC** (in ₹ LPA), and **notice period**. Saved to `config/profile.yml`.
4. **Portals.** The agent sets up `portals.yml` with Indian startups + ready-made searches for Naukri, LinkedIn, Instahyre, and more.

That's it. From here you can paste a job and go.

> Tip: the more context you give it ("my strongest project was X", "I want to avoid pure-services companies", "I can join in 30 days with a buyout"), the sharper its evaluations get. Think of it as onboarding a recruiter who works only for you.

### Manual setup (optional)

If you'd rather set it up yourself:

```bash
cp config/profile.example.yml config/profile.yml   # then edit
cp templates/portals.example.yml portals.yml        # then edit
# create cv.md in the project root with your CV in markdown
```

See `config/profile.example.yml` — it's fully commented for India (CTC, notice period, cities, fresher/experienced).

---

## 5. The files that matter

| File | What it is | Who edits it |
|---|---|---|
| `cv.md` | Your CV in plain markdown — the single source of truth | You (or the agent, with your OK) |
| `config/profile.yml` | Your details: contact, target roles, CTC, notice period, cities | You / the agent |
| `portals.yml` | Which companies and job boards to scan | You / the agent |
| `data/applications.md` | Your application tracker (auto-maintained) | The agent |
| `data/pipeline.md` | Inbox of job URLs to evaluate later | You (paste URLs here) |
| `reports/` | One detailed evaluation report per job | The agent |
| `output/` | Generated résumé PDFs | The agent |

`cv.md`, `config/profile.yml`, `portals.yml`, and everything under `data/` and `reports/` are **yours** — they're git-ignored and never overwritten by updates.

---

## 6. The commands

Type these in your AI CLI. (On Claude Code they're `/jobhunt …`; other CLIs use the same words.)

| Command | What it does |
|---|---|
| `/jobhunt` | Show the command menu |
| `/jobhunt {paste JD text or a URL}` | **Evaluate a job** end-to-end: A–G report + tailored résumé + tracker entry |
| `/jobhunt scan` | Scan your tracked companies + job boards for new matching roles |
| `/jobhunt pipeline` | Evaluate every URL you saved in `data/pipeline.md` |
| `/jobhunt pdf` | Build a tailored India-format résumé PDF |
| `/jobhunt cover` | Write a cover letter for a role |
| `/jobhunt interview-prep` | Generate an interview prep guide for a specific company |
| `/jobhunt contacto` | Draft a LinkedIn outreach / referral message |
| `/jobhunt tracker` | Show all your applications and their status |
| `/jobhunt ofertas` | Compare and rank multiple offers |
| `/jobhunt deep` | Deep-research a company |
| `/jobhunt followup` | See which applications are due a follow-up |
| `/jobhunt patterns` | Analyse your rejections to sharpen targeting |

Most days you'll use three: paste a job, build a résumé, check the tracker.

---

## 7. A day in the life (end-to-end walkthrough)

Here's a realistic flow for an experienced candidate.

**Step 1 — Find a role.** You spot a "Backend Engineer" opening on Naukri or LinkedIn.

**Step 2 — Evaluate it.** Copy the job URL (or the full JD text) and paste it:
```
/jobhunt https://www.naukri.com/job-listings-backend-engineer-...
```
The agent reads the JD against your `cv.md` and returns an **A–G report** (see §8) with a fit score out of 5. If the score is below ~3.5, it will tell you it's probably not worth applying — and why. Trust that; your time is the scarce resource.

**Step 3 — Build the résumé.** If it's a good match:
```
/jobhunt pdf
```
It tailors your résumé to the JD's vocabulary (only rewording your *real* experience — never inventing), renders it to an A4 PDF in `output/`, and keeps it ATS-clean. (It picks the fresher or experienced template automatically from your profile.)

**Step 4 — Cover letter / referral (optional).**
```
/jobhunt cover
/jobhunt contacto      # finds an angle for a LinkedIn referral — referrals are huge in India
```

**Step 5 — Apply yourself.** You review everything and submit on the company's site. The agent has already logged the role in `data/applications.md` as `Evaluated`; tell it once you've applied and it updates the status to `Applied`.

**Step 6 — Stay on top of it.**
```
/jobhunt tracker       # where does everything stand?
/jobhunt followup      # who's due a nudge?
```

**Batch mode.** If you collected ten URLs through the week, paste them into `data/pipeline.md` (one per line) and run `/jobhunt pipeline` to evaluate them all in one go.

---

## 8. Understanding the A–G evaluation

Every job gets a structured report so you're never guessing. The blocks:

| Block | Question it answers |
|---|---|
| **A — Role summary** | What is this role really, in one glance? |
| **B — Match with your CV** | Which requirements you hit (with exact CV lines), and where the gaps are |
| **C — Level & strategy** | Are you a fit for the level? Plus **notice-period & counter-offer strategy** (India) |
| **D — Comp & demand** | Market CTC for this role in **₹ LPA** (fixed vs variable vs ESOP), from AmbitionBox, Glassdoor India, Levels.fyi, 6figr |
| **E — Customisation plan** | The top changes to make to your CV and LinkedIn for this role |
| **F — Interview plan** | 6–10 STAR + Reflection stories mapped to the JD, building a reusable story bank |
| **G — Posting legitimacy** | Signals that flag ghost jobs / scams so you don't waste effort |

The **fit score (1–5)** and what it means:

| Score | Meaning |
|---|---|
| 4.5+ | Strong match — apply now |
| 4.0–4.4 | Good — worth applying |
| 3.5–3.9 | Decent — only with a specific reason |
| Below 3.5 | The agent recommends against applying |

Full reports are saved in `reports/` so you can revisit them before an interview.

---

## 9. Building your résumé (fresher vs experienced)

The system ships **two India résumé formats**, both ATS-clean and A4:

- **Fresher** (`cv-fresher-india.html`) — academics-first. Order: Career Objective → Education (10th %, 12th %, Degree + CGPA) → Technical Skills → Projects → Internships → Certifications → Positions of Responsibility → Coding Profiles (LeetCode/GitHub/HackerRank).
- **Experienced** (`cv-experienced-india.html`) — impact-first. Order: Professional Summary → Core Skills → Work Experience → Key Achievements → Education → Certifications → optional footer (Notice Period, Current/Expected CTC).

The agent picks the right one from your profile (`candidate_type`, or `cv.template` if you set it). Both share one clean design system — a single navy accent, no photo, no clutter. The design and the reasoning behind it are documented in [RESUME-DESIGN.md](RESUME-DESIGN.md).

**To generate one:**
```
/jobhunt pdf                 # tailored to the last job you evaluated
/jobhunt pdf {company-slug}  # for a specific saved evaluation
```
The PDF lands in `output/`. Example starting CVs: [`examples/cv-fresher-india-example.md`](../examples/cv-fresher-india-example.md) and [`examples/cv-experienced-india-example.md`](../examples/cv-experienced-india-example.md).

**Want a different look?** Just ask: *"use a maroon accent on my résumé"*, *"add a Publications section"*, *"make the summary two lines shorter"*. The agent edits the template directly.

---

## 10. Indian job sources cheat sheet

| Source | Best for | How to use it here |
|---|---|---|
| **`/jobhunt scan`** (Greenhouse/Lever/Ashby startups) | Indian product startups (Razorpay, Zerodha, Groww, Postman…) | Automatic, free — runs from `portals.yml` |
| **Naukri, LinkedIn India, Indeed** | The widest set of roles | Paste a role URL → `/jobhunt pipeline`, or use the `search_queries` in `portals.yml` |
| **Instahyre, Cutshort, Hirist** | Curated tech roles | Paste the URL → `/jobhunt pipeline` |
| **Wellfound, foundit** | Startups / general | Same — paste URL or use search queries |
| **IIMJobs** | MBA / finance / product / leadership | Paste URL → pipeline |
| **Internshala, Unstop** | Internships, fresher & campus | Paste URL → pipeline |
| **TCS / Infosys / Wipro / Accenture** | Large IT services (own portals) | Apply on their site / campus drives; paste a role URL to evaluate |

Full guide, plus an India comp & process glossary (CTC, in-hand, notice buyout, gratuity, EPF, BGV, bond…), is in [INDIAN-JOB-SOURCES.md](INDIAN-JOB-SOURCES.md).

---

## 11. Customising the system (just ask)

This is the point of the tool — it edits itself when you ask. Examples that all work in plain English:

- *"Change my target roles to Data Analyst and BI Engineer."*
- *"Target only Bengaluru, Hyderabad, and remote."*
- *"I'm a fresher — switch me to the fresher résumé format."*
- *"Add Zerodha, Groww, and Atlan to my tracked companies."*
- *"My notice period is now 30 days with a buyout."*
- *"Score remote roles higher — I strongly prefer remote."*
- *"Add a maroon accent and a Publications section to my résumé."*
- *"Scan for new roles every 3 days."*

Personal choices are saved to **your** files (`config/profile.yml`, `modes/_profile.md`) so a future update never wipes them.

---

## 12. Your data & privacy

Everything runs locally. There are two layers (see [`DATA_CONTRACT.md`](../DATA_CONTRACT.md)):

- **Your layer** — `cv.md`, `config/profile.yml`, `portals.yml`, `data/*`, `reports/*`. Git-ignored, never auto-overwritten.
- **System layer** — the playbooks, scripts, and templates. Safe to update/improve.

Your CV and personal details never leave your machine except when *you* (via the AI CLI) ask it to research comp or open a job page. Nothing auto-publishes; nothing auto-applies.

---

## 13. Troubleshooting & FAQ

**`npm run doctor` says files are missing.** Normal on a fresh install — open your AI CLI and let onboarding create them (§4).

**PDF generation fails / "playwright not found".** Run `npx playwright install chromium`. The résumé HTML still renders without it; only the PDF step needs the browser.

**`/jobhunt scan` returns nothing.** Most Indian boards (Naukri, LinkedIn) have no public API and can't be scanned token-free — that's expected. Use them by pasting a role URL into `data/pipeline.md` and running `/jobhunt pipeline`. Scanning works for startups on Greenhouse/Lever/Ashby. See [INDIAN-JOB-SOURCES.md](INDIAN-JOB-SOURCES.md).

**A company in `portals.yml` isn't found.** Career URLs and ATS slugs change. Ask the agent to verify and fix the entry, or just paste a specific role URL and evaluate it directly.

**The first evaluations feel generic.** It doesn't know you yet. Feed it context — your best project, your constraints, what you want to avoid. It improves with every interaction.

**Can I use it without Claude Code?** Yes — Cursor, GitHub Copilot, Gemini CLI, OpenCode, and other agent-skill CLIs all work. The playbooks live in `AGENTS.md` and `modes/`.

**Is the résumé really ATS-safe?** Yes — single column, system fonts, real text (no images/icons for content), A4, and the PDF step normalises problem characters. See [RESUME-DESIGN.md](RESUME-DESIGN.md).

**Will it apply to jobs for me?** No. By design it evaluates, drafts, and builds — you always review and click Apply yourself.

---

*Questions or ideas? Open an issue at the [repo](https://github.com/subhamlistingsmanager/jobhunt.agents-India). Happy hunting. 🇮🇳*
