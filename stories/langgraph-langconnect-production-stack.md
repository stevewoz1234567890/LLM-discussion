# LangGraph agent + LangConnect RAG on public cloud (field report)

**TL;DR:** One shipping line described a **12+ month** path from design to **public cloud**: a **LangGraph**–hosted **runnable agent config** behind a **Next.js** web app, **LangConnect**–backed **RAG** on **Google Cloud Run**, **production Supabase** (OAuth and security), and **LangSmith** for tracing and **cost-focused** tuning—framed as following **LangChain’s Open Agent Platform (OAP)**–style shape.

### Architecture (as reported)

- **Agent surface:** LangGraph **hosted** configuration, exposed to the product as a runnable agent.
- **Client:** Next.js frontend (and **Next.js server** routes for sensitive backend work).
- **RAG:** LangConnect service on **Cloud Run**, with **Cloud SQL for PostgreSQL** plus a **vector** store option; **per-user**, **OAuth-secured** ingestion / digest paths described as designed **for scale**.
- **Identity / data:** **Supabase** auth and security patterns described as **full production** posture.
- **Observability / economics:** **LangSmith** tracing and explicit **cost optimization** work; **OpenAI** usage split for **platform OpEx** visibility; LLM tier described as **GPT-4.1** at a “layer 2” in the stack.
- **Monetization:** **Stripe**; token billing with a stated **~200%** markup over **retail token** cost, with **server-side** balance / accounting guarded in the Next backend (verify live behavior on the product—numbers are self-reported).

### Delivery and QA stance

- **Status:** **production beta** at time of note.
- **Testing:** **TDD** with **tests running against production** (the builder argues this is increasingly workable with the right guardrails).

### Tooling

Much of the stack was built with **Codex** in **VS Code**, per the author—anecdotal, not a benchmark of tools.

### Reference

- Live site (for architecture-curious readers): [your-vehicle-ai.com](https://your-vehicle-ai.com)

### Note

This section is a **compressed summary of a builder’s account**, not an independent audit. Economics, security claims, and uptime should be validated against the running product and their docs. The author also described moving from **production beta** toward **full launch** and having **more weekly capacity for new engagements** as that transition settles—use the site contact if that is relevant to you.

### Open question

How are teams **partitioning** LangGraph deployments, LangConnect indices, and **per-tenant** RAG isolation when **Cloud Run** + **managed Postgres** are in the loop—what broke first at scale?
