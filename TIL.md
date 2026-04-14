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

*MESI entry: community note; job-market entry: figures and claims as stated by agentic-engineering-jobs.com—see the article for definitions and methods.*
