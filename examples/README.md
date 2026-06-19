# Examples

Reference files that demonstrate jobhunt-india data formats and conventions. None of these are used at runtime -- they exist so you can see the expected structure before creating your own files.

## Files

| File | Demonstrates |
|------|-------------|
| `cv-example.md` | How to structure `cv.md` -- sections, metrics formatting, and proof-point style for a fictional AI engineer (Alex Chen) |
| `resume-example.md` | Resume variant of `cv-example.md` -- same content, branded as "Resume" for US/industry markets. Use this as a structural guide when writing a resume (1–2 page targeted format) vs a CV (longer academic format) |
| `cv-experienced-india-example.md` | India-format CV for an **experienced** professional (fictional backend engineer, ~5 yrs, Bengaluru). Leads with impact; includes CGPA, ₹ LPA CTC, and a notice-period footer. Pairs with `templates/cv-experienced-india.html` |
| `cv-fresher-india-example.md` | India-format CV for a **fresher / campus placement** (fictional final-year B.Tech CSE student). Leads with education (CGPA, 12th/10th %), then projects, internships, and coding profiles. Pairs with `templates/cv-fresher-india.html` |
| `article-digest-example.md` | How to write `article-digest.md` -- compact proof points with hero metrics, architecture summaries, and key decisions per project |
| `sample-report.md` | The A-F evaluation report format produced by the evaluation pipeline, with all six blocks (Role Summary through Interview Plan) |
| `ats-normalization-test.md` | Regression fixture for `generate-pdf.mjs` Unicode normalization -- lists every problematic codepoint and its ASCII-safe replacement |
| `dual-track-engineer-instructor/` | Complete profile config for a candidate with two primary archetypes (engineer + instructor), including `cv.md`, `profile.yml`, and a README explaining when and how to use the dual-track pattern |

## Usage

These files are read-only references. To set up your own jobhunt-india instance:

1. Run `npm run doctor` to check prerequisites.
2. Use `cv-example.md` (or `resume-example.md` for US/industry contexts) as a structural guide when writing your `cv.md`.
3. Use `article-digest-example.md` as a template for your `article-digest.md` (optional but improves evaluation quality).
4. See the `dual-track-engineer-instructor/` folder if your career spans two distinct archetypes.

## India formats: fresher vs experienced

Two India-specific examples and their matching templates follow Indian resume norms — A4 paper, `+91 XXXXX XXXXX` phone, CGPA **and** percentages, ₹ LPA for CTC, and no photo/DOB/marital status (legacy fields that hurt ATS parsing and invite bias).

- **`cv-fresher-india-example.md`** (template: `cv-fresher-india.html`) — use for **students, freshers, and campus placements** (final-year or 0–1 yr). Leads with a Career Objective and **Education first** (Degree + CGPA, then 12th and 10th with board and percentage), followed by Technical Skills, Projects, Internships/Training, Certifications, Positions of Responsibility, and optional Coding Profiles (LeetCode/GitHub/HackerRank).
- **`cv-experienced-india-example.md`** (template: `cv-experienced-india.html`) — use for **working professionals (roughly 1–15 yrs)**. Leads with a metric-driven Summary and Work Experience; Education moves below. Includes an optional "Additional Details" footer for Notice Period and Current/Expected CTC, which Indian recruiters routinely ask for.

Rule of thumb: if your strongest signal is your degree and projects, use the fresher format; once you have real work impact to show, switch to the experienced format.
