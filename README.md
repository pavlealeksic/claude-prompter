# claude-prompter

> Turn rough ideas into strong, well-structured prompts — right inside Claude Code.

`claude-prompter` is a Claude Code plugin that acts as your **prompt coach**. Type `/cp`
(or `/claude-prompt`) followed by a messy one-liner, and it hands back a polished,
**domain-aware** prompt, explains what it improved in plain English, and offers to run it,
save it, or tweak it. It only asks clarifying questions when something essential is
missing, so it stays out of your way.

It's built for people who are **new to prompting** and want better results without first
learning prompt engineering — but it's just as handy for experienced users who want a fast,
consistent way to draft strong prompts.

---

## Table of contents

- [Why use it](#why-use-it)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick start](#quick-start)
- [Commands](#commands)
- [How it works](#how-it-works)
- [Worked examples](#worked-examples)
- [Template library](#template-library)
- [The prompt-coach skill](#the-prompt-coach-skill)
- [Your personal prompt library](#your-personal-prompt-library)
- [Customizing & extending](#customizing--extending)
- [Project structure](#project-structure)
- [Troubleshooting](#troubleshooting)
- [Updating & uninstalling](#updating--uninstalling)
- [Contributing](#contributing)
- [License](#license)

---

## Why use it

Most weak results come from weak prompts, not a weak model. Beginners typically leave out
the four things that matter most: the **goal**, the **context**, the **constraints**, and
the **output format**. `claude-prompter` fills those gaps for you and shows you what it
added, so you learn the pattern over time.

**Before:**

```
make a login function
```

**After (what the plugin produces):**

```
Write a TypeScript `login(email, password)` function for our Express API.

Context: users are stored in Postgres via Prisma; passwords are bcrypt-hashed.
Requirements:
- Look up the user by email, verify the password with bcrypt.
- On success, return a signed JWT (1h expiry) using the JWT_SECRET env var.
- On failure, throw an AuthError with a generic "invalid credentials" message.
Constraints: no plaintext password logging; don't reveal whether the email exists.
Output: the function plus a one-line usage example.
```

Same intent — far better result.

---

## Features

- **Domain-aware enhancement.** Detects whether you're doing code, debugging, refactoring,
  summarizing, explaining, writing, research, image generation, or a multi-step task, and
  applies that domain's best practices.
- **Built-in template library.** Eight ready-made, high-quality prompt scaffolds. Browse
  them with `/cp-templates`; the enhancer pulls the right one automatically.
- **Asks only when needed.** If your draft is good enough, it enhances it directly. If
  something essential is missing, it asks 1–3 short, multiple-choice questions — never an
  interrogation.
- **Teaches as it goes.** Every result includes a short "What I improved" note in plain
  language, so you pick up the patterns yourself.
- **Few-shot quality bar.** Weak→strong examples are baked into the methodology to keep the
  output consistently strong.
- **Proactive coach skill.** Claude can offer to sharpen a vague prompt even when you
  didn't type the command — and stays quiet when your request is already clear.
- **Personal prompt library.** Save prompts you like to `~/.claude/prompts/` and reuse them
  across every project; they show up in `/cp-templates`.
- **Single source of truth.** All commands share one methodology file, so the behavior is
  consistent and easy to customize.

---

## Requirements

- [Claude Code](https://docs.claude.com/en/docs/claude-code) with plugin support.
- That's it — no API keys, no external services, no dependencies to install.

---

## Installation

Claude Code installs plugins from a **marketplace**. This repo *is* a marketplace that
contains a single plugin, so installation is two commands.

### From a local clone

```
/plugin marketplace add /path/to/claude-prompter
/plugin install claude-prompter@claude-prompter
```

For example, if you cloned it to the path used while building this:

```
/plugin marketplace add /Users/pavlealeksic/Gits/moji/claude-prompter
/plugin install claude-prompter@claude-prompter
```

### From a git remote

Once you've pushed this repo somewhere:

```
/plugin marketplace add <your-git-repo-url>
/plugin install claude-prompter@claude-prompter
```

The syntax is `/plugin install <plugin-name>@<marketplace-name>`. Both are
`claude-prompter` here, which is why the command reads a little repetitively.

### Verify it loaded

After installing, the new commands should appear when you type `/`. If they don't, reload
or restart Claude Code. You can also run `/plugin` to see installed plugins and confirm
`claude-prompter` is listed and enabled.

---

## Quick start

```
/cp build me a website
```

You'll be asked a couple of quick questions (what kind of site, for whom, any tech
preference), then handed a clean, structured prompt and asked whether to run it, save it,
or edit it.

A few more to try:

```
/cp write a function that dedupes a list but keeps order
/claude-prompt help me draft an email asking my manager for time off
/cp summarize this meeting
/cp-templates debug
```

---

## Commands

| Command | Argument | What it does |
| --- | --- | --- |
| `/cp` | `<your idea>` | Enhance a rough draft into a strong, domain-aware prompt. |
| `/claude-prompt` | `<your idea>` | Identical to `/cp` — the long-form name. |
| `/cp-templates` | `[keyword]` | Browse the template library and your saved prompts; optionally jump straight to a match. |

### `/cp` and `/claude-prompt`

The main event. Pass anything after the command:

```
/cp <whatever you're trying to do>
```

It detects the domain, fills in the missing pieces (asking only if essential), and returns
a copy-pasteable prompt plus a short note on what it improved. Then it offers to **run**,
**save**, **edit**, or finish. The two command names are interchangeable — `/cp` is just
faster to type.

If you run it with no text, it asks what you want to do first.

### `/cp-templates`

Browse the ready-made scaffolds and any prompts you've saved:

```
/cp-templates            # list everything and pick one
/cp-templates debug      # jump straight to the closest match
/cp-templates email      # e.g. opens the write-email template
```

Pick a template and it will either fill it in with you (asking for the blanks) or just show
you the raw version to copy.

---

## How it works

When you run `/cp <draft>`, the command tells Claude to follow the methodology in
`prompting-guide.md`. The flow is:

1. **Read your draft.** If it's empty, ask what you want to do.
2. **Detect the domain** and load the matching template as a scaffold.
3. **Assess what's missing** against the things that make prompts work: goal, context,
   requirements, constraints, output format, success criteria.
4. **Ask only if essential.** 1–3 multiple-choice questions, or none at all.
5. **Write the enhanced prompt** in a copy-pasteable block, sized to match the task — small
   asks get small prompts.
6. **Explain what improved** in 2–4 plain bullets.
7. **Offer next steps:** run it, save it, edit it, or you're done.

---

## Worked examples

### Code

**You type:** `/cp a script to rename files`

**You might be asked:** which language, rename based on what rule, which folder.

**You get:**

```
Write a Python 3 script that renames every .jpg file in a given folder by prefixing the
file's creation date (YYYY-MM-DD-) to the existing name.

Requirements:
- Take the target folder as a command-line argument.
- Skip non-.jpg files; print a one-line summary of how many were renamed.
- Don't overwrite a file if the target name already exists — append -1, -2, etc.
Constraints: standard library only; do a dry-run print before renaming if --dry-run is set.
Output: the full script plus an example command to run it.
```

### Writing

**You type:** `/cp email asking for a day off`

**You get:**

```
Write a short, polite email to my manager requesting Friday, June 6 off as PTO.

Tone: warm but professional. Audience: my direct manager, who is busy.
Include: the date, that my work is on track, and an offer to hand off anything urgent.
Keep it under 90 words. End with a clear ask for approval.
```

### Summarize

**You type:** `/cp summarize this meeting`

**You get:**

```
Summarize the meeting transcript below for teammates who missed it.

Format:
- TL;DR (2 sentences)
- Decisions (bullets)
- Action items (owner → task → due date)
Leave out small talk and tangents. Keep the whole thing under 200 words.

Transcript:
[paste here]
```

---

## Template library

Eight scaffolds ship with the plugin. The enhancer pulls the right one automatically, or
you can open them yourself with `/cp-templates`.

| Template | Use it when… |
| --- | --- |
| `build-feature` | You want new code — a function, component, script, or whole feature. |
| `debug` | Something is broken, throwing an error, or behaving unexpectedly. |
| `refactor` | The code works but you want it cleaner, faster, or better structured. |
| `summarize` | You want a long text, meeting, article, or thread condensed. |
| `explain` | You want to understand a concept or be taught something. |
| `write-email` | You need to draft an email, message, or note. |
| `research` | You want options investigated, compared, or a recommendation. |
| `image` | You want to describe an image for an image-generation model. |

Each template includes a "Use this when" line, the fill-in-the-blank prompt, and a couple
of tips on what matters most for that kind of request.

---

## The prompt-coach skill

Beyond the explicit commands, the plugin ships a **skill** (`prompt-coach`) that lets
Claude *proactively* offer to sharpen a prompt. It's designed to:

- **Step in** when a request is so vague that guessing would likely miss (e.g. "make me a
  website", "fix it"), or when you explicitly ask to improve a prompt.
- **Stay quiet** when your request is already clear and specific — it won't interrupt
  focused work.

When it does step in on a workable request, it offers help in a single line rather than
forcing the full flow:

> I can sharpen this into a stronger prompt first — want that, or should I just go ahead?

If you'd rather never be prompted proactively, you can disable just the skill while keeping
the commands — see [Customizing](#customizing--extending).

---

## Your personal prompt library

When you like an enhanced prompt, choose **save** and it's written to:

```
~/.claude/prompts/<slug>.md
```

Each saved file gets a small header (title, domain, date) so the folder stays browsable.
Because it lives in your home Claude config, the library is available in **every** project,
and your saved prompts show up alongside the built-in templates in `/cp-templates`.

Over time this becomes your own curated, reusable prompt collection.

---

## Customizing & extending

Everything is plain Markdown — no build step.

- **Change the coaching behavior** (tone, structure, how aggressively it asks questions):
  edit `prompting-guide.md`. All three commands and the skill read from it, so one edit
  changes everything.
- **Add a template:** drop a new `.md` file into `templates/` following the existing format
  (a "Use this when" line, a fenced template block, and a "Tips" line), then add it to the
  **Domain Playbook** section of `prompting-guide.md` so the enhancer knows about it.
- **Rename or add a command:** add a Markdown file to `commands/`. The filename becomes the
  command name (`commands/foo.md` → `/foo`). Use `$ARGUMENTS` to capture the user's text and
  `${CLAUDE_PLUGIN_ROOT}` to reference files inside the plugin.
- **Disable the proactive skill only:** remove or rename `skills/prompt-coach/SKILL.md` (or
  disable it via `/plugin`) — the `/cp` commands keep working.

---

## Project structure

```
claude-prompter/
├── .claude-plugin/
│   ├── plugin.json          # plugin manifest (name, version, description)
│   └── marketplace.json     # makes the repo installable as a marketplace
├── commands/
│   ├── cp.md                # /cp
│   ├── claude-prompt.md     # /claude-prompt (alias of /cp)
│   └── cp-templates.md      # /cp-templates
├── skills/
│   └── prompt-coach/
│       └── SKILL.md         # proactive, auto-invoked coaching
├── templates/               # 8 ready-made prompt scaffolds
│   ├── build-feature.md
│   ├── debug.md
│   ├── refactor.md
│   ├── summarize.md
│   ├── explain.md
│   ├── write-email.md
│   ├── research.md
│   └── image.md
├── prompting-guide.md       # the methodology — single source of truth
└── README.md
```

The command files are deliberately thin; the real logic lives once in `prompting-guide.md`.

---

## Troubleshooting

**The commands don't show up after installing.**
Reload or restart Claude Code. Run `/plugin` and confirm `claude-prompter` is installed and
enabled. Make sure you ran both the `marketplace add` and the `install` steps.

**`/plugin install` can't find the plugin.**
The format is `/plugin install claude-prompter@claude-prompter` (plugin name @ marketplace
name). Confirm the marketplace was added first with `/plugin marketplace add <path-or-url>`.

**Saved prompts don't appear in `/cp-templates`.**
They're only created after you choose "save" at least once. The folder is
`~/.claude/prompts/`; if it doesn't exist yet, your saved library is simply empty.

**The skill keeps offering to coach when I don't want it.**
Disable the skill (see [Customizing](#customizing--extending)) and use the `/cp` commands
explicitly instead.

---

## Updating & uninstalling

Update the marketplace and reinstall to pick up changes:

```
/plugin marketplace update claude-prompter
/plugin install claude-prompter@claude-prompter
```

Uninstall the plugin (and optionally remove the marketplace):

```
/plugin uninstall claude-prompter@claude-prompter
/plugin marketplace remove claude-prompter
```

Your saved prompts in `~/.claude/prompts/` are kept; delete that folder yourself if you
want them gone.

---

## Contributing

This plugin is intentionally simple to extend. The highest-value contributions are new
templates and refinements to `prompting-guide.md`. Keep templates concise, beginner-
friendly, and focused on the few details that most improve results for that domain.

Ideas worth adding next: a prompt **scoring / critique** mode (rate a prompt and explain
its weaknesses without rewriting it), and more domain templates (e.g. SQL, data analysis,
test writing).

---

## License

Use it however you like.
