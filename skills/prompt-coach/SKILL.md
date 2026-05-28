---
name: prompt-coach
description: >-
  Sharpen a vague or weak prompt into a clear, well-structured one before acting on it.
  Use when the user's request is so brief or ambiguous that guessing would likely produce
  the wrong result, OR when the user explicitly asks to improve, rewrite, or "make a better
  prompt." Detects the domain (code, debug, writing, summarize, explain, research, image),
  asks 1-3 questions only if essential info is missing, then proposes a stronger prompt.
  Do NOT trigger when the request is already clear and specific enough to act on directly.
---

# Prompt Coach

When this skill activates, you help the user turn a rough request into a strong prompt
instead of charging ahead on a guess.

## When to step in

- The user's message is vague enough that a good answer requires assumptions that could
  easily be wrong (e.g. "make me a website", "summarize this", "fix it").
- The user explicitly asks to improve, polish, or rewrite a prompt.

Do **not** step in when the request is already clear and specific — just do the work. Be
especially careful not to interrupt a focused, well-scoped ask with coaching.

## How to coach

Read and follow the full methodology in
`${CLAUDE_PLUGIN_ROOT}/prompting-guide.md`. In short:

1. Detect the domain and load the matching template from
   `${CLAUDE_PLUGIN_ROOT}/templates/` when one fits.
2. Ask 1–3 targeted questions **only if essential info is missing** (offer multiple-choice
   options so it's easy to answer).
3. Propose an enhanced prompt in a copy-pasteable block, sized to the task.
4. Note in 2–4 plain-language bullets what you improved.
5. Offer to **run it**, **save it** to `~/.claude/prompts/`, **edit**, or stop.

## Light touch

If the user clearly just wants the task done and the request is workable, offer the
improved-prompt option in a single line rather than forcing the full flow:

> I can sharpen this into a stronger prompt first — want that, or should I just go ahead?
