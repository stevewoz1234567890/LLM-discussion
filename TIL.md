# Today I Learned

## CPU cache coherence (MESI) for multi-agent LLM coordination

**TL;DR:** Hardware cache coherence (MESI: Modified / Exclusive / Shared / Invalid) maps well to reducing token waste when many agents share artifacts. Tag local copies with MESI-style validity; re-fetch only on invalidation after another agent writes. Lazy invalidation can collapse many writes into one re-fetch.

### The problem

In multi-agent setups, agents often share docs, tool outputs, and intermediate reasoning. A common pattern: Agent A writes a shared document, then Agents B, C, D each pull the **full** artifact into context even when they do not need it yet. With LangGraph and `BaseStore`, this can balloon—on the order of **1.5M+ unnecessary “sync” tokens** in a 50-step workflow (per author’s measurements).

### The analogy

CPU caches avoid re-reading main memory every cycle by tracking whether a local line is still valid. **MESI** is the classic protocol for that.

### The adaptation

Agents keep **state references** with MESI-like flags and **only re-fetch when invalidated** by another agent’s write. **Lazy invalidation** lets multiple consecutive writes collapse into a single re-fetch at the next read.

### Results (reported)

- **84–95%** reduction in synchronization tokens, workload-dependent; **write-heavy** workloads benefit most (many writes → one re-fetch on read).

### References

- Paper: [arXiv:2603.15183](https://arxiv.org/abs/2603.15183)
- Code: [github.com/hipvlady/agent-coherence](https://github.com/hipvlady/agent-coherence) (LangGraph `BaseStore` adapter)

### Open question

Has anyone else seen this kind of token duplication in LangGraph pipelines, and how are you handling it?

---

## LangChain / LangGraph in the 2026 agentic engineering job market

[agentic-engineering-jobs.com](https://agentic-engineering-jobs.com) runs a job board for engineers who build AI agents. They queried **591** listings and summarized the LangChain-related market in 2026.

### Findings (as reported)

- **LangChain** in **22.3%** of all agentic engineering jobs—about **2×** the next framework.
- **LangGraph** as its own category: **15.1%**; **27%** of listings **standalone** (per their breakdown—see article).
- **Pay:** LangChain-specific roles **$60k–$70k lower** at the **median** than framework-agnostic roles (article explains why).
- **Region:** **EU** = **66%** of LangChain-ecosystem jobs.
- **Employers:** **SAP**, **Mastercard**, **Citi**, **Accenture**, **Capgemini** among top employers.

### Reference

- Full article: [LangChain job market 2026](https://agentic-engineering-jobs.com/langchain-job-market-2026)

### Note

Authors invite questions on **data** and **methodology**.

---

## Micro LLMs, skills/tools, and keeping coordinators small

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

---

## Throughput, JIT micro-model caches, and “dictation vs editing”

*Continuation of the same broad micro-LLM / coordinator thread—paraphrased chat, not verified claims.*

### Many fast specialists, composed like a shell pipeline

Thought experiment: **many** micro LLMs (e.g. **~100**), each doing on the order of **~1000 tokens/s**, **orchestrated** by **spawning** sub-agents—**compose** them to finish **complex** work quickly without one monolithic model doing every subtask serially at low throughput.

### JIT train, LRU cache, forget the details in the big model

Pushed further: the **coordinator** **learns** a niche, **trains** (or distills into) a **micro model** for that skill, **uses** it, then **stops carrying** fine-grained procedure in the large model’s “head”—later it **reloads** the micro model for that task. **Just-in-time** training + an **LRU** (or similar) **cache** of small specialists. Goal: **offload parameters** and attention to work that is **“below”** heavy **reasoning**—the big model **thinks in concepts**, not **token-level syntax** (e.g. **paren placement**).

### What someone is already doing

One person described **sub-agents** with **dual VSM** files scoped to **upstream libraries** in their project, using **Haiku** or a **large Qwen** to **write code** while a stronger coordinator (**Opus** in their stack) **directs**—a living pattern close to the thought experiment.

### Dictation coordinator + file-type editor

Open design question: instead of “sub-agent writes code” generically, split **reasoning / dictation** from **mechanical text editing**—how far can a **coordinating** agent **dictate** while a **very small, very fast** editor model applies edits for **specific file types**? That split is framed as **actively interesting**, not solved.

### Cross-domain translation as the micro model’s job

Another concrete shape: a **file-type micro LLM** is mainly a **fuzzy translator** between **many external representations** (JSON, JS, TOML, XML, pseudocode, …) and **one internal** form (e.g. **Clojure / EDN**)—**and back**. The coordinator stays in **simple interchange** formats; the specialist owns **reader / compiler / ecosystem** edge cases—**completion** or **“code healer”** behavior with **deep local** knowledge. One quip: a huge slice of such a Clojure specialist could look like **“Clojure MCP–light”** (capabilities and facts as tight tooling, not only weights).

### Training anecdote (outcome pending)

Same conversation: someone reported being **~8M tokens** into a **~30B token** run training a **125M** parameter model, testing a **new attention** idea; **results not yet in** (order of **~8 days** from the message)—**anecdote only**, not an endorsement or benchmark.

---

*MESI entry: community note; job-market entry: agentic-engineering-jobs.com; micro-LLM entries: paraphrased informal discussion—not verified claims.*
