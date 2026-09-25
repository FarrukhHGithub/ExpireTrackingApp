---
trigger: always_on
---

# Graphify (token control)

- Before reading full files or grepping to understand existing code, run:
  `python -m graphify query "<question>"` against `graphify-out/graph.json`
- Only read full files if the query result is insufficient or ambiguous.
- After structural changes (new models, screens, routes), run in order:
  1. `python -m graphify . --code-only --update`
  2. `python -m graphify cluster-only .`
- This applies by default. Do not wait for explicit instruction to use it.
