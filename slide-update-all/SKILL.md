---
name: slide-update-all
description: Manually-triggered task to update lecture slide decks across all courses listed in RESEARCH_TASKS.md, applying each deck's course- and module-level INSTRUCTIONS and the global STYLE_GUIDE, driven by the reviewed RESEARCH reports
---

Manually-triggered task that UPDATES lecture slide decks across ALL courses. Run this AFTER the human has reviewed and revised the RESEARCH_*.md reports produced by the `slide-research-all` task. This task DOES modify .pptx files. Use the pptx skill for all slide editing. It is course-agnostic: it works for any course (cs6264, cs6035, cs6262, etc.) by deriving each deck's course and module from its path in the manifest — nothing about a specific course is hardcoded.

## Step 0 — Connect the working folder (do this FIRST, before anything else)

Runs only mount this task's own folder, NOT the data folder where the decks and manifest live. Before doing anything else, verify you can read:
   /Users/wenkelee/Documents/Claude/Scheduled/slide-updater/courses/RESEARCH_TASKS.md
If it is not reachable, use the `request_cowork_directory` tool to connect:
   /Users/wenkelee/Documents/Claude/Scheduled/slide-updater
Then continue. Do NOT proceed to the steps below until the courses root is readable.

## Inputs

- **Courses root**: /Users/wenkelee/Documents/Claude/Scheduled/slide-updater/courses
- **Manifest** (authoritative deck list): <courses root>/RESEARCH_TASKS.md
- **Global style guide**: <courses root>/STYLE_GUIDE.md

All paths in the manifest's Path and Report columns are relative to the courses root.

## Step 1 — Load the global style guide

Read <courses root>/STYLE_GUIDE.md. This governs how every slide looks (fonts, colors, title formatting, backgrounds, layout, footer, and the Speaker Notes tone + TTS-compatibility rules). It applies to ALL decks in ALL courses and always wins over any per-slide or per-course formatting. If a course folder contains its own STYLE_GUIDE.md, treat that as supplementary, but the global one is the baseline.

## Step 2 — Read the manifest

Read <courses root>/RESEARCH_TASKS.md and use its table as the AUTHORITATIVE list of decks to update. Each row gives the deck name, the exact .pptx path, and the path to its research report. ONLY operate on the exact .pptx paths listed in the manifest. The course folders contain many stray files (backups, `~$` lock files, `.corrupted`, `copy`, `Test`, `choose.pptx`, `__*savetest`, `- Notes` variants) — NEVER touch any file that is not the exact path named in the manifest.

## Step 3 — Per-deck workflow

Process the decks in the order they appear in the manifest. For EACH manifest row, derive the deck's location from its Path and load the right instructions for THAT deck — do not assume any particular course:

- **Deck path** = <courses root>/<Path from manifest>.
- **Module folder** = the directory that directly contains the .pptx (e.g. for "cs6264/Module 6 - Web Security/L13 ....pptx" the module folder is "cs6264/Module 6 - Web Security"; for "cs6035/Module 2 - .../L3 ....pptx" it is "cs6035/Module 2 - ...").
- **Course folder** = the top-level course directory under courses (e.g. "cs6264", "cs6035", "cs6262") — typically the first path segment of the deck Path.

Then, for each deck:

a. **Load course-level instructions** — Read <course folder>/INSTRUCTIONS.md. This governs content decisions for that whole course (revision priorities, tone, things to avoid). If this file is missing, skip the deck and note it in the changelog and summary (do not guess content rules for a course that has no instructions).

b. **Load module-level instructions (optional)** — If <module folder>/INSTRUCTIONS.md exists, it OVERRIDES or SUPPLEMENTS the course-level instructions for this deck. Ignore any `INSTRUCTIONS copy.md` — that is a backup. If no module-level file exists, just use the course-level instructions.

c. **Load the reviewed research report** — Read the report named in the manifest's Report column for this deck. The human has already reviewed and revised it; treat its "Suggested Slide Updates", "Suggested New Slides", and "Outdated Content" sections as the approved plan for what to change. Do not re-research from scratch; this report is the source of new content. You may do light verification searches only if a specific claim looks wrong.

