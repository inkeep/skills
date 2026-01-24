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
