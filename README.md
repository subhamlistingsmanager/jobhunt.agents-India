# Job Hunts Agent India 🇮🇳

**An AI job-search co-pilot for Indian candidates — freshers, students, and working professionals.**

Run it inside your AI coding CLI (Claude Code, Antigravity, Cursor, GitHub Copilot, Gemini CLI, and others). Paste a job, and it reads the JD against your CV, tells you whether it's worth applying, builds an India-format resume tailored to that role, and tracks every application in one place. Everything runs **locally** — you always have the final say before anything is sent.


---

## Why this exists

Job hunting in India is its own game: CTC negotiations in lakhs, 60–90 day notice periods, Naukri vs LinkedIn vs Instahyre, fresher resumes that lead with 10th/12th marks and CGPA, and ATS filters that quietly reject 70% of resumes. This tool handles that game for you:

- **Two resume formats, the Indian way** — a **fresher** format (academics-first: 10th/12th %, CGPA, projects, internships, coding profiles) and an **experienced** format (impact-first: work history, achievements, optional CTC/notice footer). Both are ATS-clean and A4.
- **Indian job sources** — scans Indian startups that use Greenhouse/Lever/Ashby for free, and gives you ready-made searches for **Naukri, LinkedIn India, Instahyre, Cutshort, Hirist, Wellfound, Foundit, IIMJobs, Internshala, Unstop**.
- **Honest fit scoring** — a structured A–G evaluation (CV match, level strategy, comp, interview plan, plus a legitimacy check that flags ghost jobs/scams). It tells you *not* to apply when the fit is weak. Quality over spray-and-pray.
- **India comp intelligence** — comp research from AmbitionBox, Glassdoor India, Levels.fyi, 6figr; everything framed as CTC in ₹ LPA (fixed vs variable vs ESOP), with notice-period and counter-offer strategy.

> **This is a filter, not a spam cannon.** It helps you find the few roles worth your time out of hundreds, and write a genuinely tailored application for each. It never auto-submits.

---

## Quick start

You need [Node.js](https://nodejs.org) 18+ and an AI coding CLI (e.g. [Claude Code](https://claude.com/claude-code)).

```bash
# 1. Clone
git clone https://github.com/subhamlistingsmanager/jobhunt.agents-India.git
cd jobhunt.agents-India

# 2. Install
npm install
npx playwright install chromium     # only needed for PDF resume generation

# 3. Check setup
npm run doctor

# 4. Open your AI CLI in this folder
claude        # or: cursor / copilot / gemini / opencode
```

**On first launch, just chat.** The agent walks you through setup — your CV, whether you're a fresher or experienced, target roles, target cities, expected CTC, and notice period. Nothing to edit by hand.

> 📖 **New here? Read the [step-by-step How-to-Use guide](docs/HOW-TO-USE.md)** — install → setup → evaluating jobs → building your résumé → tracking, with a full day-in-the-life walkthrough.

Prefer to set it up manually?

```bash
cp config/profile.example.yml config/profile.yml   # your details (CTC, notice, cities)
cp templates/portals.example.yml portals.yml        # companies + Indian job boards
# create cv.md in the project root with your CV in markdown
```

Then tell the agent things like:
- *"I'm a final-year B.Tech student — use the fresher resume format."*
- *"Target only Bengaluru, Hyderabad, and remote roles."*
- *"My notice period is 60 days and expected CTC is 28 LPA."*
- *"Add Zerodha, Groww, and Postman to my tracked companies."*

---

## How you'll use it day to day

| You want to… | Command |
|---|---|
| Evaluate a job (paste JD text or a URL) | `/jobhunt {paste JD or URL}` |
| Process a batch of saved job URLs | `/jobhunt pipeline` |
| Scan portals for new matching roles | `/jobhunt scan` |
| Build a tailored India-format resume PDF | `/jobhunt pdf` |
| Write a cover letter | `/jobhunt cover` |
| Prep for an interview at a company | `/jobhunt interview-prep` |
| Draft a LinkedIn referral message | `/jobhunt contacto` |
| See all your applications | `/jobhunt tracker` |
| Compare multiple offers | `/jobhunt ofertas` |
| See every command | `/jobhunt` |

**Typical flow:** find a role on Naukri/LinkedIn → paste its URL into the agent → get an A–G evaluation + a tailored A4 resume → review it → apply yourself → the application is logged in your tracker.

---

## What's inside

- **`CLAUDE.md`** — the system's brain (rules the agent follows). `AGENTS.md` / `GEMINI.md` mirror it for other CLIs.
- **`modes/`** — the playbooks: evaluation, scan, pdf, cover, interview, apply, tracker, and more.
- **`templates/`** — `cv-experienced-india.html`, `cv-fresher-india.html`, cover letter, and the portals config.
- **`providers/`** — zero-token scanners for ATS boards (Greenhouse, Lever, Ashby, Workable, …).
- **`config/profile.yml`** — your details. **`data/`** — your tracker, pipeline, and history.

**📚 Documentation**
- [How-to-Use guide](docs/HOW-TO-USE.md) — the complete walkthrough, start to finish.
- [Indian job sources](docs/INDIAN-JOB-SOURCES.md) — every board + an India comp/process glossary (CTC, in-hand, notice buyout, gratuity, EPF, BGV, bond…).
- [Résumé design system](docs/RESUME-DESIGN.md) — the tokens, hierarchy, and ATS rules behind the two India résumé formats.

It's **designed to be edited by the agent itself.** Want different scoring, more cities, a new resume section, or your own list of companies? Just ask — it reads the same files it writes.

---

## Your data stays yours

There are two layers (see `DATA_CONTRACT.md`):

- **User layer** (never auto-overwritten): `cv.md`, `config/profile.yml`, `modes/_profile.md`, `data/*`, `reports/*`, `portals.yml`.
- **System layer** (safe to update/improve): `CLAUDE.md`, `modes/*` (except `_profile.md`), scripts, templates.

Personal choices always go in the user layer, so improving the engine never wipes your customizations.

---

## Indian job sources at a glance

- **Scannable for free** (ATS-backed): Razorpay, Zerodha, Groww, CRED, Swiggy, Meesho, Flipkart, Postman, Freshworks, BrowserStack, and other startups on Greenhouse/Lever/Ashby → `/jobhunt scan`.
- **Paste-the-URL** (no public API): Naukri, LinkedIn India, Indeed, Foundit, Instahyre, Cutshort, Hirist, Wellfound, IIMJobs, Apna, Internshala, Unstop → paste a role URL, then `/jobhunt pipeline`.
- **Large IT services** (TCS, Infosys, Wipro, HCL, Accenture): apply on their own portals / campus drives; paste specific role URLs to evaluate.

Full details and tips in **[docs/INDIAN-JOB-SOURCES.md](docs/INDIAN-JOB-SOURCES.md)**.

---

## Ethical use

- **It never submits an application for you.** It evaluates, drafts, and builds resumes — you click Apply.
- **It discourages low-fit applications.** Below ~3.5/5, it recommends against applying. Your time and the recruiter's time both matter.
- **No fabrication.** It only rewords your real experience using the JD's vocabulary — it never invents skills, metrics, or experience.

Not professional career advice; it's a personal decision-support tool. See `LEGAL_DISCLAIMER.md`.

---

## Credits & licence

India adaptation maintained by [@ca-who-codes](https://github.com/ca-who-codes).

Markets and job hunts carry uncertainty — use your own judgement on every application.