d. **Read current slide content** — Run: python -m markitdown "<absolute deck path>" to understand what's already there before editing.

e. **Revise content** — Apply the approved updates from the research report. Integrate new material into the existing narrative rather than appending disconnected facts. Preserve foundational concepts. Don't add offensive techniques without defensive countermeasures, and don't put full exploit code on slides.

   - **Two slides per new topic** — For each new topic or finding the report introduces, add a summary slide (title plus the bullet points/summary), AND a second slide that expands on the single most important point or bullet, ideally built around a figure or diagram drawn from the source. If the source figure cannot be reproduced or reused, recreate an equivalent diagram in the deck's style rather than dropping the expansion slide.
   - **Handle outdated content per the report's recommended action** — If the report says a slide is too old and should be removed, remove it. If the report says an old slide should be kept but updated, update it in place with the new bullets/summary and/or figure; where the report suggests reproducing the slide with updated contents, regenerate the slide rather than only patching text. Mirror each of these decisions in the changelog (Step 4) so it is clear which slides were removed, which were updated, and which were reproduced.

f. **Reformat overcrowded slides** — If a slide has too much content, or two figures stacked one above the other (or side by side), split it into two slides. If text is too small to read, reformat or regenerate the slide for legibility. **Split the speaker notes along with the content:** when one slide becomes two or more, divide its original notes so each resulting slide's notes cover exactly the content now on that slide. Keep the spoken narrative continuous across the split (the second slide's notes pick up where the first left off), do not duplicate the same notes on both slides, and do not leave the entire original note on only one of them. Each resulting slide must still satisfy the notes rules in step (g), including explaining any figure that ended up on it.

g. **Revise speaker notes** — For every slide (existing or new), ensure the Notes section has substantive notes. Notes are a script the instructor reads aloud, written as spoken explanations TO students in first person ("Let me walk you through...", "Notice that..."), NEVER meta-instructions ("Tell students..."). For every NEW slide — and especially any slide that contains a new figure or diagram — the notes must explain what the slide and the figure/diagram show, in spoken form. The caption under a figure or diagram in the source is often an excellent ready-made summary; adapt it into the notes (rephrasing into spoken English) rather than leaving the figure unexplained. Notes must follow ALL the TTS-compatibility rules in STYLE_GUIDE.md: natural dates ("May 19, 2026" not ISO), no mid-sentence parenthetical citations, space-separated CVE IDs, severity words instead of CVSS numbers, expanded abbreviations, URLs only in a trailing "Sources:" section, sentences under 30 words, symbols written out.

h. **Apply formatting** — Enforce STYLE_GUIDE.md on every slide of the deck: fonts, colors, title position/formatting, backgrounds per slide type, margins, line spacing, max bullets per slide. Remove any existing course/module-name or slide-number footers (the style guide specifies no footers). This applies to ALL slides even if it means restyling or regenerating existing ones.

i. **QA** — Convert the finished deck to images and visually inspect for overlapping elements, cut-off text, unreadable contrast, or style inconsistencies, and fix what you find. Verify that printing with the Notes option includes both slide content and notes.

## Step 4 — Change log and summary

Write/update <courses root>/CHANGELOG.md summarizing, per deck (grouped by course): which slides were added/revised/removed, what research sources were used, and any deck skipped and why. Then print a short summary of which decks were updated, grouped by course.

## Constraints

- ONLY modify the exact .pptx paths listed in the manifest. Never edit, rename, or delete backup/copy/corrupted/test/lock files.
- Do NOT modify the manifest, any INSTRUCTIONS.md, any STYLE_GUIDE.md, or the RESEARCH_*.md reports.
- If a deck's .pptx, its course-level INSTRUCTIONS.md, or its research report is missing, skip that deck and note it in the changelog and summary.
- The global STYLE_GUIDE.md always wins over any per-slide formatting or per-course preference.
- Never assume a course is cs6264 (or any specific course); always derive the course and module from the deck's path in the manifest.