---
name: doc-it
description: Capture the current conversation's context into the docs vault — either updating an existing markdown file or creating a new one. Claude decides which based on topic overlap. Always proposes changes before writing.
---

You are now in documentation-capture mode. The goal: convert what the user just learned in this conversation into durable documentation under `~/Documents/docs/[project]/`.

## Workflow

### 1. Identify the topic

Summarize in one sentence what this conversation is *about* — the feature, system, or decision worth preserving. If the conversation covered multiple topics, ask the user which one to capture.

If the user passed an argument (e.g. `/doc-it founder-sheet email modal`), use that as the topic hint.

### 2. Search existing docs

Look for a doc that covers the same feature/system — not just one that mentions the same keywords.

- `mcp__ask-docs__keyword_search` with the topic
- `mcp__ask-docs__rag_search` with a paraphrased version
- List files in `~/Documents/docs/[current-project]/` directly

### 3. Decide: update vs create

**Update an existing doc when:**
- It covers the same feature, system, or component
- The new info is additive (new section), corrective (fixing an inaccuracy), or a deeper dive into an existing section
- The existing doc is under ~600 lines and the addition wouldn't push it past that

**Create a new doc when:**
- Topic is genuinely distinct (different feature/subsystem)
- Existing doc is already long and adding would dilute focus
- The new context is a different *kind* of doc (e.g. existing is architecture, new would be a runbook)

When borderline, prefer update — fewer files, stronger backlinks.

### 4. Propose before executing

ALWAYS show the user a structured plan first.

**For updates**, show:
- Which file
- Which sections will change (added / modified / removed)
- The actual content of new or substantially changed sections (so the user can react to wording, not just intent)

**For new files**, show:
- Target path
- Proposed section outline (H2 headings)
- Tags to apply
- Any new tag files that would need to be created

Wait for explicit confirmation. Do not edit until the user says go.

### 5. Execute

- **Updates**: use `Edit` tool, preserve existing structure and tone, don't reformat unrelated sections
- **New files**: follow conventions in `~/.claude/CLAUDE.md` — full path `~/Documents/docs/[project-name]/[filename].md`, wiki-link tags at bottom, create or update tag files in `~/Documents/docs/_tags/`
- Update tag files' "Used in" sections for any new doc

### 6. Report

One line: `Updated path/file.md — added X section` or `Created path/file.md with tags [a, b, c]`. No summary of what was written — the diff speaks for itself.

## Content rules

- Capture *why* and *non-obvious behaviors*, not API surface restatements
- If something is buggy or known-incorrect, put it in a "Known Issues" section, not buried mid-description
- Link to code with `file_path:line_number` instead of pasting code blocks, unless the snippet is the whole point
- Don't include ephemeral context: in-progress task state, current branch, turn-by-turn conversation flow
- Don't paraphrase existing docs into the new one — link to them with `[[doc-name]]`

## Tone

- Match the existing vault's tone (terse, declarative, table-heavy)
- No emojis unless the user added them
- No "this document describes..." preambles
