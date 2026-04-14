# LLM discussion

Curated notes and links on **LLM agents**, **frameworks** (LangChain, LangGraph), and **multi-agent coordination**—not a runnable application.

## What’s here

| Doc | What it is |
|-----|------------|
| **[TIL.md](TIL.md)** | Short “today I learned” style notes with references where we have them |

### Topics in [TIL.md](TIL.md)

1. **MESI-style coherence for agents** — Reducing redundant sync tokens when agents share artifacts (LangGraph `BaseStore`); paper and reference implementation linked.
2. **2026 agentic engineering job market** — LangChain / LangGraph share and salary/geo patterns from [agentic-engineering-jobs.com](https://agentic-engineering-jobs.com) (591 listings; see their article for methodology).
3. **Micro LLMs, skills/tools, coordinators** — Informal discussion: lazy tool loading, LoRA routing experiments, local “sudo-agent,” Unix-style tiny specialists.
4. **Throughput, JIT caches, dictation vs editing** — Same discussion thread (continuation): many fast micro models composed as sub-agents, LRU-style JIT specialists, file-type translators / “MCP-light” shape, in-progress training anecdote.

Entries **(3)** and **(4)** are **paraphrased chat**, not fact-checked claims.

## Adding material

Append new sections to `TIL.md`, or split into more files and link them from this README if the doc grows unwieldy.
