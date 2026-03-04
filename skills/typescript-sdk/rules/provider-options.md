---
title: "Model Provider Options"
description: "Configuration options for different model providers"
topic-path: "typescript-sdk/models"
---

# Model Provider Options

## Provider Options

Inkeep Agents supports all [Vercel AI SDK provider options](https://ai-sdk.dev/providers/ai-sdk-providers/).

### How providerOptions works

`providerOptions` accepts two types of values:

* **Scalars** (`temperature`, `topP`, `maxOutputTokens`, `seed`, `maxDuration`) — standard generation parameters applied to every call
* **Objects** (`anthropic: {}`, `openai: {}`, `gateway: {}`, etc.) — provider-specific options for that provider

This means you can mix them freely:

```typescript
providerOptions: {
  temperature: 0.7,            // generation param
  anthropic: {                 // Anthropic-specific options
    thinking: { type: 'enabled', budgetTokens: 8000 }
  }
}
```

<Note>
  Constructor-level config (`baseURL`, `headers`, `resourceName`, `apiVersion`) is always specified at the top level of `providerOptions`, not nested under a provider key.
</Note>

### Complete Examples

**Basic configuration:**

<Tabs>
  <Tab title="TypeScript">
    ```typescript
    models: {
      base: {
        model: "anthropic/claude-sonnet-4-5",
        providerOptions: {
          maxOutputTokens: 4096,
          temperature: 0.7,
          topP: 0.95,
          seed: 12345,
          maxDuration: 30,
        }
      }
    }
    ```
  </Tab>

  <Tab title="JSON">
    ```json
    {
      "maxOutputTokens": 4096,
      "temperature": 0.7,
      "topP": 0.95,
      "seed": 12345,
      "maxDuration": 30
    }
    ```
  </Tab>
</Tabs>

**OpenAI with reasoning:**

<Tabs>
  <Tab title="TypeScript">
    ```typescript
    models: {
      base: {
        model: "openai/o1-preview",
        providerOptions: {
          openai: { reasoningEffort: 'medium' }, // 'low' | 'medium' | 'high'
          maxOutputTokens: 4096
        }
      }
    }
    ```
  </Tab>

  <Tab title="JSON">
    ```json
    {
      "openai": { "reasoningEffort": "medium" },
      "maxOutputTokens": 4096
    }
    ```
  </Tab>
</Tabs>

**Anthropic with thinking:**

<Tabs>
  <Tab title="TypeScript">
    ```typescript
    models: {
      base: {
        model: "anthropic/claude-sonnet-4-5",
        providerOptions: {
          anthropic: {
            thinking: { type: 'enabled', budgetTokens: 8000 }
          },
          temperature: 0.5
        }
      }
    }
    ```
  </Tab>

  <Tab title="JSON">
    ```json
    {
      "anthropic": {
        "thinking": { "type": "enabled", "budgetTokens": 8000 }
      },
      "temperature": 0.5
    }
    ```
  </Tab>
</Tabs>

**Google with thinking:**

<Tabs>
  <Tab title="TypeScript">
    ```typescript
    models: {
      base: {
        model: "google/gemini-2.5-flash",
        providerOptions: {
          google: {
            thinkingConfig: { thinkingBudget: 8192, includeThoughts: true }
          },
          temperature: 0.7
        }
      }
    }
    ```
  </Tab>

  <Tab title="JSON">
    ```json
    {
      "google": {
        "thinkingConfig": { "thinkingBudget": 8192, "includeThoughts": true }
      },
      "temperature": 0.7
    }
    ```
  </Tab>
</Tabs>

**Vercel AI Gateway with model routing:**

The Gateway provider supports routing requests across multiple models with automatic fallback. If the primary model fails or is unavailable, the gateway tries the next model in the list.

<Tabs>
  <Tab title="TypeScript">
    ```typescript
    models: {
      base: {
        model: "gateway/openai/gpt-4.1",
        providerOptions: {
          gateway: {
            models: ["openai/gpt-4.1", "anthropic/claude-sonnet-4-5", "google/gemini-3.1-flash-lite-preview"],  // Try in order
          }
        }
      }
    }
    ```
  </Tab>

  <Tab title="JSON">
    ```json
    {
      "gateway": {
        "models": ["openai/gpt-4.1", "anthropic/claude-sonnet-4-5", "google/gemini-3.1-flash-lite-preview"]
      }
    }
    ```
  </Tab>
</Tabs>

<Note>
  All models in the `models` array must be valid [Vercel AI Gateway model IDs](https://ai-sdk.dev/providers/ai-sdk-providers/ai-gateway). The gateway falls through to the next model on failure — if all models fail, the request errors. Set `AI_GATEWAY_API_KEY` in your environment for authentication.
</Note>

**Azure OpenAI:**

<Tabs>
  <Tab title="TypeScript">
    ```typescript
    models: {
      base: {
        model: "azure/my-gpt4-deployment",
        providerOptions: {
          resourceName: "my-azure-openai-resource",  // Required
          apiVersion: "2024-10-21",  // Optional
          temperature: 0.7
        }
      }
    }
    ```
  </Tab>

  <Tab title="JSON">
    ```json
    {
      "resourceName": "my-azure-openai-resource",
      "apiVersion": "2024-10-21",
      "temperature": 0.7
    }
    ```
  </Tab>
</Tabs>

<Note>
  Azure OpenAI **requires** either `resourceName` (for standard Azure OpenAI deployments) or `baseURL` (for custom endpoints) in `providerOptions`. The `AZURE_API_KEY` environment variable must be set for authentication. Note that only one Azure OpenAI resource can be used at a time since authentication is handled via a single environment variable.
</Note>

**Custom OpenAI-compatible provider:**

<Tabs>
  <Tab title="TypeScript">
    ```typescript
    models: {
      base: {
        model: "custom/my-custom-model",
        providerOptions: {
          baseURL: "https://api.example.com/v1",  // Required
          temperature: 0.7
        }
      }
    }
    ```
  </Tab>

  <Tab title="JSON">
    ```json
    {
      "baseUrl": "http://127.0.0.1:8090/v1",
      "temperature": 0.7
    }
    ```
  </Tab>
</Tabs>

<Note>
  Custom OpenAI-compatible providers **require** a base URL to be specified in `providerOptions.baseURL` or `providerOptions.baseUrl`. The `CUSTOM_LLM_API_KEY` environment variable will be automatically used for authentication if present.
</Note>

### Context Window Override

For custom or unlisted models, you can explicitly specify the context window size:

<Tabs>
  <Tab title="TypeScript">
    ```typescript
    models: {
      base: {
        model: "custom/my-model",
        providerOptions: {
          contextWindowSize: 100000,  // Context window in tokens
          baseURL: "https://api.example.com/v1"
        }
      }
    }
    ```
  </Tab>

  <Tab title="JSON">
    ```json
    {
      "contextWindowSize": 100000,
      "baseUrl": "https://api.example.com/v1"
    }
    ```
  </Tab>
</Tabs>

<Note>
  The `contextWindowSize` option is useful when:

  * Using a custom model not in the built-in registry
  * The framework incorrectly detects the context window size
  * You want to artificially limit the context window for testing

  This affects compression triggers and oversized artifact detection (artifacts exceeding 30% of the context window).
</Note>
