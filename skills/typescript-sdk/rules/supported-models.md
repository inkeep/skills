---
title: "Supported Models Reference"
description: "List of supported model providers and their API keys"
topic-path: "typescript-sdk/models"
---

# Supported Models Reference

## Supported Models

| Provider                     | Example Models                                                                                                                           | API Key                        |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| **Anthropic**                | `anthropic/claude-sonnet-4-5`<br />`anthropic/claude-haiku-4-5`<br />`anthropic/claude-opus-4-6`                                         | `ANTHROPIC_API_KEY`            |
| **OpenAI**                   | `openai/gpt-5.2`<br />`openai/gpt-5.1`<br />`openai/gpt-4.1`<br />`openai/gpt-4.1-mini`<br />`openai/gpt-4.1-nano`<br />`openai/gpt-5`\* | `OPENAI_API_KEY`               |
| **Azure OpenAI**             | `azure/my-gpt4-deployment`<br />`azure/my-gpt35-deployment`                                                                              | `AZURE_API_KEY`                |
| **Google**                   | `google/gemini-2.5-flash`<br />`google/gemini-2.5-flash-lite`                                                                            | `GOOGLE_GENERATIVE_AI_API_KEY` |
| **OpenRouter**               | `openrouter/anthropic/claude-sonnet-4-0`<br />`openrouter/meta-llama/llama-3.1-405b`                                                     | `OPENROUTER_API_KEY`           |
| **Gateway**                  | `gateway/openai/gpt-4.1-mini`                                                                                                            | `AI_GATEWAY_API_KEY`           |
| **NVIDIA NIM**               | `nim/nvidia/llama-3.3-nemotron-super-49b-v1.5`<br />`nim/nvidia/nemotron-4-340b-instruct`                                                | `NIM_API_KEY`                  |
| **Custom OpenAI-compatible** | `custom/my-custom-model`<br />`custom/llama-3-custom`                                                                                    | `CUSTOM_LLM_API_KEY`           |
| **Mock**                     | `mock/default`                                                                                                                           | None required                  |

<Note>\*`openai/gpt-5`, `openai/gpt-5-mini`, and `openai/gpt-5-nano` require a verified OpenAI organization. If your organization is not yet verified, these models will not be available.</Note>

### Pinned vs Unpinned Models

**Pinned models** include a specific date or version (e.g., `anthropic/claude-sonnet-4-20250514`) and always use that exact version.

**Unpinned models** use generic identifiers (e.g., `anthropic/claude-sonnet-4-5`) and let the provider choose the latest version, which may change over time as providers update their models.

```typescript
models: {
  base: {
    model: "anthropic/claude-sonnet-4-5",  // Unpinned - provider chooses version
    // vs
    model: "anthropic/claude-sonnet-4-20250514"  // Pinned - exact version
  }
}
```

The TypeScript SDK also provides constants for common models:

```typescript
import { Models } from "@inkeep/agents-sdk";

models: {
  base: {
    model: Models.ANTHROPIC_CLAUDE_SONNET_4_5,  // Type-safe constants
  }
}
```
