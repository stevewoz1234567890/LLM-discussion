# Pre-inference prompt quality scoring (PQS + LangChain)

**TL;DR:** [Prompt Quality Score (PQS)](https://pqs.onchainintel.net) exposes a **LangChain callback handler** that grades prompts **before** inference across several rubric dimensions (e.g. specificity, context, clarity, output format). The idea is to catch weak prompts **before** you spend tokens, instead of only noticing bad outputs afterward.

### The problem (as framed by the project)

Low-quality prompts often yield low-quality answers no matter how strong the model is. Without a **pre-flight** signal, improvement loops tend to start **after** a failed or mediocre completion—**after** tokens are burned. The maintainers also report that a very large share of real prompts score poorly on their standard (see their site for the exact claim and methodology).

### How it hooks in

`PQSCallbackHandler` plugs into LangChain’s **callback** system so each outgoing prompt can be scored automatically.

### Minimal usage

```python
from pqs_sdk import PQSCallbackHandler
from langchain_openai import ChatOpenAI

handler = PQSCallbackHandler(api_key="your-key", vertical="software")
llm = ChatOpenAI(callbacks=[handler])
```

You get a compact score/grade in logs (e.g. pass vs “optimize before proceeding”) before the model runs.

### Tiers (as advertised)

- **Scoring:** free tier for grading prompts (per their docs).
- **Optimization:** a separate endpoint can rewrite prompts toward higher scores; they cite a small **USDC** fee per optimization call—check current pricing on their site.

### References

- API / keys: [pqs.onchainintel.net](https://pqs.onchainintel.net)
- SDK: [github.com/OnChainAIIntel/pqs-sdk](https://github.com/OnChainAIIntel/pqs-sdk)
- Install: `pip install prompt-quality-score`

### Open question

For teams already using LangChain: is **pre-inference** scoring (vs retrieval checks, eval harnesses, or human review) worth the extra latency and dependency in your stack?
