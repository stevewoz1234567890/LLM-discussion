# Throughput, JIT micro-model caches, and “dictation vs editing”

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
