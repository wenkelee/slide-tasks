---
name: slide-research-cs6035
description: Weekly research digest for all CS 6035 lecture decks, scoped to the RESEARCH_TASKS_cs6035.md manifest. Research only — does not modify any PPTX files.
---

Run the weekly CS 6035 slide research. This task ONLY gathers research and writes findings reports. It does NOT modify any .pptx files, run LibreOffice, or convert images.

STEP 0 — Connect the working folder first. Verify you can read:
  /Users/wenkelee/Documents/Claude/Courses/courses/RESEARCH_TASKS_cs6035.md
If it is not reachable, use the request_cowork_directory tool to connect:
  /Users/wenkelee/Documents/Claude/Courses
Do not proceed until that manifest is readable.

STEP 1 — Load the procedure. Read the canonical research procedure at:
  /Users/wenkelee/Documents/Claude/Scheduled/slide-research-cs6264/SKILL.md
Follow it exactly, with ONE scoping override: use the CS 6035 manifest
  /Users/wenkelee/Documents/Claude/Courses/courses/RESEARCH_TASKS_cs6035.md
instead of the default RESEARCH_TASKS_cs6264.md. Research ONLY the decks listed in the cs6035 manifest.

STEP 2 — For context on each deck, also read:
  /Users/wenkelee/Documents/Claude/Courses/courses/cs6035/INSTRUCTIONS.md
Use each deck's "Summary" and "Research focus" there to steer the research toward what that deck actually needs.

For EACH deck row in the cs6035 manifest:
  a. Read the current slide content with: python -m markitdown "<courses>/<Path>"  (paths in the manifest are relative to the courses root, which is /Users/wenkelee/Documents/Claude/Courses/courses). If a deck file cannot be opened because it is locked or open in PowerPoint, skip it and note it in the summary.
  b. Search the web for developments from the past 6-12 months on that deck's Topics. Prioritize peer-reviewed papers and official documentation, material from the last 6 months (flag anything older than 12 months), findings that actually help instruction, and cross-reference claims across multiple sources.
  c. Write a research report to the manifest's Report path (e.g. cs6035/RESEARCH_1.1.md, relative to /Users/wenkelee/Documents/Claude/Courses/courses). Each report must include: Date (today, in natural spoken form); New Findings (bulleted, each with a 2-3 sentence summary and a source URL); Suggested Slide Updates (which slides to revise and what to add/change); Suggested New Slides (for each new topic, TWO slides — a summary slide with title and 3-5 bullets, and a follow-up slide expanding the single most important point, ideally around a named figure/diagram from the source with its caption quoted); and Outdated Content (anything now incorrect or stale, with an explicit recommended action: REMOVE entirely, or KEEP-but-UPDATE with exactly what should change).

CONSTRAINTS: Do NOT touch any .pptx files. Do NOT modify the manifest or any INSTRUCTIONS.md or STYLE_GUIDE.md. Process decks in manifest order. If a deck's .pptx is missing or locked, skip it and note it. After processing, print a summary of which reports were written or updated, and list any decks skipped and why.

NOTE: This is research only. Applying the reviewed reports to the actual slides remains a separate, manually-triggered step (the slide-update-cs6035 task), so you review the findings before any slide is changed.