---
title: "Passing Context via Headers"
description: "How to pass dynamic context to agents via HTTP headers"
topic-path: "typescript-sdk/headers"
---

# Passing Context via Headers

## Passing context via headers

Include context values as HTTP headers when calling your agent API. These headers are validated against your configured schema and cached for the conversation.

```bash
curl -N \
  -X POST "http://localhost:3003/api/chat" \
  -H "Authorization: Bearer $INKEEP_API_KEY" \
  -H "user_id: u_123" \
  -H "auth_token: t_abc" \
  -H "org_name: Acme Corp" \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [ { "role": "user", "content": "What can you help me with?" } ],
    "conversationId": "conv-123"
  }'
```

<Note>
  Header keys are normalized to lowercase. Define them as lowercase in your schema and reference them as lowercase in templates.
</Note>

## Configuring headers

Define a schema for your headers and configure how it's used in your agent.
You must include the headers schema in your context config.

```typescript
import { z } from "zod";
import { agent, subAgent } from "@inkeep/agents-sdk";
import { contextConfig, fetchDefinition, headers } from '@inkeep/agents-core';


// Define schema for expected headers (use lowercase keys)
const personalAgentHeaders = headers({
  schema: z.object({
    user_id: z.string(),
    auth_token: z.string(),
    org_name: z.string().optional()
  });
});

// Create a context fetcher that uses header values with type-safe templating
const userFetcher = fetchDefinition({
  id: "user-info",
  name: "User Information",
  trigger: "initialization",
  fetchConfig: {
    url: `https://api.example.com/users/${personalAgentHeaders.toTemplate('user_id')}`,
    method: "GET",
    headers: {
      Authorization: `Bearer ${personalAgentHeaders.toTemplate('auth_token')}`,
    },
    transform: "user", // Extract user from response, for example if the response is { "user": { "name": "John Doe", "email": "john.doe@example.com" } }, the transform will return the user object
  },
  responseSchema: z.object({
    user: z.object({
      name: z.string(),
      email: z.string(),
    }),
  }),
  defaultValue: "Guest User"
});

// Configure context for your agent
const personalAgentContext = contextConfig({
  headers: personalAgentHeaders,
  contextVariables: {
    user: userFetcher,
  },
});

// Create a Sub Agent that uses context variables
const personalAssistant = subAgent({
  id: "personal-assistant",
  name: "Personal Assistant",
  description: "Personalized AI assistant",
  prompt: `You are a helpful assistant for ${personalAgentContext.toTemplate('user.name')} from ${personalAgentHeaders.toTemplate('org_name')}.

  User ID: ${personalAgentHeaders.toTemplate('user_id')}

  Provide personalized assistance based on their context.`,
});

// Attach context to your Agent
const myAgent = agent({
  id: "personal-agent",
  name: "Personal Assistant Agent",
  defaultSubAgent: personalAssistant,
  subAgents: () => [personalAssistant],
  contextConfig: personalAgentContext,
});
```

## How it works

<Steps>
  <Step>
    **Validation**: Headers are validated against your configured schema when a request arrives
  </Step>

  <Step>
    **Caching**: Validated context is cached per conversation for reuse across multiple interactions
  </Step>

  <Step>
    **Reuse**: Subsequent requests with the same `conversationId` automatically use cached context values
  </Step>

  <Step>
    **Updates**: Provide new header values to update the context for an ongoing conversation
  </Step>
</Steps>

<Tip>
  Context values persist across conversation turns. To update them, send new header values with the same conversation ID.
</Tip>
