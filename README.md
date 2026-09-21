<div align="center">

# 🧠 Prompt Engineer

**An agent skill that turns weak, ambiguous, or overcomplicated prompts into clear, usable, task-appropriate ones.**

Drop it into Claude Code, Codex, or any agent that supports the [Agent Skills spec](https://agentskills.io/specification) — no setup beyond copying a folder.

[![Skill Spec](https://img.shields.io/badge/Agent%20Skills-compatible-6E56CF?style=flat-square)](https://agentskills.io/specification)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-ready-CC785C?style=flat-square)](https://docs.claude.com/en/docs/claude-code/skills)
[![License: MIT](https://img.shields.io/badge/license-MIT-3DA639?style=flat-square)](LICENSE)

</div>

---

## What it does

`prompt-engineer` is a reference skill for writing and improving prompts — system prompts, agent instructions, RAG prompts, one-off chat prompts, whatever needs to reliably steer an LLM. It was ported from a working custom GPT and rebuilt as a portable, model-agnostic skill.

Point an agent at it and ask it to write or improve a prompt. It will:

| Situation | What happens |
|---|---|
| You hand it an existing prompt | It preserves intent and required formats, keeps working parts, and fixes concrete weaknesses |
| You describe a new prompt you need | It writes one directly if the goal is clear enough |
| The goal is too vague | It asks **one** focused clarification question — never a checklist |
| You ask for a critique | It explains concrete issues and illustrates fixes when useful |

Results follow the requested format and preserve integration contracts. Otherwise, the skill chooses prose, Markdown, or XML according to the prompt's needs. Standalone prompts come in one pasteable code block by default; critiques and repository edits use the appropriate delivery format.

## Structure that serves the task

A short prompt does not need a template. Longer prompts can use labeled sections or XML tags to distinguish instructions, context, sources, and examples. The skill treats formatting as a choice and keeps the requested response schema separate from the prompt's own presentation. See [`SKILL.md`](SKILL.md#output-contract) for the delivery rules.

## GPT-6 Astra update

The Astra reference covers completion and approval boundaries, verification scope, writing style, and API migration caveats. The shared skill now uses a narrower discovery description, loads only relevant model guidance, and removes unconditional XML conversion and fixed example counts. These are editorial changes based on official guidance; they are not a claim of measured performance improvement.

## Built-in model tuning

Prompting patterns that work well for one model family can misfire on another — different defaults for verbosity, tool-use triggering, reasoning depth, and instruction literalism. Rather than guessing, the skill ships with sourced reference docs for supported model targets and loads the right one only when it's relevant:

| Model family | Reference |
|---|---|
| GPT-6 Astra | [`references/gpt-6-astra.md`](references/gpt-6-astra.md) |
| Claude Opus 5 | [`references/claude-opus-5.md`](references/claude-opus-5.md) |
| Claude Sonnet 5 | [`references/claude-sonnet-5.md`](references/claude-sonnet-5.md) |
| Claude Fable 5 / Mythos 5 | [`references/claude-fable-5.md`](references/claude-fable-5.md) |
| Claude cross-model fundamentals | [`references/claude-best-practices.md`](references/claude-best-practices.md) |
| GPT-5.6 prompting patterns | [`references/gpt-5.6.md`](references/gpt-5.6.md) |
| GPT-5.6 model & API changes | [`references/gpt-5.6-model-guide.md`](references/gpt-5.6-model-guide.md) |
| GPT-5.5 | [`references/gpt-5.5.md`](references/gpt-5.5.md) |
| GPT-5.4 | [`references/gpt-5.4.md`](references/gpt-5.4.md) |

These stay out of the main skill file so they only load into context when a specific model is actually being tuned for — keeping the always-loaded footprint small.

## Install

This is a plain [Agent Skills spec](https://agentskills.io/specification) folder — `SKILL.md` at the root, everything else in `references/`. It works with any agent that reads that spec: Claude Code, Codex, Cursor, OpenCode, and [70+ others](https://github.com/vercel-labs/skills).

### Option A — the `skills` CLI (recommended, agent-agnostic)

```bash
npx skills add https://github.com/LeonardSEO/prompt-engineer-skill
```

The CLI detects which agents you have installed and asks which one(s) to target — or specify explicitly:

```bash
npx skills add https://github.com/LeonardSEO/prompt-engineer-skill -a claude-code   # Claude Code
npx skills add https://github.com/LeonardSEO/prompt-engineer-skill -a codex        # Codex
npx skills add https://github.com/LeonardSEO/prompt-engineer-skill -a cursor       # Cursor
```

Add `-g` to install into your user directory (available in every project) instead of just the current one. See [vercel-labs/skills](https://github.com/vercel-labs/skills) for the full CLI reference.

### Option B — clone directly into an agent's skills folder

Every agent that supports skills reads them from a predictable path. Clone into whichever one applies:

| Agent | User-wide | Project-scoped |
|---|---|---|
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| Codex | `~/.codex/skills/` (or `$CODEX_HOME/skills/`) | `.agents/skills/` or `.codex/skills/` |

```bash
git clone https://github.com/LeonardSEO/prompt-engineer-skill.git ~/.claude/skills/prompt-engineer   # Claude Code, user-wide
git clone https://github.com/LeonardSEO/prompt-engineer-skill.git ~/.codex/skills/prompt-engineer    # Codex, user-wide
```

For other agents, check that agent's docs for where it looks for skills, then clone this repo there.

## Repo layout

```
prompt-engineer/
├── SKILL.md              # the skill itself — workflow, rules, output contract
└── references/           # model-specific tuning notes, loaded on demand
    ├── claude-opus-5.md
    ├── claude-sonnet-5.md
    ├── claude-fable-5.md
    ├── claude-best-practices.md
    ├── gpt-6-astra.md
    ├── gpt-5.6.md
    ├── gpt-5.6-model-guide.md
    ├── gpt-5.5.md
    └── gpt-5.4.md
```

## License

[MIT](LICENSE) — use it, fork it, adapt it to your own model roster.
