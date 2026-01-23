---
title: Use Explicit Stable IDs
impact: MEDIUM
impactDescription: ensures consistency across deployments
tags: subagent, ids, deployment, best-practice
---

## Use Explicit Stable IDs

Always provide explicit, stable IDs for agents and sub agents. This ensures consistency when pushing/pulling between code and the Visual Builder.

**Incorrect (relying on auto-generated IDs):**

```typescript
// No id specified - will be auto-generated and may change
const myAgent = subAgent({
  name: 'Support Agent',
  instructions: '...'
})
```

**Correct (explicit stable ID):**

```typescript
const myAgent = subAgent({
  id: 'support-agent-v1',  // Explicit, stable ID
  name: 'Support Agent',
  instructions: '...'
})
```

Use meaningful, versioned IDs that clearly identify the agent's purpose. This enables:
- Reliable push/pull synchronization
- Clear tracking of agent versions
- Easier debugging and monitoring
