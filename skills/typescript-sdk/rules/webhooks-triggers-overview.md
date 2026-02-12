---
title: "Triggers Overview"
description: "Creating webhook endpoints to invoke agents from external services"
topic-path: "typescript-sdk/triggers/webhooks"
---

# Triggers Overview

## Overview

Triggers are useful for:

* **Event-driven workflows** - Respond to events from external services (GitHub issues, Stripe payments, etc.)
* **Third-party integrations** - Connect services that can send webhooks to your agents
* **Automated tasks** - Trigger agent actions based on external events
* **Custom applications** - Allow your own services to invoke agents via HTTP

## Creating Triggers

### Basic Trigger

```typescript
import { agent, trigger, subAgent } from "@inkeep/agents-sdk";

const slackTrigger = trigger({
  name: "Slack Messages",
  description: "Handle incoming Slack messages",
  messageTemplate: "New message from {{user.name}}: {{text}}",
});

export default agent({
  id: "support-agent",
  name: "Support Agent",
  defaultSubAgent: subAgent({
    id: "support",
    name: "Support Assistant",
    prompt: "You help answer support questions.",
  }),
  triggers: () => [slackTrigger],
});
```

### Trigger with Structured Data Only

If you don't need a human-readable text message, you can omit `messageTemplate` and the agent will receive only the structured payload data:

```typescript
const dataTrigger = trigger({
  name: "Data Webhook",
  description: "Receives structured data for processing",
  // No messageTemplate - agent receives only the data part
  inputSchema: z.object({
    eventType: z.string(),
    payload: z.record(z.unknown()),
  }),
});
```

The agent will receive:

```json
{
  "parts": [
    { "kind": "data", "data": { "eventType": "order.created", "payload": { ... } }, "metadata": { "source": "trigger" } }
  ]
}
```

### Trigger with Input Validation

Use JSON Schema or Zod to validate incoming webhook payloads:

```typescript
import { z } from "zod";

const githubWebhookTrigger = trigger({
  name: "GitHub Events",
  description: "Handle GitHub webhook events",
  inputSchema: z.object({
    action: z.string(),
    repository: z.object({
      full_name: z.string(),
    }),
    sender: z.object({
      login: z.string(),
    }),
  }),
  messageTemplate:
    "GitHub event: {{action}} on {{repository.full_name}} by {{sender.login}}",
  signingSecret: process.env.GITHUB_WEBHOOK_SECRET,
});
```

### Trigger with Authentication

Triggers support flexible header-based authentication. You can require any headers with expected values:

<Tabs>
  <Tab title="Single Header">
    ```typescript
    const apiKeyTrigger = trigger({
      name: "Authenticated Webhook",
      messageTemplate: "Webhook received: {{message}}",
      authentication: {
        headers: [
          { name: "X-API-Key", value: process.env.WEBHOOK_API_KEY! },
        ],
      },
    });
    ```
  </Tab>

  <Tab title="Multiple Headers">
    ```typescript
    const multiAuthTrigger = trigger({
      name: "Multi-Header Auth Webhook",
      messageTemplate: "Webhook received: {{message}}",
      authentication: {
        headers: [
          { name: "X-API-Key", value: process.env.WEBHOOK_API_KEY! },
          { name: "X-Client-ID", value: process.env.WEBHOOK_CLIENT_ID! },
        ],
      },
    });
    ```
  </Tab>

  <Tab title="Authorization Header">
    ```typescript
    // For Bearer token or Basic auth, use the Authorization header
    const bearerTrigger = trigger({
      name: "Bearer Auth Webhook",
      messageTemplate: "Webhook received: {{message}}",
      authentication: {
        headers: [
          { 
            name: "Authorization", 
            value: `Bearer ${process.env.WEBHOOK_BEARER_TOKEN!}` 
          },
        ],
      },
    });
    ```
  </Tab>
</Tabs>

<Note>
  Header values are securely hashed before storage. The original values are never stored in the database.
</Note>
