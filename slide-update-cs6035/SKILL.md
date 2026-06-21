---
name: slide-update-cs6035
description: Manually-triggered task to update the CS 6035 lecture decks from reviewed research reports. Follows the canonical slide-update-cs6264 procedure, scoped to the cs6035 manifest. Modifies .pptx files.
---

Manually-triggered task that UPDATES the CS 6035 lecture decks from reviewed research reports. This task DOES modify .pptx files. Run it AFTER the human has reviewed and revised the cs6035 RESEARCH_*.md reports produced by the slide-research-cs6035 task.

STEP 0 — Connect the working folder first. Verify you can read:
  /Users/wenkelee/Documents/Claude/Courses/courses/RESEARCH_TASKS_cs6035.md
If it is not reachable, use the request_cowork_directory tool to connect:
  /Users/wenkelee/Documents/Claude/Courses
Do not proceed until that manifest is readable.

STEP 1 — Load the procedure. Read the canonical slide-update procedure at:
  /Users/wenkelee/Documents/Claude/Scheduled/slide-update-cs6264/SKILL.md
Follow it EXACTLY, with ONE scoping override: use the CS 6035 manifest
  /Users/wenkelee/Documents/Claude/Courses/courses/RESEARCH_TASKS_cs6035.md
instead of the default RESEARCH_TASKS_cs6264.md. Update ONLY the decks listed in the cs6035 manifest.

Notes specific to cs6035:
- The cs6035 decks live in a single flat directory (no per-module subfolders), so for each deck the "module folder" is the course folder itself (cs6035). There is one INSTRUCTIONS.md at /Users/wenkelee/Documents/Claude/Courses/courses/cs6035/INSTRUCTIONS.md that plays both the course-level and module-level role; the procedure's "module-level instructions" step simply resolves to that same file, which is expected.
- Everything else — loading STYLE_GUIDE.md, reading each deck's reviewed report, revising content and speaker notes per the TTS rules, reformatting overcrowded slides, applying formatting, QA, and writing the CHANGELOG — is governed entirely by the canonical procedure. Do not duplicate or override it here beyond the manifest scoping above.

After processing, print a summary of which cs6035 decks were updated and list any skipped (missing .pptx, missing report, etc.) and why.