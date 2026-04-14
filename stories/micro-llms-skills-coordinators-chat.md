# Micro LLMs, skills/tools, and keeping coordinators small

*Paraphrased from a community chat (Discord-style thread).*

### Idea: skills and micro models converge

One line of thought: **tool “skills”** and **small LLMs** may **merge**—you want a model that is genuinely strong at *one* thing (e.g. a file type), and a **coordinating** agent calls a **tool write** (or equivalent) that **routes to that micro model** trained for the skill. That gives **just-in-time, tool-specific sub-agents** so the coordinator does **not** keep every skill description and nuance in its own context.

### What already helps: lazy loading

Others noted that **skills and tools are already lazy-loaded** in some products: e.g. **search for the tool first**, then pull in its **description**—reportedly automatic in some “Claude code” style setups and **configurable** elsewhere. Pushback: even then, some **context is still spent** on the path to invocation; the open problem is partly **search / routing quality** for tools and skills.

### Micro LLM + LoRA: one experiment

Someone **routed to hot-swapped LoRA adapters** on **local** models using a **fast classifier**. Conclusion for them: **fine-tuning effort did not pay off** compared with how fast **frontier models** and **context windows** are moving—**not worth it today** for their use case. They expect **better lazy-load + retrieval** for tools/skills to **scale** more cleanly for big models.

### When fine-tuning might matter

Another participant: **no production wins from fine-tuning yet**—mostly **watching**. They helped prep a **synthetic data** pipeline toward a **Clojure-focused** small model but are **waiting for better small base models**. The bar is high: the specialist must be **good enough that offload saves cost**, not because the coordinator *cannot* do the task—**economics**, not raw capability. **“We’re getting close”** was the mood, not “we’re there.”

### Local “sudo-agent” and unix-style micro models

Same thread floated a **local sudo-agent**: coordinators (local or remote) **route privileged / secret-touching commands** through an **on-device, user-owned** agent so secrets stay in a **controlled local boundary**.

Longer-term picture: **single-skill micro LLMs** as **small, composable utilities**—eventually modest sizes (**10B → 1B → even ~1M** in the speculation) for a dedicated **sudo** or file-type specialist, **“baked in”** like classic Unix tools.

### Tension

**Coordinator stays dumb about details** vs **one big context** vs **specialist fine-tunes**—tradeoffs shift as **models**, **context**, and **tool-discovery UX** improve.
