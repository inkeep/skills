---
title: "Model Provider Options"
description: "Configuration options for different model providers"
topic-path: "typescript-sdk/models"
---

# Model Provider Options

## Provider Options

Inkeep Agents supports all [Vercel AI SDK provider options](https://ai-sdk.dev/providers/ai-sdk-providers/).

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
            thinking: { type: 'enabled', budgetTokens: 8000 },
            temperature: 0.5
          }
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
