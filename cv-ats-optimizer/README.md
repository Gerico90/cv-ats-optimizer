# ATS Resume Optimizer — a Claude Skill

A Claude Skill that rewrites your resume against a **specific job posting** so it survives ATS parsing and scores high on keyword match, then hands you a selectable-text PDF plus a report with before/after scores.

Available in English and Spanish.

## Why

Most resume advice optimizes for the human reader. An ATS is not a human reader: it flattens your two-column layout, skips your text boxes, ignores your header, and scores you on literal keyword overlap with the posting. A great resume can be dropped before anyone sees it.

This skill fixes both halves of the problem:

1. **Parseability** — single column, system fonts, contact in the body, no tables/icons/skill bars, selectable text (never an image PDF).
2. **Keyword match** — pulls the posting's literal terms and maps them against what you actually have.

It is built on one idea: **your master CV is a bank, not a resume.** You write down everything you've ever done, once. Each application pulls a different 1–2 page subset out of it.

## Integrity

The skill never invents experience, dates, proficiency levels or tools. Optimizing means reordering, rewriting and surfacing what is already true in the posting's language. Keywords you don't actually meet get reported as real gaps instead of quietly added to the resume — which is what stops you from acing the filter and then failing the interview.

## Install

1. Download `dist/cv-ats-optimizer-en.skill` (or `-es` for Spanish).
2. In Claude: **Settings → Capabilities → Skills** → upload the file.
3. Enable code execution / file creation — the PDF is generated with Python.
4. New chat: paste a job posting and say "optimize my resume for this". It triggers on its own.

Full walkthrough, including how to fill in your master CV: **[SETUP.md](SETUP.md)**.

## What's in here

```
en/skill/     English version (source files)
es/skill/     Spanish version (source files)
dist/         packaged .skill files, ready to upload
SETUP.md      setup and how to fill in your master CV
```

To edit and repackage: change the files under `en/skill/`, then zip the folder and rename it to `.skill`.

## Privacy note

`master-cv.md` ships **blank on purpose**. Once you fill it in, that file holds your phone number, email, full employment history and a written list of your professional weak spots. It stops being a shareable template at that moment. If you pass this skill on to someone, strip the master back to placeholders first.

## License

MIT — see [LICENSE](LICENSE). Use it, fork it, adapt it commercially. Just keep the attribution notice.
