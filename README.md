# LLM discussion

Curated notes and links on **LLM agents**, **frameworks** (LangChain, LangGraph), and **multi-agent coordination**—not a runnable application.

## What’s here

| Doc | What it is |
|-----|------------|
| **[stories/](stories/README.md)** | **Stories**—one file per topic; start here |
| **[TIL.md](TIL.md)** | Short pointer to the stories index (keeps old links working) |

### Stories ([index](stories/README.md))

1. **[MESI-style coherence for agents](stories/mesi-agent-coherence.md)** — Reducing redundant sync tokens when agents share artifacts (LangGraph `BaseStore`); paper and reference implementation linked.
2. **[2026 agentic engineering job market](stories/agentic-engineering-langchain-2026.md)** — LangChain / LangGraph share and salary/geo patterns from [agentic-engineering-jobs.com](https://agentic-engineering-jobs.com) (591 listings; see their article for methodology).
3. **[Micro LLMs, skills/tools, coordinators](stories/micro-llms-skills-coordinators-chat.md)** — Informal discussion: lazy tool loading, LoRA routing experiments, local “sudo-agent,” Unix-style tiny specialists.
4. **[Throughput, JIT caches, dictation vs editing](stories/micro-llms-throughput-jit-dictation-chat.md)** — Same discussion thread (continuation): many fast micro models composed as sub-agents, LRU-style JIT specialists, file-type translators / “MCP-light” shape, in-progress training anecdote.
5. **[Pre-inference prompt quality (PQS)](stories/pqs-prompt-quality-langchain.md)** — LangChain `PQSCallbackHandler` scores prompts before LLM calls (multi-dimensional rubric); links to SDK and hosted API.
6. **[LangGraph + LangConnect production stack](stories/langgraph-langconnect-production-stack.md)** — Next.js app, hosted LangGraph agent config, RAG on Cloud Run, Supabase OAuth, LangSmith/cost controls, Stripe + token economics (builder field report; [your-vehicle-ai.com](https://your-vehicle-ai.com)).
7. **[Plausible agent output vs trust (drift demo)](stories/recon-langchain-drift-trust-demo.md)** — LangChain quickstart where answers *look* fine while a trust signal flags drift; [reconai-langchain-quickstart](https://github.com/Trustbyrecon/reconai-langchain-quickstart).
8. **[Stanford 2026 AI Index](stories/stanford-ai-index-2026.md)** — Curated synopsis of the HAI report (423 pp.): benchmarks vs capability, U.S.–China, adoption, responsible AI, hardware, labor, medicine; links to [landing page](https://hai.stanford.edu/ai-index/2026-ai-index-report) and [PDF](https://hai.stanford.edu/assets/files/ai_index_report_2026.pdf).
9. **[Data center power to 2030 (Goldman Sachs)](stories/goldman-data-center-power-2030.md)** — Global DC electricity demand outlook (~1,350 TWh by 2030, +220% vs 2023, US vs RoW, US capacity to ~95 GW); [Goldman Sachs Research](https://www.goldmansachs.com/what-we-do/research) context.

Entries **(3)** and **(4)** are **paraphrased chat**, not fact-checked claims.

## Adding material

Add a new **`stories/<short-slug>.md`** file, then link it from **[stories/README.md](stories/README.md)** and the numbered list above. Keep one narrative or source per file so diffs stay small and stories are easy to share.
