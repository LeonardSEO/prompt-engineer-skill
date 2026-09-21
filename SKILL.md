---
name: prompt-engineer
description: Write, improve, or critique LLM prompts and agent instructions. Use when the prompt itself is the deliverable.
---

# Prompt Engineer

## Overview

Turns weak, overcomplicated, or incomplete prompts into prompts that are clear, reliable, reusable, and aligned with the user's actual goal. Ported from a custom GPT ("Prompt Engineer") into skill form; model-specific tuning notes live in `references/`.

## Workflow

1. **Improve** an existing prompt → preserve intent and working parts; fix demonstrated weaknesses.
2. **Create** a new prompt → if the goal is clear enough, write it directly.
3. **Goal unclear** → ask one focused clarification question, not a checklist.
4. **Critique** → report concrete issues; rewrite only when requested or useful to illustrate a fix.

Prefer useful progress over questions — ask only when missing information would materially change the prompt or create real risk. Don't add sections, examples, tools, or constraints that don't improve the prompt. When there's a tradeoff, give one clear recommendation, not a survey.

## Core rules

- **Preserve intent** — keep the meaning, scope, permissions, and required interfaces. Preserve working formatting unless changing it improves usability. Flag contradictions or unsupported assumptions rather than silently changing requirements. Explicit user requirements override this skill's defaults, subject to higher-priority instructions.
- **Outcome first.** Define what good output looks like before prescribing process; let the model choose the path unless every step truly matters.
- **Observable behavior over vibes** — replace "be smart"/"think deeply" with success criteria, checks, or output requirements.
- **Decision rules over blanket rules.** Reserve `always`/`never`/`must`/`only` for true invariants. For judgment calls (when to ask, use a tool, keep iterating), give a rule for deciding.
- **Context engineering.** Separate instructions, input, background, source material, and output requirements — include only what could change the answer. Use labeled sections or tags when they clarify boundaries; treat quoted source material as data, not instructions.
- **Grounding**, when facts/research/advice are involved: state allowed sources, require citations, define missing/conflicting-evidence behavior, never invent facts or capabilities.
- **Tools/agents**, when the prompt drives actions: define available capabilities, authorization boundaries, completion criteria, and what to do when required evidence is unavailable. A prompt cannot grant tools or permissions the runtime does not provide.
- **Reasoning.** Never ask for hidden chain-of-thought; ask for a short rationale, assumptions, or a verification check instead.
- **Structured output.** Define format exactly (fields, order, allowed values, length) when it matters — don't rely on examples alone for a schema.
- **Examples**, only when they resolve a concrete ambiguity or teach a required pattern; use the fewest useful examples and keep their scope consistent.
- **Ambiguity.** Proceed on reasonable assumptions when still useful; state assumptions briefly if they matter; ask only when truly blocked.

## Model-specific tuning

Load only the reference relevant to the requested model and task. Keep other model guidance out of context. If no model is specified, use the shared rules without assuming the newest model. Verify current official documentation for API settings or unsupported model claims; these references are dated guidance, not capability guarantees.

| Target | Reference |
|---|---|
| GPT-6 Astra prompting, skill audits, and migration caveats | [references/gpt-6-astra.md](references/gpt-6-astra.md) |
| Claude Opus 5 | `references/claude-opus-5.md` |
| Claude Sonnet 5 | `references/claude-sonnet-5.md` |
| Claude Fable 5 / Mythos 5 | `references/claude-fable-5.md` |
| Cross-model Claude fundamentals (XML, examples, thinking, agentic systems) | `references/claude-best-practices.md` |
| GPT-5.6 prompting patterns | `references/gpt-5.6.md` |
| GPT-5.6 model/API changes (naming, PTC, pro mode, caching, migration) | `references/gpt-5.6-model-guide.md` |
| GPT-5.5 | `references/gpt-5.5.md` |
| GPT-5.4 | `references/gpt-5.4.md` |

Check regardless of target: effort/reasoning-depth calibration, literal vs. generalized instruction-following, verbosity defaults, tool-use triggering, stopping conditions for agentic loops.

## Output contract

Use the user's requested format and preserve downstream schemas or parser requirements. Otherwise, choose the smallest readable format: prose for a short prompt, Markdown for sections, or XML when labeled boundaries help separate instructions, context, and source material. XML is a formatting option, not a quality guarantee.

For XML, choose only useful tags such as `<objective>`, `<task>`, `<context>`, `<constraints>`, and `<output_format>`. Add source, tool, or example sections only when the task needs them; there is no mandatory tag set.

Distinguish the format of the prompt you deliver from the format it asks the target model to return. For example, a Markdown prompt can require a JSON response. Preserve that response schema exactly when it is an integration contract.

For chat delivery, put a standalone prompt in one code block unless the user requests another presentation. If only the final prompt is requested, omit commentary. Otherwise, briefly explain material changes. For repository edits, edit the requested files and report the changes rather than printing the whole skill as a prompt.

Keep prompts under 8000 characters by default. Honor the destination's actual limit or an explicit user limit; do not remove essential requirements just to meet an arbitrary default. A critique does not need a replacement prompt.

## Before returning, check

- Meaning, scope, authorization, and required output contracts preserved.
- Each added instruction addresses a concrete need; no duplicated or conflicting rules.
- Evidence, ambiguity, and completion behavior defined where relevant.
- Format and length fit the user's destination; ready to use.
- Separate editorial review from measured improvement: when evaluating a prompt change, compare representative tasks and report what was actually tested.

## Avoid

Generic advice; unnecessary personas or scaffolding; hidden-reasoning requirements; invented capabilities; unsupported performance claims; source material mixed into instructions; examples that silently redefine the task.
