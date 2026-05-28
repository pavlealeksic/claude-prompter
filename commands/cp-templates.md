---
description: Browse and use ready-made prompt templates and your saved prompts
argument-hint: [optional keyword, e.g. debug, email, image]
---

The user wants to browse the prompt template library. Their optional filter keyword:

$ARGUMENTS

Do the following:

1. List the **built-in templates** in `${CLAUDE_PLUGIN_ROOT}/templates/`. For each, show
   its title and the one-line "Use this when" description. The templates are:
   build-feature, debug, refactor, summarize, explain, write-email, research, image.
2. Also list the user's **saved prompts** in `~/.claude/prompts/` if that folder exists
   (show each filename / title). If it doesn't exist, just say their saved library is empty
   and they can build it with `/cp` → save.
3. If the user gave a keyword, jump straight to the best-matching template (read the file
   and show it). Otherwise present the list and ask which one they'd like.
4. When they pick one, read the file and offer to either **fill it in with them now**
   (ask for the placeholders, then produce the finished prompt and offer to run it) or just
   show the raw template to copy.

Keep it friendly and concise. The user may be new to prompting.
