---
name: learning-issue-ppt
description: Create or update PowerPoint learning issue records from GPT Q&A, debugging conversations, solved study problems, or technical troubleshooting notes. Use when the user wants a PPT/PPTX draft that summarizes a learning problem, preserves screenshot placeholders, records multiple candidate causes and attempts, splits dense content across slides, lets the user choose the PPTX output path, maintains navigation/index slides for long-term decks, and adds a red bold Finish slide at the end of each issue.
---

# Learning Issue PPT

## Purpose

Turn a solved learning problem or GPT Q&A into a PowerPoint learning record. Optimize for later review, not live presentation: keep visible slide text concise, reserve clear space for pasted screenshots, and put detailed reasoning in speaker notes.

## Output Shape

Create one issue as a small slide group. Do not force one issue onto one slide.

Default slide group:

1. Issue overview
2. Screenshot placeholder
3. Cause analysis
4. Solution
5. Lessons learned
6. Finish

Split slides when content is dense, screenshots are large, or code/config is too long. Typical expanded group:

1. Issue overview
2. Original screenshot placeholder
3. Key screenshot/zoom placeholder
4. Cause analysis
5. Solution steps
6. Key code/config
7. Lessons learned
8. Finish

## Slide Rules

- Use one main idea per slide.
- Keep visible text short and reviewable.
- Put detailed explanation, discarded attempts, and expanded reasoning in speaker notes.
- Leave screenshot pages mostly empty so the user can paste screenshots manually.
- Use separate slides for large screenshots.
- Use additional screenshot slides for long screenshots, zoomed error regions, before/after screenshots, or multiple relevant UI states.
- Never shrink screenshots into unreadable thumbnails just to fit text.
- Use stable issue numbering such as `001`, `002`, `003`.
- For long-term decks, maintain a cover, table of contents, category index, and recent additions page when creating or updating the whole deck.

## Issue Overview Slide

Include:

- Issue number and title
- One-sentence summary
- Technology stack or topic
- Status such as `Solved`, `Draft`, or `Needs review`
- Final cause in one short sentence
- Final solution in one short sentence

Avoid long background paragraphs on the overview slide.

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

When creating or updating a deck containing multiple issues, maintain:

- Cover page
- Table of contents by issue number
- Category index by topic/technology
- Recent additions page

Table of contents format:

```text
001 useEffect repeated API requests       React       Solved
002 Git push rejected                     Git         Solved
003 SQL JOIN duplicate rows               SQL         Draft
```

Each issue should start after the previous issue's Finish slide.

Do not list Finish slides as separate issues in the table of contents.

## Output Path

Before creating a PPTX, determine the final output path.

Priority:

1. Use the exact output path provided by the user.
2. If the user provides only a directory, create a clear filename in that directory.
3. If the user does not provide a path, ask where to save the PPTX before authoring the deck.
4. Use a default output location only when the user explicitly says to use the default or does not care.

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
