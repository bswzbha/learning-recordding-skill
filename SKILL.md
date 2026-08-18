---
name: learning-issue-ppt
description: Create or update PowerPoint learning issue records from GPT Q&A, debugging conversations, solved study problems, or technical troubleshooting notes. Use when the user wants a PPT/PPTX draft that summarizes a learning problem, uses screenshot-first overview slides, preserves screenshot placeholders, records multiple candidate causes and attempts, splits dense content across slides, lets the user choose the PPTX output path, maintains clickable outline/index slides, and adds a red bold Finish slide at the end of each issue.
---

# Learning Issue PPT

## Purpose

Turn a solved learning problem or GPT Q&A into a PowerPoint learning record. Optimize for later review, not live presentation: make screenshots the main memory cue, keep visible text very concise, and put detailed reasoning in speaker notes.

## Output Shape

Create one issue as a small slide group. Do not force one issue onto one slide.

Default slide group:

1. Issue overview
2. Cause analysis
3. Solution
4. Lessons learned
5. Finish

Split slides when content is dense, screenshots are large, or code/config is too long. Typical expanded group:

1. Issue overview
2. Key screenshot/zoom placeholder
3. Cause analysis
4. Solution steps
5. Key code/config
6. Lessons learned
7. Finish

## Slide Rules

- Use one main idea per slide.
- Keep visible text short and reviewable; screenshots should carry most of the recall burden.
- Put detailed explanation, discarded attempts, and expanded reasoning in speaker notes.
- Put the primary screenshot placeholder on the first issue overview slide under the title.
- Leave screenshot areas large so the user can paste screenshots manually.
- Use separate slides for large screenshots.
- Use additional screenshot slides for long screenshots, zoomed error regions, before/after screenshots, or multiple relevant UI states.
- Never shrink screenshots into unreadable thumbnails just to fit text.
- Use stable issue numbering such as `001`, `002`, `003`.
- For long-term decks, maintain a cover, table of contents, category index, and recent additions page when creating or updating the whole deck.

## Issue Overview Slide

The first slide of every issue must be screenshot-first.

Required layout:

1. Top: issue title
2. Middle: large screenshot placeholder
3. Bottom: minimal phenomenon and conclusion text
4. Footer: compact issue navigation label linked back to the table of contents

Title requirements:

- Font size: 48 pt
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

Use a separate slide for long code or configuration. Keep code snippets short enough to read.

## Lessons Slide

Include:

- 1-3 reusable lessons
- Next debugging checklist
- One short prevention tip when applicable

Example:

```text
Lessons
- useEffect compares dependency references.
- Objects, arrays, and functions can retrigger effects.
- Check dependency stability before rewriting the API call.

Next time
- Inspect the dependency array.
- Log which state update triggers re-render.
```

## Finish Slide

Every issue must end with a dedicated Finish slide.

Required format:

- Background: pure white
- Text: `Finish`
- Font size: 72 pt
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
001 useEffect repeated API requests       React       Solved
002 Git push rejected                     Git         Solved
003 SQL JOIN duplicate rows               SQL         Draft
```

Hyperlink requirements:

- Each issue row in the table of contents must link to that issue's overview slide.
- Each issue overview slide must include a compact footer label such as `Issue 001 | Fluent / ICEM mesh import`.
- The footer issue label must hyperlink back to the table of contents.
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
