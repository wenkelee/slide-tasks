---
name: slide-research-all
description: RETIRED — replaced by slide-research-cs6264. Safe to delete from the Scheduled tab.
---

Manifest-driven research task for all CS 6264 lecture decks. This task is fast and lightweight — it does NOT modify any PPTX files. It only gathers research and writes findings reports.

## Step 0 — Connect the working folder (do this FIRST, before anything else)

Scheduled runs only mount this task's own folder, NOT the data folder where the decks and manifest live. Before doing anything else, verify you can read:
   /Users/wenkelee/Documents/Claude/Courses/courses/RESEARCH_TASKS.md
If it is not reachable, use the `request_cowork_directory` tool to connect:
   /Users/wenkelee/Documents/Claude/Courses
Then continue. Do NOT proceed to the steps below until the manifest is readable.

## Manifest selection

This task is manifest-driven and manifest-agnostic. By default it uses the all-courses manifest `RESEARCH_TASKS.md`. **If the task prompt names a specific manifest file** (for example `RESEARCH_TASKS_cs6035.md`, the cs6035-only manifest), use THAT file instead, and research only the decks it lists. Everything else below is identical regardless of which manifest is used; "the manifest" means whichever file was selected. The reports each manifest names already point into the right course folder, so scoping happens entirely through the chosen manifest.

## How it works

1. Read the manifest file (the default `RESEARCH_TASKS.md`, or the specific manifest named in the task prompt):
   /Users/wenkelee/Documents/Claude/Courses/courses/RESEARCH_TASKS.md
   This file contains a table of the decks that need weekly research. Each row has:
   - Deck name (e.g., L1, L2, … L13, L14, L15, L16, L17)
   - Path to the .pptx file (relative to the courses root, which is /Users/wenkelee/Documents/Claude/Courses/courses)
   - Path to the research report output (relative to the courses root)
   - Topics to research
2. For EACH deck listed in the manifest, do the following:
   a. Read the current slide content using:
      python -m markitdown "/Users/wenkelee/Documents/Claude/Courses/courses/<Path>"
   b. Search the web for developments from the past 6–12 months on the topics listed in the manifest for that deck. Prioritize:
      - Peer-reviewed papers and official documentation
      - Material from the last 6 months (flag anything older than 12 months)
      - Findings relevant to instruction — a breakthrough is only worth adding if it helps students
      - Cross-reference claims across multiple sources
   c. Write a research report to:
      /Users/wenkelee/Documents/Claude/Courses/courses/<Report>
      The report must include:
      - **Date**: today's date
      - **New Findings**: bullet list of the most important developments, each with a 2–3 sentence summary and a source URL
      - **Suggested Slide Updates**: which existing slides should be revised and what to add or change. For EACH item, name the specific slide (by title and/or number) it affects, so the update task can find it.
      - **Per-Slide Notes Additions**: for every finding that is relevant to a specific EXISTING slide, write a ready-to-paste **notes addition** for that slide — a short spoken-English paragraph (2–4 sentences) the instructor could read aloud, that folds the new/updated material into how that slide is presented. These must already follow the STYLE_GUIDE TTS rules (natural spoken dates, no mid-sentence parenthetical citations, space-separated CVE IDs, severity words not CVSS numbers, expanded abbreviations, sentences under 30 words) and must end with a "Sources:" line listing the URL(s). Key each addition to its slide (title and/or number). The goal is that the update task can drop these straight into the right slides' Notes, so existing slides — not just new ones — get expanded with the new material.
      - **Suggested New Slides**: any new slides that should be added, with a title and 3–5 bullet points of content. For each new topic or finding, suggest TWO slides, not one: (1) a summary slide with the title and 3–5 bullet points, and (2) a follow-up slide that expands on the single most important point or bullet in more depth — ideally built around a figure or diagram taken from the source. Name the specific figure/diagram and its source, and quote the source's caption, so the update task can reproduce it and use the caption as a notes summary.
      - **Outdated Content**: anything in the current deck that is now incorrect or stale. Be explicit about the recommended action for each item:
        - If a slide is too old and should be REMOVED entirely, say so clearly and give the reason.
        - If an old slide can be KEPT but needs updating (new bullet points, a revised summary, and/or a new figure/diagram), say so clearly, list exactly what should change, and — where possible — suggest reproducing the slide with the updated contents (new bullets and/or replacement figure) rather than only describing the edit.
3. After processing all decks, print a summary of which reports were updated.

## Important constraints

- Do NOT touch any .pptx files. Do NOT run LibreOffice, pptxgenjs, or any image conversion tools.
- Do NOT modify the manifest file itself.
- If a .pptx file listed in the manifest does not exist, skip it and note it in the summary.
- Process decks in the order they appear in the manifest.
- Each research report is independent — do not cross-reference between decks unless the manifest topics overlap.