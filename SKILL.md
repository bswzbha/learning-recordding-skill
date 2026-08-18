---
name: learning-issue-ppt
description: Create or update PowerPoint learning issue records from GPT Q&A, debugging conversations, solved study problems, or technical troubleshooting notes. Use when the user wants a PPT/PPTX draft that summarizes a learning problem, uses screenshot-first overview slides, preserves screenshot placeholders, records multiple candidate causes and attempts, splits dense content across slides, lets the user choose the PPTX output path, maintains clickable outline/index slides, and adds a red bold Finish slide at the end of each issue.
---

# Learning Issue PPT

## Purpose

Turn a solved learning problem or GPT Q&A into a PowerPoint learning record. Optimize for later review, not live presentation: make screenshots the main memory cue, keep visible text very concise, and put detailed reasoning in speaker notes.

## Output Shape

Before creating a deck, ask or infer whether the user wants:

1. `New PPT`: create a new learning-record deck.
2. `Append to existing PPT`: open an existing learning-record deck, add the new issue, and update navigation.

Create each issue as a small slide group. Do not force one issue onto one slide.

Default slide group:

1. Issue overview
2. Cause analysis
3. Solution
4. Finish

Split slides when content is dense, screenshots are large, or code/config is too long. Typical expanded group:

1. Issue overview
2. Key screenshot/zoom placeholder
3. Cause analysis
4. Solution steps
5. Key code/config
6. Finish

## Slide Rules

- Use one main idea per slide.
- Keep visible text short and reviewable; screenshots should carry most of the recall burden.
- Put detailed explanation, discarded attempts, and expanded reasoning in speaker notes.
- Use Microsoft YaHei (`微软雅黑`) for all Chinese text.
- Use Times New Roman for all English text.
- For mixed Chinese/English text, apply fonts at the run level when the presentation tooling supports it; otherwise use Microsoft YaHei as the fallback because the deck is Chinese-first.
- Put the primary screenshot placeholder on the first issue overview slide under the title.
- Leave screenshot areas large so the user can paste screenshots manually.
- Use separate slides for large screenshots.
- Use additional screenshot slides for long screenshots, zoomed error regions, before/after screenshots, or multiple relevant UI states.
- Never shrink screenshots into unreadable thumbnails just to fit text.
- Use stable issue numbering such as `001`, `002`, `003`.
- For long-term decks, maintain a cover, table of contents, category index, and recent additions page when creating or updating the whole deck.

## New vs Append Mode

Use `New PPT` mode when the user wants a fresh record file. Create the navigation slides and start the first issue at `001`.

Use `Append to existing PPT` mode when the user wants to continue an existing record file.

Append mode requirements:

- Let the user choose the existing `.pptx` file when no path is provided.
- Inspect the existing deck before editing.
- Detect the highest existing issue number from the table of contents and issue footers.
- Number the new issue as the next sequential ID, such as `002`, `003`, `004`.
- Insert the new issue slide group immediately after the previous issue's `Finish` slide.
- Keep the previous issue's `Finish` slide unchanged.
- Update the table of contents with the new issue entry.
- Add or refresh the table-of-contents hyperlink so the new entry jumps to the new issue overview / problem-description screenshot slide.
- Ensure every concrete slide in the new issue group links back to the table of contents.
- Preserve existing issues and existing screenshots.
- Save as an updated copy unless the user explicitly asks to overwrite the existing PPTX.

## Issue Overview Slide

The first slide of every issue must be screenshot-first.

Required layout:

1. Top: issue title
2. Middle: large screenshot placeholder
3. Bottom: minimal phenomenon and conclusion text
4. Footer: compact issue navigation label linked back to the table of contents

Title requirements:

- Font size: 36 pt
- Weight: bold
- Color: red
- Content: the actual problem title, for example `Fluent 读取 .msh 后无反应并报 invalid grid`

Do not add a top-left label such as `学习问题记录`.

Bottom text requirements:

- Keep text under the screenshot, not above it.
- Include only the shortest useful `Phenomenon` and `Conclusion`.
- Use 1-3 short lines total when possible.
- Do not use large text cards on the overview slide unless the screenshot area still remains dominant.

Example:

```text
Phenomenon: Importing Case1.msh shows no UI change; Check reports invalid grid.
Conclusion: The file opens, but it is not a valid Fluent fluid-domain mesh.
```

Avoid long background paragraphs on the overview slide. Move details to later slides or speaker notes.

## Screenshot Slides

Use screenshot placeholders only. Do not generate images.

Recommended visible text:

```text
Paste problem screenshot here
```

For multiple screenshots, name the placeholder by purpose:

```text
Paste original error screenshot here
Paste key error region screenshot here
Paste fixed result screenshot here
```

If there is only one main screenshot, place it on the first issue overview slide. Create extra screenshot slides only for zoomed error regions, long screenshots, before/after evidence, or multiple important UI states.

## Cause Analysis Slide

Use a candidate-cause format. Show only the concise version on the slide.

Example visible slide content:

```text
A. API function recursion             Ruled out
B. State update triggers re-render    Partly related
C. Object dependency changes          Final cause
```

For each candidate cause, put details in speaker notes:

```text
Candidate A: API function recursion
Evidence:
Attempt:
Result:
Conclusion:
```

Allowed conclusions:

- `Ruled out`
- `Partly related`
- `Final cause`
- `Unverified`

If the conversation contains only the final cause, create one candidate and mark it `Final cause`.

## Solution Slide

Include:

- Final working steps
- Critical command, code, configuration, or setting
- What changed from the failed attempts
- A screenshot placeholder below the concise solution text for evidence from the solving process

