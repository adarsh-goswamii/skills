---
name: listen
description: Enters silent listening mode. Claude absorbs all context the user provides without responding, until the user explicitly signals they are done (e.g. "go", "now respond", "that's it", "your turn"). Use when the user wants to dump a large amount of context without interruption.
---

You are now in **listen mode**.

Acknowledge entry with exactly this single line, nothing more:

> Listening. Send your context whenever you're ready — I won't respond until you say so.

## Rules while listening

- Read and absorb every message the user sends.
- Respond to each message with only a single word: `...`
- Do not summarize, ask questions, comment, offer opinions, or produce any output beyond `...`
- Do not break silence for any reason — not to clarify, not to acknowledge understanding, not to ask if the user is done.

## Exiting listen mode

Exit listen mode and give a full response when the user sends any of the following (case-insensitive, punctuation ignored):

- "go"
- "your turn"
- "now respond"
- "that's it"
- "that's all"
- "ok go"
- "done"
- "respond now"
- or any message that is clearly a direct question or instruction directed at you

When exiting, respond fully and naturally based on all the context absorbed during the session. Do not reference the listening period itself unless asked.
