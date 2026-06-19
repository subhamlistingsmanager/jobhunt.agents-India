# Indian Job Sources

A practical guide to where Indian jobs live and how this agent reaches each one. Two questions decide the workflow for any source:

1. **Does it have a public, machine-readable ATS feed?** If yes, `/jobhunt scan` can list roles zero-token.
2. **If not**, you paste a JD URL into `data/pipeline.md` and run `/jobhunt pipeline`, or paste the JD text straight into the agent for an A–G evaluation.

Most Indian job boards fall into the second group. That is normal — the agent is built to handle both.

---

## 1. ATS-backed sources (scannable, zero-token)

These run on hosted applicant-tracking systems (ATS) the scanner already understands: **Greenhouse, Lever, Ashby, Workable, SmartRecruiters, Recruitee**. The scanner reads their public job feeds directly, so it can list open roles without a browser and without spending tokens. See `modes/scan.md` for the mechanics and `docs/local-parser-cookbook.md` for adding a company.

Many Indian startups and scale-ups post on these. Examples seen in the wild include Razorpay, Zerodha, Groww, CRED, Meesho, Postman, Hasura, and similar product companies. (Always confirm a company's current ATS before adding it — companies switch providers.)

| Provider | Typical career-page shape | How the agent uses it |
|----------|---------------------------|------------------------|
| Greenhouse | `job-boards.greenhouse.io/{slug}` | `/jobhunt scan` via public board API |
| Lever | `jobs.lever.co/{slug}` | `/jobhunt scan` via postings API |
| Ashby | `jobs.ashbyhq.com/{slug}` | `/jobhunt scan` via job-board GraphQL |
| Workable | `apply.workable.com/{slug}` | `/jobhunt scan` (feed) |
| SmartRecruiters | `careers.smartrecruiters.com/{slug}` | `/jobhunt scan` (feed) |
| Recruitee | `{slug}.recruitee.com` | `/jobhunt scan` (feed) |

**How to use:** add the company to `tracked_companies` in `portals.yml` with its `careers_url`, then run `/jobhunt scan`. New roles land in `data/pipeline.md` for evaluation. This is the cheapest, most reliable path — prefer it whenever a target company is on one of these systems.

---

## 2. Indian job boards WITHOUT a public ATS API (browser / WebSearch-driven)

These have no open feed the scanner can read. Use them one of two ways:

- **Paste-the-URL:** copy a specific job URL into `data/pipeline.md` under Pending, then run `/jobhunt pipeline`. The agent opens the page, confirms it is live, and runs the full evaluation.
- **Paste-the-JD:** copy the job description text straight into the agent and ask for an evaluation. Use this when the URL sits behind a login (common on Naukri/LinkedIn).
- **Discovery by search:** ask the agent to run a WebSearch `site:` query to surface roles, then feed the good ones into the pipeline.

| Board | Best for | Typical roles / seniority | How to use with this agent |
|-------|----------|---------------------------|----------------------------|
| **Naukri.com** | India's largest general board; widest coverage across sectors | All levels, all functions | Paste the JD text (listings often need login). For discovery: WebSearch `site:naukri.com "{role}" {city}` |
| **LinkedIn (India)** | Professional roles, referrals, recruiter reach | Mid to senior, white-collar | Paste the job URL or JD. Pair with `/jobhunt contacto` for referral outreach. Discovery: `site:linkedin.com/jobs "{role}" India` |
| **Indeed India** | Aggregated listings, broad volume | Entry to mid, many sectors | Paste JD or URL into pipeline. Discovery: `site:in.indeed.com "{role}"` |
| **Foundit** (ex-Monster) | General board, decent for IT and non-IT | Mid level | Paste JD or URL. Discovery: `site:foundit.in "{role}"` |
| **Instahyre** | Curated tech/product hiring, recruiter-matched | Engineering, product, design; mid–senior | Paste JD or URL. Often invite/login-gated, so paste-the-JD is usually easier |
| **Cutshort** | AI-matched startup tech roles | Engineering, product; junior–senior | Paste JD or URL into pipeline |
| **Hirist** | Tech-only board | Software/IT roles, all levels | Paste JD or URL. Discovery: `site:hirist.tech "{role}"` |
| **Wellfound** (ex-AngelList Talent) | Startup roles, often with equity shown | Startup eng/product/GTM; all levels | Paste JD or URL. Useful when comparing ESOP-heavy offers |
| **IIMJobs** | MBA / finance / leadership / consulting | Mid–senior, management | Paste JD or URL. Discovery: `site:iimjobs.com "{role}"` |
| **Apna** | Entry-level and blue/grey-collar | Freshers, field, ops, support | Paste JD. Mobile-first; paste-the-JD is most reliable |
| **Shine** | General board (Hindustan Times group) | Entry to mid | Paste JD or URL |
| **TimesJobs** | General board (Times group) | Entry to mid | Paste JD or URL |
| **Internshala** | Internships and fresher jobs | Students, freshers, interns | Paste JD or URL. Good for early-career searches |
| **Unstop** (ex-Dare2Compete) | Campus hiring, competitions, hackathons | Students, freshers; some lateral | Paste JD or URL. Many roles come via contests, not standard postings |

**Notes:**
- Naukri and LinkedIn carry the bulk of Indian listings. Treat them as the default discovery surface, with `paste-the-JD` as the fallback whenever a listing hides behind a login wall.
- Many of these boards re-host roles that also live on a company's own ATS. If a company you track is on Greenhouse/Lever/Ashby, scan it there instead — the source data is fresher and zero-token.
- For any board with stable, public HTML you can write a local parser (see `docs/local-parser-cookbook.md`) so `/jobhunt scan` covers it. Login-gated boards cannot be parsed this way.

---

## 3. Company-direct career pages

Large Indian IT-services firms and many enterprises run their own career portals on **Workday, Darwinbox, or SAP SuccessFactors**. Examples include TCS, Infosys, Wipro, HCLTech, and Accenture India. These are browser-driven: there is no clean public feed, and listings often require search and pagination.

| Platform | Where you'll see it | How to use |
|----------|---------------------|------------|
| Workday | `{company}.wd{n}.myworkdayjobs.com/...` | Paste the **specific role URL** into the pipeline. Some Workday tenants expose a feed the scanner can read; most need paste-the-URL |
| Darwinbox | Company careers subdomain | Paste the specific role URL |
| SuccessFactors | `career{n}.successfactors.eu/...` or company portal | Paste the specific role URL |

**Rule:** paste the URL of the **exact role**, not the search-results page. A search page has no single JD to evaluate, and the agent's liveness gate will treat it as a dead/closed posting. Open the role, confirm it loads, then drop that URL into `data/pipeline.md`.

---

## 4. Indian comp & process glossary

Neutral, factual definitions of terms you will meet in Indian listings, application forms, and offer letters. None of these are guarantees — they describe common practice, which varies by employer.

| Term | Meaning |
|------|---------|
| **CTC** (Cost to Company) | The employer's total annual spend on you. Includes fixed pay, variable/bonus, employer PF contribution, gratuity, and often the notional value of ESOP/RSU. A high CTC is not the same as high take-home. |
| **Fixed pay** | The guaranteed portion of CTC, paid monthly regardless of performance. |
| **Variable pay** | Performance- or company-linked component (quarterly/annual bonus). Realised amount can be below 100% of the stated figure. |
| **ESOP / RSU** | Employee stock options or restricted stock units. Often quoted inside CTC at a notional value; actual worth depends on vesting, the company's valuation, and an eventual liquidity event. Treat as upside, not salary. |
| **In-hand salary** | Monthly cash credited to your bank after deductions (PF, professional tax, income tax/TDS). Lower than fixed pay; see the rough orientation in `modes/oferta.md` Block D. |
| **LPA** (lakhs per annum) | The standard unit for quoting annual pay. 1 lakh = ₹100,000. "12 LPA" = ₹12,00,000 CTC per year. |
| **Notice period** | Time you must serve after resigning before you can leave. Commonly **30, 60, or 90 days**. Longer periods are typical in IT services. |
| **Buyout / shortfall recovery** | Paying to leave before the notice period ends. Either you (or the new employer) buy out the balance, or the current employer recovers the shortfall from your dues. |
| **Joining bonus** | One-time payment on joining, sometimes used to offset a notice-period buyout. May carry a clawback if you leave early. |
| **Retention bonus** | A payment tied to staying for a set period; usually clawed back if you leave before it vests. |
| **Gratuity** | A statutory lump sum for employees who complete (generally) five years of continuous service, calculated on last-drawn pay. Often shown as a line in CTC. |
| **EPF / PF** (Employees' Provident Fund) | Mandatory retirement savings. Employee and employer each contribute a percentage of basic pay; the employer share usually sits inside CTC. |
| **Form 16** | The annual TDS certificate your employer issues, summarising salary paid and tax deducted. Used when filing income tax. |
| **Relieving letter** | Document from your previous employer confirming you served notice and left in good standing. Often required by the next employer before or at joining. |
| **Background verification (BGV)** | Pre- or post-joining checks on employment history, education, and sometimes criminal/address records. A failed or gappy BGV can delay or revoke an offer. |
| **Service agreement / bond** | A contract requiring you to stay for a fixed term (common in IT services, especially after paid training), with a penalty for leaving early. Read the terms before signing. |

---

## 5. How to get the most out of this in India

> **Tips**
>
> - **Use `/jobhunt scan` for startups on standard ATS.** Razorpay, Groww, CRED-type companies on Greenhouse/Lever/Ashby scan zero-token. Add them to `tracked_companies` in `portals.yml`.
> - **Use paste-the-URL pipeline for Naukri / LinkedIn / Indeed.** These have no feed. Drop the role URL into `data/pipeline.md` and run `/jobhunt pipeline`, or paste the JD text directly if the page needs a login.
> - **Paste the exact role URL for IT-services portals** (TCS, Infosys, Wipro, HCL, Accenture). Never the search page — the liveness gate will reject it.
> - **Keep `cv.md` ATS-clean.** Plain headings, no tables/columns/graphics in the parsed text, standard section names. Indian ATS (and Workday/Darwinbox parsers) reward simple structure.
> - **Fill notice period and expected CTC in your profile.** Set `cover_letter.notice_period_days` and your comp expectations in `config/profile.yml` so the agent answers "Notice Period" and "Expected CTC" form fields without guessing.
> - **Lean on referrals.** Many Indian roles fill through referrals. After evaluating a role, run `/jobhunt contacto` to draft polite outreach and, where appropriate, ask for a referral.