Use a separate slide for long code or configuration. Keep code snippets short enough to read.
Keep solution text near the top and reserve the lower half for a large `Paste solution process screenshot here` placeholder when the user may want to paste proof, command output, fixed UI state, or comparison screenshots.

## Finish Slide

Every issue must end with a dedicated Finish slide.

Required format:

- Background: pure white
- Text: `Finish`
- Font size: 48 pt
- Weight: bold
- Color: red
- Position: centered horizontally and vertically
- Other elements: none

Do not add title, footer, navigation, issue number, page number, notes, or decoration to the Finish slide.

## Long-Term Deck Navigation

When creating or updating a deck containing one or more issues, maintain a clickable outline.

Required navigation slides:

- Table of contents / outline page
- Category index by topic/technology when there are multiple topics
- Recent additions page when updating a long-term deck

The table of contents must exist even when the deck has only one issue.

Table of contents format:

```text
001 useEffect repeats        002 git push rejected        003 JOIN duplicates
004 npm install fails        005 env not loaded           006 CSS overflow
007 Python import error      008 Vite blank page          009 Fluent invalid grid
```

Table-of-contents row rules:

- Use a three-column entry grid across the slide so one page can hold more issues.
- Each entry contains only issue number plus concise title, such as `001 .msh invalid grid`.
- Do not split each entry into metadata columns.
- Keep each title short; prefer a compressed review title over the full issue title.
- Use small row text; 14-18 pt is preferred.
- Do not repeat the full long problem title in the table of contents.
- Do not include category, topic, status, or other metadata columns on the table of contents slide.

Hyperlink requirements:

- Each issue entry in the table of contents must hyperlink to that issue's overview / problem-description screenshot slide.
- Apply the hyperlink to the visible issue-entry text itself, not only to a surrounding rectangle or background shape.
- Each concrete issue slide, except the Finish slide, must include a compact footer label such as `Issue 001 | Fluent / ICEM mesh import`.
- Every footer issue label on concrete issue slides must hyperlink back to the table of contents / outline slide.
- Apply the return hyperlink to the visible footer label text itself, not only to the text box boundary.
- In append mode, refresh both directions: table of contents to issue overview, and issue slides back to table of contents.
- The hyperlink target must be the actual table-of-contents slide, not a placeholder or missing slide.
- Verify links after generation when the presentation tooling supports link inspection.

Each issue should start after the previous issue's Finish slide.

Do not list Finish slides as separate issues in the table of contents.

## Output Path

Before creating a PPTX, determine the final output path with a file picker when possible.

Priority:

1. Use the exact output path provided by the user.
2. If the user provides only a directory, create a clear filename in that directory.
3. If the user does not provide a path, open a system save-file dialog so the user can choose the folder and filename.
4. If a system dialog is unavailable, blocked, or running in a non-GUI environment, ask the user to type the full output path.
5. Use a default output location only when the user explicitly says to use the default or does not care.

File picker requirements:

- Use a save-file dialog, not an open-file dialog.
- Filter or default the extension to `.pptx`.
- Suggest a clear default filename before showing the dialog.
- Treat canceling the dialog as "no path chosen" and ask the user for the path manually.
- Do not create the PPTX until a final path has been chosen.

Recommended filename format:

```text
learning-issue-YYYY-MM-DD-topic.pptx
```

For monthly long-term decks:

```text
YYYY-MM-learning-issues.pptx
```

Never silently save the final PPTX to a temporary directory when the user expects to choose a location.

## Writing Style

- Write in the user's language unless they request otherwise.
- Prefer concise Chinese when the input is Chinese.
- Preserve important English technical terms such as `useEffect`, `dependency array`, `git push`, and error messages.
- Remove filler, repetition, and conversational back-and-forth.
- Keep drafts editable; mark uncertain facts as `待补充` or `Unverified`.

## PowerPoint Creation

When generating an actual PPTX, use the Presentations skill/workflow available in the environment. Follow its rendering and validation requirements. This skill defines the learning-record structure; the presentation authoring skill handles PPTX creation, rendering, and overflow checks.

## Skill Maintenance Preview

When this skill is modified, create or update a preview PPTX template so the user can inspect the current layout quickly.

Required preview file:

```text
templates/learning-issue-preview-template.pptx
```

Preview template requirements:

- Include a table-of-contents slide.
- Use a three-column entry grid on the table-of-contents slide.
- Each entry shows only issue number plus concise title.
- Show at least two issue entries so append behavior is visible.
- Place issue `002` immediately after issue `001`'s Finish slide.
- Make each table-of-contents entry hyperlink to that issue's overview / problem-description screenshot slide.
- Make each concrete issue slide footer hyperlink back to the table of contents / outline slide.
- Use Microsoft YaHei (`微软雅黑`) for Chinese text and Times New Roman for English text.
- Keep table-of-contents row text at 14-18 pt.
- Use a concise title in the table of contents, not the full problem title.
- Do not include a category/status column on the table-of-contents slide.
- Include representative issues using the screenshot-first overview layout.
- Show the title at 36 pt, bold, red.
- Put the main screenshot placeholder directly below the title.
- Put only minimal phenomenon and conclusion text below the screenshot placeholder.
- Include cause analysis, solution, and Finish slides.
- On the solution slide, place concise solution text above a solution-process screenshot placeholder.
- Do not create a separate lessons learned / experience summary slide.
- Use a red bold 48 pt centered `Finish` slide.
- Avoid adding the `学习问题记录` label.
- Render the preview deck after creation and inspect for clipping, overlap, unreadable text, and missing layout elements.

Use this preview PPTX as the visual acceptance fixture for future skill edits.
