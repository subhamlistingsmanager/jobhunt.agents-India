# Templates

System-layer template files used by jobhunt-india scripts and modes. These files are auto-updated when you run `npm run update` -- put user customizations in the user-layer files instead (see DATA_CONTRACT.md).

## Files

| File | Used By | Purpose |
|------|---------|---------|
| **`cv-experienced-india.html`** | `generate-pdf.mjs` | **India ATS resume for experienced professionals** (impact-first). Default for `candidate_type: experienced`. A4. See section below. |
| **`cv-fresher-india.html`** | `generate-pdf.mjs` | **India ATS resume for freshers/students** (academics-first). Default for `candidate_type: fresher`. A4. See section below. |
| `cv-template.html` | `generate-pdf.mjs` | Original generic ATS CV template (kept as a base; the India templates extend its CSS) |
| `resume-template.html` | `generate-pdf.mjs` (via `--template`) | Resume-branded variant of `cv-template.html`. Same layout and placeholder tokens; differs in: `<title>` reads "Resume" instead of "CV", omits Certifications section, targets 1–2 page US/industry format. See detailed section below. |
| `cv-template.tex` | `generate-latex.mjs` | LaTeX/Overleaf template for ATS-optimized CV PDFs |
| `portals.example.yml` | Onboarding | Example portal scanner configuration (copy to `portals.yml` to activate) |
| `states.yml` | `verify-pipeline.mjs`, `normalize-statuses.mjs`, `merge-tracker.mjs` | Canonical application states and their aliases |

### India resume templates (use these by default)

**`cv-experienced-india.html`** and **`cv-fresher-india.html`** are the default resume formats for Job Hunts Agent India. Both share ONE design system — a single navy accent, one type scale, one spacing rhythm, defined as tokens in `:root` (see [`docs/RESUME-DESIGN.md`](../docs/RESUME-DESIGN.md)). They keep `cv-template.html`'s ATS-safe single-column, system-font approach and render at **A4** (Indian standard). Neither includes a photo, DOB, marital status, or parent's name — those legacy fields hurt ATS parsing and are intentionally omitted.

`modes/pdf.md` picks between them from `config/profile.yml` (`cv.template` → else `candidate_type`):

- **`cv-experienced-india.html`** — Professional Summary → Core Skills → Work Experience → Key Achievements → Education → Certifications → optional Additional Details footer (Notice Period, Current/Expected CTC). New placeholders: `{{TARGET_ROLE}}`, `{{GITHUB_URL}}`, `{{GITHUB_DISPLAY}}`, `{{SECTION_ACHIEVEMENTS}}`, `{{ACHIEVEMENTS}}`, `{{ADDITIONAL_DETAILS}}`.
- **`cv-fresher-india.html`** — Career Objective → Education (10th %, 12th %, Degree+CGPA — first) → Technical Skills → Projects → Internships/Training → Certifications → Positions of Responsibility/Achievements → optional Coding Profiles. New placeholders: `{{SECTION_OBJECTIVE}}`, `{{OBJECTIVE_TEXT}}`, `{{SECTION_INTERNSHIPS}}`, `{{INTERNSHIPS}}`, `{{SECTION_RESPONSIBILITIES}}`, `{{RESPONSIBILITIES}}`, `{{SECTION_CODING_PROFILES}}`, `{{CODING_PROFILES}}`.

Each template documents its full placeholder list in a comment at the top of the file. The rendered section order must match the user's `cv.md` (enforced by `generate-pdf.mjs`). Example CVs: `examples/cv-experienced-india-example.md`, `examples/cv-fresher-india-example.md`.

### cv-template.html

The HTML template rendered by Playwright into PDF. Uses placeholder tokens (`{{NAME}}`, `{{SUMMARY_TEXT}}`, `{{EXPERIENCE}}`, etc.) that the PDF pipeline fills at generation time.

**Design:** Space Grotesk headings + DM Sans body, single-column ATS-safe layout, self-hosted fonts from `fonts/`.

**Customization:** Edit this file to change colors, spacing, or section order. The placeholder tokens are documented in `batch/batch-prompt.md` under "Template placeholders."

### resume-template.html

Resume-branded variant of `cv-template.html` for US/industry job applications. Key differences from the CV template:

- **Title** reads "Resume" instead of "CV"
- **No Certifications section** — resumes focus on recent, relevant experience
- **Designed for 1–2 pages** — omits academic-style sections

Otherwise uses the same placeholder tokens (`{{NAME}}`, `{{SUMMARY_TEXT}}`, etc.) and is fully compatible with the existing PDF pipeline.

**Keep in sync:** When updating `cv-template.html`, apply matching changes to `resume-template.html` (preserving the differences noted above).

### cv-template.tex

LaTeX template for Overleaf-compatible CV generation. Based on the [sb2nov/resume](https://github.com/sb2nov/resume) format. Uses placeholder tokens (`{{NAME}}`, `{{EXPERIENCE}}`, `{{PROJECTS}}`, etc.) that the LaTeX pipeline fills at generation time.

**Design:** Single-column ATS-safe layout using standard CTAN packages (`fontawesome5`, `enumitem`, `hyperref`, `titlesec`). No custom fonts or external dependencies — uploads directly to Overleaf.

**Usage:**
```bash
# Validate and compile .tex → .pdf (requires pdflatex on PATH)
node generate-latex.mjs output/cv-name-company-date.tex

# Or specify a custom output path
node generate-latex.mjs output/cv-name-company-date.tex output/custom-name.pdf
```

**Prerequisites:** `pdflatex` via [MiKTeX](https://miktex.org/) (Windows) or TeX Live (Linux/macOS). First compilation may auto-install missing LaTeX packages. Alternatively, upload the `.tex` file directly to [Overleaf](https://www.overleaf.com) — no local install needed.

**Customization:** Edit this file to change margins, section order, or formatting commands. The placeholder tokens are documented in `modes/latex.md` under "Template Placeholders."

### portals.example.yml

Pre-configured portal scanner with 45+ tracked companies and search queries. Contains title filters, company career page URLs, Greenhouse API endpoints, and WebSearch queries.

**To activate:** Copy to project root as `portals.yml` and customize `title_filter.positive` keywords for your target roles. Add or remove companies as needed.

### states.yml

Defines the 8 canonical application states (`Evaluated`, `Applied`, `Responded`, `Interview`, `Offer`, `Rejected`, `Discarded`, `SKIP`) with aliases for common variants. All pipeline scripts validate statuses against this file.

**Do not rename states** -- the dashboard and all scripts depend on these exact IDs. You can add aliases if you encounter new variants that should map to an existing state.
