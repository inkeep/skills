---
title: "Model Types Reference"
description: "When to use each model type in the configuration hierarchy"
topic-path: "typescript-sdk/models"
---

# Model Types Reference

## Model Types

| Type               | Purpose                       | Fallback                      |
| ------------------ | ----------------------------- | ----------------------------- |
| `base`             | Text generation and reasoning | **Required at project level** |
| `structuredOutput` | JSON/structured output only   | Falls back to `base`          |
| `summarizer`       | Summaries and status updates  | Falls back to `base`          |

Each model type inherits independently through the project → agent → sub agent hierarchy. `structuredOutput` and `summarizer` resolve to the most specific level that sets them, regardless of where `base` is configured — overriding only `base` at the agent or sub agent level does **not** change which `structuredOutput` or `summarizer` is used. They fall back to the resolved `base` only when no level sets them.
