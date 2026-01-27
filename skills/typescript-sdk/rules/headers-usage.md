---
title: "Using Headers in Agents"
description: "How to use header values in prompts, context fetchers, and external tools"
topic-path: "typescript-sdk/headers"
---

# Using Headers in Agents

## Using headers in your agents

Header values can be used in your agent prompts and fetch definitions using JSONPath template syntax `{{headers.field_name}}`.
You can use the headers schema's `toTemplate()` method for type-safe templating with autocomplete and validation.

### In Context Fetchers

Use header values to fetch dynamic data from external APIs:

```typescript
// Define schema for expected headers (use lowercase keys)
const personalAgentHeaders = headers({
  schema: z.object({
    user_id: z.string(),
    auth_token: z.string(),
    org_name: z.string().optional()
  });
});


const userDataFetcher = fetchDefinition({
  id: "user-data",
  name: "User Data",
  fetchConfig: {
    url: `https://api.example.com/users/${personalAgentHeaders.toTemplate('user_id')}/profile`,
    headers: {
      Authorization: `Bearer ${personalAgentHeaders.toTemplate('auth_token')}`,
      "X-Organization": personalAgentHeaders.toTemplate('org_name')
    },
    body: {
      includePreferences: true,
      userId: personalAgentHeaders.toTemplate('user_id')
    }
  },
  responseSchema: z.object({
    name: z.string(),
    preferences: z.record(z.unknown())
  })
});

// Configure context for your Agent
// You must include the headers schema and fetchers in your context config.
const personalAgentContext = contextConfig({
  headers: personalAgentHeaders,
  contextVariables: {
    user: userFetcher,
  },
});
```

### In Agent Prompts

Reference context directly in agent prompts for personalization using the context config's template method:

```typescript
// Create context config with both headers and fetchers
const userContext = contextConfig({
  headers: requestHeaders,
  contextVariables: {
    userName: userDataFetcher,
  },
});

const assistantAgent = subAgent({
  prompt: `You are an assistant for ${userContext.toTemplate('userName')} from ${requestHeaders.toTemplate('org_name')}.

  User context:
  - ID: ${requestHeaders.toTemplate('user_id')}
  - Organization: ${requestHeaders.toTemplate('org_name')}

  Provide help tailored to their organization's needs.`
});
```

### In External Tools

Configure external agents or MCP servers with dynamic headers using the headers schema:

```typescript
// Define schema for expected headers (use lowercase keys)
const personalAgentHeaders = headers({
  schema: z.object({
    user_id: z.string(),
    auth_token: z.string(),
    org_name: z.string().optional()
  });
});

// Configure external agent
const externalAgent = externalAgent({
  id: "external-service",
  baseUrl: "https://external.api.com",
  headers: {
    Authorization: `Bearer ${personalAgentHeaders.toTemplate('auth_token')}`,
    "X-User-Context": personalAgentHeaders.toTemplate('user_id'),
    "X-Org": personalAgentHeaders.toTemplate('org_name')
  }
});

// Configure context for your Agent with your headers schema.
const personalAgentContext = contextConfig({
  headers: personalAgentHeaders,
});
```
