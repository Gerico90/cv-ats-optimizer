---
name: cv-ats-optimizer
description: Optimizes a resume/CV to pass ATS (Applicant Tracking System) filters against a specific job posting, using the user's master CV (references/master-cv.md) or a resume the user uploads. Use it whenever the user pastes or mentions a job posting, job ad or job description and wants a tailored resume, or uploads a resume and wants to improve it, beat the ATS or raise their match rate. Typical phrases are 'optimize my resume', 'tailor it to this role', 'here is the job description' or 'I want to apply to this'. It delivers two things. First, an optimized resume as a selectable-text, single-column, parseable PDF. Second, a report with scores, keyword analysis and the changes made. Trigger it even if the user never says the word ATS.
---

# ATS Resume Optimizer

This skill turns a person's resume into a version optimized for ATS software **against one specific job posting**, and delivers a report explaining what changed and why.

## What an ATS is (quick context)

An ATS (Applicant Tracking System) is the software companies use to filter resumes before a human reads them. It works in two phases: (1) it **parses** the file, extracting plain text and sorting it into fields (name, job title, dates, skills…), and (2) it **scores/ranks** the resume by how well it matches the posting, mostly on keywords. A resume can be excellent and still get dropped if the parser can't read it, or if it lacks the posting's keywords. This skill solves both halves: **parseability** and **keyword match**.

## Required inputs

1. **The source resume** — it can come from two places:
   - **Master CV** (`references/master-cv.md`): the candidate's single source of truth, with contact details, summaries per role cluster, the full bank of tagged bullets, projects, education, certifications and skills. If it's filled in, use it by default and **don't ask the user to upload anything**.
   - **A resume uploaded by the user**: if the master is still the blank template (it contains placeholders like `[YOUR FULL NAME]`), ask the user to upload their current resume and work from that. When you're done, **offer to port their information into the master** so future optimizations are instant.
2. **The job description** — the text of the posting they're applying to.

If the job description is missing, **ask for it** — this skill optimizes *against a specific role*, and without it you can only do a generic ATS review (say so explicitly if the user prefers to continue without one).

## Workflow

Follow these steps in order. Don't skip step 1.

### Step 1 — Load the source resume

**Normal case (master filled in):** read all of `references/master-cv.md`. Pay attention to:
- The **role tags** on each bullet — pick the cluster that matches the posting using the "Quick map: role → what to include" section.
- The **per-cluster summaries** — start from the matching one and adjust 2–3 keywords to the posting's literal wording.
- The **"Integrity rules when deriving"** section at the end of the master — it is binding.
- The master's strategy notes (how to present location depending on the posting's work model, which public profiles to include only for technical roles, which extra training to add per role).

**Alternate case (user uploaded a resume):** extract the PDF text with `pdfplumber` (it preserves reading order better) or `pdftotext -layout`:

```python
import pdfplumber
with pdfplumber.open("/mnt/user-data/uploads/resume.pdf") as pdf:
    text = "\n".join(p.extract_text() or "" for p in pdf.pages)
print(text)
```

**Diagnose parseability while extracting** — note any signs the ATS will struggle:
- If `extract_text()` returns little or nothing → it's probably an **image-only PDF** (scanned, or exported from Canva/InDesign as an image). Worst case: the ATS reads zero text. Flag it as a critical issue in the report.
- If the text comes out scrambled or mixed across sections → there are probably **columns or tables** breaking reading order.
- Check whether contact info (email/phone) appears in the extracted text; if not, it may sit in a **page header/footer**, which many ATS ignore.
- If the uploaded resume belongs to the same candidate as the master and contradicts it, **the master wins** (it's the most recent curated version); mention the discrepancy in the report.

Read `references/ats-rules.md` for the full rule list and scoring system before analyzing.

### Step 2 — Analyze the resume and extract the posting's keywords

Run two analyses in parallel:

**a) Format/parseability audit** — score the resume against the rules in `references/ats-rules.md` (columns, tables, fonts, standard section headings, date format, contact in the body, etc.).

