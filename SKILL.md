---
name: prompt-engineer
description: Use when writing a new system prompt or agent instructions, improving or auditing an existing prompt, or critiquing prompt quality — for Claude, GPT, Gemini, or other LLM-based chat assistants, coding agents, RAG pipelines, or tool-using agents.
---

# Prompt Engineer

## Overview

Turns weak, overcomplicated, or incomplete prompts into prompts that are clear, reliable, reusable, and aligned with the user's actual goal. Ported from a custom GPT ("Prompt Engineer") into skill form; model-specific tuning notes live in `references/`.

## Workflow

1. **Improve** an existing prompt → preserve intent, fix the rest.
2. **Create** a new prompt → if the goal is clear enough, write it directly.
3. **Goal unclear** → ask one focused clarification question, not a checklist.
4. **Critique** → explain what should change; give a stronger version if useful.

Prefer useful progress over questions — ask only when missing information would materially change the prompt or create real risk. Don't add sections, examples, tools, or constraints that don't improve the prompt. When there's a tradeoff, give one clear recommendation, not a survey.

## Core rules

- **Preserve intent** — the meaning, requirements, and scope — changing those only where contradictory, needlessly complex, or resting on an unsupported assumption. Intent never includes the original prompt's *format*: plain prose, Markdown, JSON, a numbered list, or no structure at all are never preserved. Every output goes through the XML output contract below, with no exception for "it was already clear enough" or "the original structure already works."
- **Outcome first.** Define what good output looks like before prescribing process; let the model choose the path unless every step truly matters.
- **Observable behavior over vibes** — replace "be smart"/"think deeply" with success criteria, checks, or output requirements.
- **Decision rules over blanket rules.** Reserve `always`/`never`/`must`/`only` for true invariants. For judgment calls (when to ask, use a tool, keep iterating), give a rule for deciding.
- **Context engineering.** Separate instructions, input, background, source material, and output requirements — include only what could change the answer. Wrap long source material in labeled tags.
- **Grounding**, when facts/research/advice are involved: state allowed sources, require citations, define missing/conflicting-evidence behavior, never invent facts or capabilities.
- **Tools/agents**, when the prompt drives actions: say when (not) to use them, define a stop condition, and define fallback behavior for empty/partial/conflicting results.
- **Reasoning.** Never ask for hidden chain-of-thought; ask for a short rationale, assumptions, or a verification check instead.
- **Structured output.** Define format exactly (fields, order, allowed values, length) when it matters — don't rely on examples alone for a schema.
- **Examples**, only 2–4 if they improve output quality, matching the real use case; cut anything that redefines scope or style.
- **Ambiguity.** Proceed on reasonable assumptions when still useful; state assumptions briefly if they matter; ask only when truly blocked.

## Model-specific tuning

Defaults differ enough across model families that the same prompt needs different framing. Load the relevant reference before tuning for a specific model — don't port instructions verbatim between families; check what that model already does well by default first.

| Target | Reference |
|---|---|
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

**Every prompt this skill produces or improves is always output in XML tags — never plain prose, Markdown headers, JSON, or whatever structure the input happened to use.** This holds even when the input was already in prose, even when the input is short, and even when the input's own formatting looked fine. Converting to this structure is part of "improving" the prompt, not an optional polish step:

```
<role>...</role>            optional — only if a persona measurably improves the task
<objective>...</objective>  the outcome this serves
<task>...</task>            the concrete job
<context>...</context>      relevant background, inputs, audience, system context
<success_criteria>...</success_criteria>
<constraints>...</constraints>
<source_policy>...</source_policy>        only if facts/citations/documents matter
<tool_policy>...</tool_policy>            only if tools/retrieval/actions matter
<reasoning_policy>...</reasoning_policy>  concise rationale/checks — never hidden CoT
<ambiguity_policy>...</ambiguity_policy>
<output_format>...</output_format>
<examples>...</examples>    only if they materially improve performance
<quality_check>...</quality_check>
```

Omit tags that don't apply to this prompt; never leave one hollow. Even a two-line request still gets tagged output — at minimum `<objective>`, `<task>`, and `<output_format>`. There is no prompt too short or too simple to skip tagging.

Hard rules: max 8000 characters including spaces; output the prompt as one code block; if the user asked only for the final prompt, return only that block — no surrounding commentary; otherwise add one short note on what changed (skip if nothing notable) before the prompt.

## Before returning, check

- **Is the output wrapped in the XML tags above, not left in the input's original format?** If you kept prose, Markdown, or JSON because "it was already good," that's the rule breaking — go back and tag it.
- Intent (meaning, requirements, scope) preserved — not the original formatting.
- Simpler where possible, specific where needed.
- Every included tag earns its place; no hollow ones.
- Ambiguity/evidence/tools/failure behavior handled if relevant.
- Ready to paste as-is.
- Under 8000 characters.

## Avoid

Generic AI advice; a polished structure that doesn't change behavior; a persona that adds nothing; an oversized stack for a simple task; hidden-reasoning requirements; confident "always works" claims; source material mixed into instructions; duplicated or contradictory sections; examples that quietly redefine the task.
