# LangChain agents that “look right” can still be wrong (drift / trust demo)

**TL;DR:** The **LangChain** quickstart [reconai-langchain-quickstart](https://github.com/Trustbyrecon/reconai-langchain-quickstart) (**Trustbyrecon**) illustrates a common gap: the model’s **surface answer can seem reasonable** while an accompanying **trust** signal **drops**—i.e. plausibility ≠ safety or correctness for downstream reliance.

### What the demo is for

The author positions it as a **~5 minute** path from **clone** to an “aha”: seeing **good-enough-looking output** alongside **degraded trust** (their framing: **drift detection** in the example name).

### How to run it

From the repo root (after install steps in their README):

```bash
python examples/03_drift_detection/app.py
```

### Reference

- Repo: [github.com/Trustbyrecon/reconai-langchain-quickstart](https://github.com/Trustbyrecon/reconai-langchain-quickstart)

### Open question

Where do you draw the line between **UX** (“show a clean answer”) and **governance** (“surface uncertainty, drift, or retrieval mismatch”) so users do not over-trust fluent text?
