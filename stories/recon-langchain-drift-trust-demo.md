# ReconAI — trust signals when agent output “looks right”

**TL;DR:** **ReconAI** frames a gap many agent builders hit: **fluent, plausible answers** can still be **wrong or risky** for production. They ship both a **LangChain** Python quickstart and a **JavaScript `@reconai/sdk`** with **`guard()`** so **scoring and signals** can surface **before** bad runs ship.

---

## JavaScript SDK + CLI (Reflex dashboard on real runs)

As described in their **SDK + CLI** flow:

### Quickstart

```bash
npm install @reconai/sdk
npx reconai demo
npx reconai explain
```

### Demo trust spectrum

The **demo** is said to walk a full **trust spectrum**: **healthy baseline** → **false confidence** (the “looks fine at first glance” case) → **tool misuse**.

### `guard()` and the Reflex dashboard

**Reflex Dashboard** is positioned as **no longer demo-only**: you can **render the same dashboard** from a **real `guard()` result** in your own execution path—not only inside the CLI demo.

```javascript
import { guard } from "@reconai/sdk";

await guard({ agentId, actionType, toolName, signals }, async () => {
  return yourCall();
});
```

Same **scoring**, **signals**, and **dashboard** on **real runs**; stated goal is to **catch runs that look correct** before they become **production issues**.

---

## LangChain quickstart (Python)

The **LangChain** quickstart [reconai-langchain-quickstart](https://github.com/Trustbyrecon/reconai-langchain-quickstart) (**Trustbyrecon**) is a **~5 minute** path from **clone** to an “aha”: **reasonable-looking output** alongside a **dropping trust** signal (their framing: **drift detection** in the example name).

From the repo root (after install steps in their README):

```bash
python examples/03_drift_detection/app.py
```

- Repo: [github.com/Trustbyrecon/reconai-langchain-quickstart](https://github.com/Trustbyrecon/reconai-langchain-quickstart)

---

### Open question

Where do you draw the line between **UX** (“show a clean answer”) and **governance** (“surface uncertainty, drift, or retrieval mismatch”) so users do not over-trust fluent text?

### Note

Third-party **product and repos**; verify **pricing**, **terms**, and **telemetry** in their docs before adopting in regulated or sensitive workloads.
