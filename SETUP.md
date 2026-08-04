# ATS Resume Optimizer — setup guide

## What this skill does

You give Claude a job posting. It rewrites your resume to (a) survive the ATS parser and (b) score high against that specific posting's keywords, then hands you a selectable-text PDF plus a report showing before/after scores, which keywords were added, and which gaps are real and can't be papered over.

It is built around one idea: **your master CV is a bank, not a resume.** You write down everything you've ever done, once. Each application pulls a different 1–2 page subset out of it.

## Install

1. Save the `cv-ats-optimizer.skill` file.
2. In Claude, go to **Settings → Capabilities → Skills** and upload it.
3. Make sure **code execution / file creation** is enabled in your settings — the skill generates the PDF with Python, so it won't work without it.
4. To confirm it loaded, start a new chat and paste a job posting with "optimize my resume for this". The skill should trigger on its own; you don't need to name it.

## What's in the package

```
cv-ats-optimizer/
├── SKILL.md                      the workflow Claude follows
├── references/
│   ├── ats-rules.md              parseability rules + the 0–100 scoring rubric
│   └── master-cv.md              ← BLANK. This is the part you fill in.
└── assets/
    └── cv_template.html          ATS-safe single-column layout used to build the PDF
```

Only `master-cv.md` needs your input. Everything else works as shipped.

## Two ways to start

**Quick start (5 minutes).** Skip the master entirely. Upload your current resume + paste a job posting, and the skill works from the uploaded file. Fine for a one-off. The catch is you repeat the upload every time, and the output can only reuse what your current resume already says.

**Full setup (60–90 minutes, worth it if you're applying to more than a handful of roles).** Fill in `master-cv.md`. After that, every future application is just "here's a posting" and you get a tailored resume in one turn.

Easiest path to the full setup: do a quick start first, and at the end ask Claude to *"turn everything we've covered into a filled-in master-cv.md"*. It'll draft it from your uploaded resume and you edit from there.

## How to fill in master-cv.md

### 1. Define your role clusters
The biggest design decision. A cluster is a job family you actually apply to — the roles that would want a meaningfully different version of your resume. Most people have 2–4. Give each a short tag (`[DA]`, `[PM]`, `[UX]`, whatever fits your field).

If you only ever apply to one kind of role, use a single cluster and ignore the tagging system.

### 2. Write one headline + summary per cluster
3–4 lines each: years of experience, two or three quantified wins, the key tools that cluster's postings ask for. The skill starts from the matching summary and swaps 2–3 keywords to the posting's literal wording.

### 3. Build the bullet bank
This is where the real work is, and where most people under-deliver. Under each job, write **every** achievement — aim for 8–15 bullets per role, far more than fits in a resume. That surplus is the whole point: it gives the skill room to select instead of inventing.

For each bullet:
- Start with an action verb (Led, Built, Cut, Automated).
- Quantify it. Percentages, dollars, volumes, headcount, time saved. If you don't have the number, estimate a defensible one and note that it's an estimate.
- Tag it with the clusters it serves: `Rebuilt the reporting pipeline, cutting delivery from 2 weeks to 3 days [DA][DE]`.

### 4. Fill the skills bank
Group by category (technical, tools/platforms, methodologies, languages). List things explicitly and by name — ATS keyword matching is literal, so "Power BI" earns a match and "business intelligence tools" doesn't.

### 5. Write your real gaps — honestly
At the bottom, under integrity rules, list what you *don't* have: the tools you've only touched once, the certification you don't hold, the language level you can't back up. This is the most useful section in the file. It's what stops the skill from quietly inflating you into a candidate who fails the interview, and it makes the report tell you which postings you should skip and which skill to go learn.

### 6. Add your strategy notes
How to present your location for remote vs onsite postings, which public profiles (GitHub, portfolio, Dribbble) to include only for technical roles, which optional courses to surface per cluster.

## What's deliberately not included

- **No .docx output.** The skill produces PDF. If a posting demands Word, ask Claude for a .docx version of the same content — it can do it, it's just not part of the default flow.
- **No cover letters.** Different job.
- **The scores are a rubric, not a real ATS.** They're computed from verifiable criteria (is the text extractable, is it single-column, what share of keywords match), which makes before/after comparable and honest — but no tool has access to a real Workday scoring engine. Treat the number as a checklist result, not a prophecy.
- **The rules are US/Western-market oriented.** No photo, no date of birth, no marital status. If you're applying somewhere with different resume conventions (Germany, Japan), adjust `ats-rules.md`.
- **Updating the master is manual.** Skills are read-only at runtime, so Claude can't write back into the file. When your master goes stale, ask Claude to generate the updated `master-cv.md` and re-upload the skill package.

## Privacy note

The shipped `master-cv.md` is blank on purpose. Once you fill it in, that file holds your phone number, email, full employment history and a written list of your professional weak spots. It stops being a shareable template at that moment. If you ever pass this skill on to someone else, strip the master back to placeholders first.
