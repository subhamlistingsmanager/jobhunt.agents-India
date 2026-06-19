# Résumé Design System

The design system behind the two India résumé templates — `templates/cv-experienced-india.html` and `templates/cv-fresher-india.html`. Both share the exact same tokens (defined in `:root` in each file), so they look like one family.

The guiding rule: **a résumé is read by a machine first and a human second.** The design earns attention without ever risking ATS parsing. Consistency over creativity.

---

## Principles

1. **Single restrained accent.** One colour, used only where it guides the eye — section titles, the name rule, the target-role line, links, and organisation names. Everything else is ink or grey. No second accent, no gradients, no coloured boxes.
2. **One typeface.** An ATS-safe system sans stack (Liberation Sans / Helvetica / Arial). The bundled display webfonts are deliberately *not* used — their glyph spacing makes PDF text extractors inject spurious spaces inside words ("SUM M ARY") and corrupt keyword parsing.
3. **One type scale, one spacing rhythm.** Fixed steps (below). Nothing is sized ad hoc.
4. **Single column, A4.** Multi-column and sidebars confuse many Indian ATS (Naukri RMS, Workday, Greenhouse). One column, top-to-bottom, A4 (Indian standard — not US Letter).
5. **No bias-inviting fields.** No photo, date of birth, marital status, gender, or parent's name. They hurt ATS parsing and invite bias; both templates omit them by design.

---

## Tokens

```css
:root {
  /* Colour — one accent only */
  --accent:      #1f3a5c;  /* deep professional navy — titles, name rule, links, org names */
  --accent-tint: #eef2f7;  /* faint wash, used only on the fresher project badge */
  --ink:         #16181d;  /* names, headings, emphasis (bold metrics) */
  --body:        #2c2f36;  /* body copy + bullets */
  --muted:       #5f6672;  /* contact line, supporting text */
  --meta:        #8b909b;  /* dates, locations, least-important meta */
  --line:        #e2e4ea;  /* hairlines + section dividers */

  /* Type scale */
  --fs-name: 24px;   /* candidate name */
  --fs-role: 11.5px; /* target-role line */
  --fs-h2:   10.5px; /* section titles (uppercase, tracked) */
  --fs-body: 10.7px; /* summary, bullets */
  --fs-meta: 10px;   /* dates, contact, certs */

  /* Spacing rhythm (4px base) */
  --space-1: 4px;  --space-2: 8px;  --space-3: 12px;  --space-4: 16px;
}
```

Why navy: it reads as serious and trustworthy, prints cleanly in black-and-white (common when a recruiter prints to PDF), and clears WCAG AA contrast on white at every text size used.

---

## Hierarchy

| Element | Token(s) | Treatment |
|---|---|---|
| Name | `--fs-name`, `--ink`, 700 | Largest thing on the page. Tight tracking (-0.01em). |
| Target role | `--fs-role`, `--accent`, 600 | One line under the name — the role you're applying for. |
| Accent rule | 2px solid `--accent` | A single calm rule under the header. (Replaced the old two-colour gradient.) |
| Section title | `--fs-h2`, `--accent`, 700, uppercase, 0.09em tracking | Hairline (`--line`) underneath. |
| Company / college | 12px, `--ink`, 700 | Dark, not coloured — keeps the accent disciplined. |
| Role / degree | `--accent`, 600 | The one place inside a block the accent reappears. |
| Bullets | `--fs-body`, `--body` | Metrics in bullets get `<strong>` in `--ink`. |
| Dates / location | `--fs-meta`, `--meta` | Quietest text on the page. |

Accent appears in exactly five roles: section titles, the name rule, the target-role line, links, and org/role lines. If you find yourself adding a sixth use, stop — that's the noise this system avoids.

---

## ATS rules baked in

- System-font stack only (clean text extraction).
- Real text everywhere — no text inside images, no icon fonts for content.
- Single column; no tables for layout except the certifications/education alignment rows (which still extract linearly).
- Links never wrap (`white-space: nowrap`) so a URL doesn't split mid-token.
- `generate-pdf.mjs` normalises smart quotes / em-dashes / zero-width characters to ASCII and validates that the rendered section order matches your `cv.md`.

---

## Do / Don't

| ✅ Do | ❌ Don't |
|---|---|
| Keep to one page (experienced ≤ 15 yrs, all freshers) | Spill to page 2 to fit every bullet |
| Lead bullets with the outcome and a number | Open every bullet with "Responsible for…" |
| Put metrics in bold (`--ink`) | Bold whole sentences |
| Use the accent only where the system says | Add a second colour or a gradient |
| Trim skills to what the JD needs | Dump a 40-keyword skills wall |
| Keep CTC/notice in the optional footer (or omit) | Add a photo, DOB, or marital status |

---

## Extending

To re-theme (e.g. a different accent for a design/creative field), change `--accent` (and `--accent-tint`) in **both** templates so they stay a family. Keep the new accent dark enough to pass AA on white and to survive black-and-white printing. Don't add tokens for a second accent — the discipline is the point.

To add a section, copy an existing `.section` block and add a matching `{{PLACEHOLDER}}`; keep the rendered order identical to the user's `cv.md` (enforced by `generate-pdf.mjs`).
