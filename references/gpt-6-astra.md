# GPT-6 Astra

Use for Astra prompt tuning or audits of skills and agent instructions. Checked against official OpenAI guidance on 2026-09-21. Older GPT references remain for their own targets.

## Skills and instruction audits

Keep discovery descriptions narrow. Load supporting references only for the relevant task. Remove repetitive recipes and mandatory reading unrelated to the change; retain domain knowledge, interfaces, and real constraints. Audit inherited approval and stopping rules: an obsolete review gate can prevent completion. Define the intended endpoint and bounded authority rather than an indefinite instruction to keep working.

Source: [Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra).

## Behavioral tuning

Astra may pause too early or verify too broadly. Specify completion, carry forward existing authorization, and prepare permitted work before any required approval. Preserve actual permission boundaries. State how user requirements relate to skill defaults; identify the exact rule if a skill blocks progress.

Set the desired writing style. Calibrate verification to impact; repeat successful checks only after relevant changes or new concerns. Define delegation criteria only when the runtime supports it and the task benefits.

## API caveats — only for integration work

- Use `gpt-6-astra`; tool calling requires Responses.
- Migrate `none`/`minimal` effort to `low`; otherwise initially retain effective effort.
- Remove `temperature`, `top_p`, and `top_logprobs`; also remove Chat Completions `logprobs` or Responses `message.output_text.logprobs` includes.
- Async tools and mid-turn steering require application support; prose cannot enable them.
- Recheck current documentation for caching, effort updates, and regional service-tier restrictions before configuring them.

Source: [Using GPT-6 Astra](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-6-astra).
