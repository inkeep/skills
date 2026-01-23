---
title: Delegates Cannot Respond to Users
impact: HIGH
impactDescription: affects conversation flow design
tags: relationship, delegate, conversation, design
---

## Delegates Cannot Respond to Users

A child sub agent (delegate) cannot respond directly to a user. It returns results to the parent agent, who decides what to share.

**Incorrect mental model:**

```
User → Parent Agent → Delegate Agent → User (WRONG!)
```

**Correct mental model:**

```
User → Parent Agent → Delegate Agent → Parent Agent → User
```

**Correct (delegate returns data to parent):**

```typescript
const dataFetchAgent = subAgent({
  id: 'data-fetch',
  name: 'Data Fetcher',
  instructions: `
    Fetch requested data and return it in a structured format.
    Do not attempt to explain or contextualize - just return the data.
    The calling agent will handle user communication.
  `,
  canUse: () => [databaseMcp]
})

const orchestratorAgent = subAgent({
  id: 'orchestrator',
  name: 'Orchestrator',
  instructions: `
    Help users understand their data.
    Delegate to data-fetch to get raw data.
    Explain the results to the user in a friendly way.
  `,
  canDelegateTo: () => [dataFetchAgent]
})
```

Design your delegates to return structured information that the parent can use and present appropriately.
