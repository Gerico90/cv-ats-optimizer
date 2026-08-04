# ATS rules and scoring rubric (2026)

Detailed reference for auditing and rewriting a resume. Based on the real behavior of the most widely used ATS (Workday, Greenhouse, Lever, iCIMS, Taleo, SmartRecruiters).

## Contents
1. Parseability rules (format)
2. Content and keyword rules
3. Section-by-section rules
4. File format
5. Scoring rubric (0–100)

---

## 1. Parseability rules (format)

This determines whether the ATS can *read* the resume. It's a data-integrity problem, not an aesthetic one.

- **Single column.** The most important rule. Two-column designs are read out of order by most parsers (Taleo and iCIMS are the worst): they interleave the left and right columns, and skills/experience lose their context. Keep all text in one linear top-to-bottom flow.
- **No layout tables.** Tables get flattened and scrambled. If you need to align something, use the text flow's own tabs/spacing, not cells.
- **No text boxes.** Many parsers skip them entirely.
- **No vital information in page headers/footers.** Roughly 2 out of 3 ATS ignore header/footer content. **Contact details (name, email, phone) belong in the body**, at the top.
- **No graphics, icons, logos, photos or skill-level bars.** The parser sees an image = empty space = zero keywords. An "Excel ●●●●○" bar contributes zero matches; write "Excel" as text.
- **Standard system fonts:** Arial, Calibri, Helvetica, Georgia, Garamond, Times New Roman. Custom/decorative fonts may fail to render or corrupt characters on text extraction.
- **Sizes:** body 10–12pt, section headings 13–14pt, name 16–18pt. Don't go below 10pt to compress (it doesn't help the parser and signals badly to the human).
- **One accent color maximum** for headings; body text dark on white.
- **Reverse-chronological format.** The "functional" format (skills only, no clear timeline) confuses parsers and raises suspicion.

## 2. Content and keyword rules

This determines how the ATS *scores* the resume against the posting.

- **Copy the posting's exact phrases.** Matching is usually literal: if the posting says "cross-functional collaboration", use that phrase, not "team coordination". Exact match > synonym.
- **List skills explicitly.** Don't count on the ATS inferring skills from narrative prose. Group them by category: Technical skills, Tools and platforms, Certifications, Languages.
- **Acronyms: long form + short form on first use.** "Search Engine Optimization (SEO)". Different ATS index different forms.
- **Aligned job title.** If your actual title is equivalent to the posting's, mirror the posting's language in the summary (without lying about the title you held).
- **Quantified achievements.** Metrics in 60–70% of bullets. "Cut costs by 23%" scores and persuades better than "cut costs". Concrete numbers: %, $, volumes, timelines, team size.
- **Action verbs** at the start of every bullet (Led, Designed, Automated, Increased…).
- **Honest density.** Repeating a key keyword 2–3 times in real contexts is fine; keyword stuffing (hidden lists, white text) is penalized and detectable.

## 3. Section-by-section rules

Recommended order (2026 trend toward skills-based screening):
**Contact → Professional Summary → Skills → Experience → Education → Certifications**
(The classic Summary → Experience → Education → Skills is also valid.)

- **Contact:** name, phone, email, city, LinkedIn. Plain text in the body. No icons.
- **Professional Summary:** 3–4 lines. Position the candidate for *this* posting and include 2–3 of its main keywords. It is not a generic objective statement.
- **Skills:** grouped text list. This is where the keyword gap closes fastest.
- **Experience:** reverse-chronological. Each entry: Title — Company — Location — Dates. Quantified achievement bullets. Use the literal section heading "Experience" or "Professional Experience" (avoid creative names like "My Journey").
- **Education:** degree, institution, year. Literal heading "Education".
- **Certifications:** full name + acronym + year/issuer.
- **Standard headings generally:** the parser classifies content by section name. Use conventional names, not clever ones.

### Dates
- **Consistent format throughout.** The parser computes tenure and detects gaps from dates; mixing "05/2023", "'23", "May 23" produces wrong calculations.
- Recommended format: "Jan 2023 – Mar 2025" or "January 2023 – Present". Pick one and stick to it.
- Don't omit dates to hide gaps: ATS and recruiters expect them, and their absence looks suspicious.

### Length
- Modern ATS handle 2 pages without penalty. Let the content decide (1 page for a junior profile, 2 for senior). Don't shrink below 10pt to force a single page.

## 4. File format (2026)

- **Selectable-text PDF** is parsed reliably by modern ATS (Workday, Greenhouse, Lever, iCIMS) in 2026. It's safe and preserves the layout for the human who reads it afterwards.
- **The PDF must be generated from text, not as an image.** Canva/InDesign/scanned PDFs with rasterized text score ~0 because the parser sees no characters. Test: try to select/copy the text; if it highlights, you're fine.
- **.docx is still the safest bet for legacy systems** (older Taleo) and whenever the posting asks for "Word". Marginally more universally compatible. Offer to generate a .docx version if the destination requires it.
- **Always follow the posting's explicit instructions.** If it says "Word document only", submit .docx even though the PDF is technically better.
- **Never** password-protect the PDF (the parser can't open it) or use formats requiring special software (.pages, .jpg, .png).
- File name: `First_Last_Resume.pdf`.

## 5. Scoring rubric (0–100)

Derive the score from verifiable criteria; don't invent the number. Split 100 points across two blocks.

### Parseability — 50 pts
| Criterion | Pts |
|---|---|
| Text is extractable (not an image) | 15 |
| Single column / correct reading order | 10 |
| No tables, text boxes, graphics, bars | 8 |
| Contact in the body (not in header/footer) | 6 |
| Standard fonts and correct sizes | 5 |
| Standard section headings | 6 |

### Content and job match — 50 pts
| Criterion | Pts |
|---|---|
| Keyword match with the job description (proportional; ~75%+ = full marks) | 20 |
| Skills listed explicitly and grouped | 8 |
| Quantified achievements (60–70% of bullets) | 10 |
| Consistent, complete dates | 6 |
| Professional summary aligned to the posting | 6 |

**Match with the posting (%)** = (posting keywords present in the resume) / (relevant keywords the candidate genuinely meets) × 100. Report it before and after. Practical target: ≥75%.

Compute the "original ATS score" on the original resume and the "optimized ATS score" on the rewritten version, using the same rubric, so the improvement is comparable and honest.
