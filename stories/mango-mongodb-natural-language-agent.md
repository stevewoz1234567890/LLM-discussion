# Mango — natural-language queries for MongoDB

**Mango** is an **open-source AI agent** for querying **MongoDB** in **plain English**.

### Why not “text-to-SQL”

The usual **text-to-SQL** pattern maps poorly to MongoDB: **no DDL** in the SQL sense, **nested documents**, **aggregation pipelines**, and other modeling choices mean the problem is **structurally different** from relational text-to-SQL.

### Design (as described)

- **No LangChain** — custom stack with a **provider-agnostic LLM** interface, a **pluggable tool registry**, and **ChromaDB** for memory that **learns from past queries** and surfaces them as **few-shot** context for new questions.

### Try it

```bash
pip install "mango-ai[anthropic]"
mango
```

### References

- **GitHub:** [github.com/FrancescoBellingeri/mango](https://github.com/FrancescoBellingeri/mango)
- **Docs:** [mango.francescobellingeri.com](https://mango.francescobellingeri.com)
- **Colab quickstart:** [notebooks/mango_quickstart.ipynb on Colab](https://colab.research.google.com/github/francescobellingeri/mango/blob/main/notebooks/mango_quickstart.ipynb)

### Note

Third-party **open-source** project; verify behavior on your data and review security before connecting to production clusters.
