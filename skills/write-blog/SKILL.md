---
name: write-blog
description: >
  End-to-end blog writing assistant. Guides from rough notes to a polished
  markdown post. Covers technical tutorials, project showcases, and tech
  commentary. Tone: conversational and direct. Only invoke when user explicitly
  calls /write-blog.
---

When this skill is invoked, follow this exact workflow in order. Do not skip phases.

## Phase 1 — Intake

Ask all of these at once (single message, not one by one):

1. **Topic / working title** — what's the post about?
2. **Target audience** — junior devs (explain from scratch), mid-level peers (assume solid fundamentals), or mixed?
3. **Post type** — tutorial/how-to, project showcase, or opinion/commentary?
4. **Rough notes** — paste any bullets, fragments, or brain dump you have. Nothing is too messy.

Wait for the user to respond before moving on.

---

## Phase 2 — Outline

Generate a tight outline:
- **Title** — punchy and concrete (avoid "A guide to X", "Introduction to X")
- **One-line hook** — the lede that earns the read
- **Section headers** — each with one bullet describing what it covers
- **Estimated read time**

Present the outline and ask: *"Does this structure work, or should we adjust anything before I draft?"*

Iterate until the user approves. Do not start drafting until they say so.

---

## Phase 3 — Draft

Write the full post in markdown. Follow these rules without exception:

- **Tone:** conversational and direct — peer to peer, no corporate fluff
- **Opening:** hook immediately — a sharp problem statement, a surprising fact, or a bold take. Never open with "In this post, we will..."
- **Paragraphs:** 2–4 lines max
- **Technical posts:** real code snippets, not pseudocode. Explain the why, not just the what
- **Project showcases:** lead with what was built and why, then get into how
- **Commentary:** lead with the opinion, support it, don't hedge at the end
- **Closing:** concrete takeaway or next action. Never "I hope you found this useful"
- **Headers:** `##` and `###` only — `#` is reserved for the title
- **Audience calibration:** adjust depth and assumed knowledge based on Phase 1 answer

After the draft, ask: *"Want me to tighten anything, adjust tone anywhere, or is it ready to save?"*

Iterate if needed.

---

## Phase 4 — Save

When the user approves, save the file:

**Path:** `~/Documents/docs/blogs/[kebab-case-title].md`

**Frontmatter at the top:**
```
---
title: [title]
date: [today's date YYYY-MM-DD]
audience: [junior | mid-level | mixed]
type: [tutorial | showcase | commentary]
tags: [relevant comma-separated tags]
---
```

After the post body, add wiki-style tag links: `[[tag-name]]` for each tag.

For each tag, check if `~/Documents/docs/_tags/[tag-name].md` exists. If not, create it with a brief description and a "Used in" section linking back to `[[blogs/[slug]]]`. If it exists, add the backlink to its "Used in" section.