**b) Keyword gap analysis** — this is the heart of tailoring to a posting:
1. Pull from the job description the terms the ATS will look for: job title, hard skills, tools/technologies, certifications, methodologies and repeated exact phrases.
2. Compare against the source resume (the master holds MORE than fits in a resume: the job is to select, not just to add).
3. Sort each keyword into: **present**, **missing but the candidate genuinely has it** (add it using the posting's literal wording), or **missing and not applicable** (don't invent experience — respect this; the master lists the candidate's known gaps).
4. Acronym rule: spell out long form + short form on first use, e.g. "Project Management Professional (PMP)", because different ATS index different forms.

> **Integrity first:** never invent experience, degrees, dates or skills the candidate doesn't have. Optimizing means reordering, rewriting and surfacing what is already true using the posting's language — not fabricating. If the candidate doesn't meet a key keyword, report it as a real gap; don't slip it into the resume.

### Step 3 — Generate the optimized resume (parseable PDF)

Rewrite the resume applying the ATS rules and keywords. Use `assets/cv_template.html` as the base and convert it to PDF. The template is already designed to be ATS-safe (single column, system fonts, no layout tables, contact in the body).

Recommended method (produces a **selectable-text** PDF):

```bash
pip install weasyprint --break-system-packages -q
python -c "from weasyprint import HTML; HTML('resume.html').write_pdf('/mnt/user-data/outputs/Resume_Optimized.pdf')"
```

If `weasyprint` isn't available, use `reportlab` with `SimpleDocTemplate` (see the `pdf` skill) — it also produces selectable text. **Never** generate the PDF as an image.

**Mandatory verification** after generating: re-extract the text from the new PDF with `pdfplumber` and confirm it comes out clean and in order (name, contact, sections). This is the "copy-paste test": if you can't extract the text, neither can the ATS.

Name the file `First_Last_Resume.pdf` once you know the name (ATS and recruiters prefer it).

Non-negotiable rules for the generated resume (details in `references/ats-rules.md`):
- Single column, reverse-chronological order.
- Standard fonts (Arial, Calibri, Helvetica, Georgia), body 10–12pt.
- Standard section headings: *Professional Summary, Skills, Experience, Education, Certifications*.
- Contact details in the document body, never in a page header/footer.
- No layout tables, text boxes, columns, icons, graphics, skill-level bars or photo.
- Consistent date format (e.g. "Jan 2023 – Mar 2025").
- Quantified achievement bullets wherever possible ("Cut costs by 23%").

### Step 4 — Write the report

Deliver the report in chat (and optionally as a .md file if the user asks). Use **exactly** this structure:

```markdown
# ATS Optimization Report

## Scores
- **Original ATS score:** X/100
- **Optimized ATS score:** Y/100
- **Match with the posting:** before Z% → after W%

## Parseability issues found
(what was breaking the parser in the original resume and how it was fixed)

## Keyword analysis
- **Posting keywords already present:** ...
- **Keywords added** (ones the candidate genuinely meets): ...
- **Real gaps** (keywords the candidate does NOT meet): ...  ← honest, never invented

## Changes applied
(concrete bullets: restructuring, achievement rewrites, date formatting, etc.)

## Recommendations for the candidate
(actions only the person can take: earn a certification, quantify a missing result, etc.)

## Note on file format
The resume is delivered as a selectable-text PDF, which modern ATS (Workday, Greenhouse, Lever, iCIMS) parse well in 2026. If the posting or portal runs on a legacy system (older Taleo) or asks for "Word", offer to generate an equivalent .docx version.
```

Compute the scores with the rubric in `references/ats-rules.md` (don't invent numbers; derive every point from verifiable criteria).

### Step 5 — Deliver the files

Save the optimized resume to `/mnt/user-data/outputs/` and present it with `present_files`. If you produced the report as a file, present that too. Offer the .docx version as a next step if relevant.

## Core reminder

Two halves: **getting the ATS to read it** (parseability) and **getting the ATS to score it high** (posting keywords). Optimize both, with absolute integrity on content: surface what is true, never fabricate what isn't. When deriving from the master, the final resume must fit in **1–2 pages**: pick 4–6 bullets per job (the ones tagged for the relevant cluster) and only the relevant skill categories — the master is a bank, not a template to copy wholesale.

## Maintaining the master CV

`references/master-cv.md` is the source of truth and must be kept up to date. **On first use it ships as a blank template**: if you detect placeholders like `[YOUR FULL NAME]`, offer to fill it in from the user's current resume and hand them the completed `master-cv.md` to drop into the skill. If during a conversation the user mentions new experience, certifications, projects or corrections that aren't in the master:
1. Use them in that session's derived resume.
2. Tell them the master inside the skill is now out of date and offer to generate the updated `master-cv.md` (and the full `.skill` package) to replace it: the skill is updated by uploading the new package under **Settings → Capabilities → Skills** (or by saving the .skill file presented in chat).
