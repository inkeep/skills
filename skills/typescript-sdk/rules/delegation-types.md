---
title: "Types of Delegation Relationships"
description: "Sub agent, external agent, and team agent delegation"
topic-path: "typescript-sdk/agent-relationships"
---

# Types of Delegation Relationships

## Types of Delegation Relationships

### Sub Agent Delegation

Sub agent delegation is used to delegate a task to a sub agent as seen above.

### External Agent Delegation

External agent delegation is used to delegate a task to an [external agent](/typescript-sdk/external-agents).

```typescript
import { myExternalAgent } from "./external-agents/external-agent-example";

const mathSupervisor = subAgent({
  id: "supervisor",
  prompt: "Delegate to the external agent to calculate the answer to the question",
  canDelegateTo: () => [myExternalAgent],
});
```

You can also specify headers to include with every request to the external agent.

```typescript
const mathSupervisor = subAgent({
  id: "supervisor",
  prompt: "Delegate to the external agent to calculate the answer to the question",
  canDelegateTo: () => [myExternalAgent.with({ headers: { "authorization": "my-api-key" } })],
});
```

### Team Agent Delegation

Team agent delegation is used to delegate a task to another agent in the same project.

```typescript
import { myAgent } from "./agents/my-team-agent";

const mathSupervisor = subAgent({
  id: "supervisor",
  prompt: "Delegate to the team agent to calculate the answer to the question",
  canDelegateTo: () => [myAgent],
});
```

You can also specify headers to include with every request to the team agent.

```typescript
const mathSupervisor = subAgent({
  id: "supervisor",
  prompt: "Delegate to the team agent to calculate the answer to the question",
  canDelegateTo: () => [myAgent.with({ headers: { "authorization": "my-api-key" } })],
});
```
