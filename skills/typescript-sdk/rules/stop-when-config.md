---
title: "Configuring StopWhen"
description: "Control stopping conditions to prevent infinite loops in agents"
topic-path: "typescript-sdk/agent-settings"
---

# Configuring StopWhen

## Configuring StopWhen

Control stopping conditions to prevent infinite loops:

```typescript
// Agent level - limit transfers between Sub Agents
agent({
  id: "support-agent",
  stopWhen: {
    transferCountIs: 5  // Max transfers in one conversation
  },
});

// Sub Agent level - limit generation steps
subAgent({
  id: "my-sub-agent",
  stopWhen: {
    stepCountIs: 20  // Max tool calls + LLM responses
  },
});
```

**Configuration levels:**

* `transferCountIs`: Project or Agent level
* `stepCountIs`: Project or Sub Agent level

Settings inherit from Project → Agent → Sub Agent.
