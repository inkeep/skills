---
title: Transfer vs Delegate - Know the Difference
impact: CRITICAL
impactDescription: fundamental to multi-agent architecture
tags: relationship, transfer, delegate, multi-agent
---

## Transfer vs Delegate: Know the Difference

- **Transfer**: Hands off control completely. The receiving agent becomes the primary driver and can respond directly to the user.
- **Delegate**: Assigns a subtask. The child agent completes work and returns results to the parent, who then continues.

**Use transfer when:**
- The conversation topic has changed domains
- A specialist should take over completely
- The user needs to interact with a different capability

**Use delegate when:**
- You need information or work done as part of a larger task
- The parent agent should synthesize results
- The subtask is just one step in a workflow

**Example (transfer for domain handoff):**

```typescript
const routerAgent = subAgent({
  id: 'router',
  name: 'Router',
  instructions: `
    Route users to the appropriate specialist.
    Transfer to billing-agent for payment/subscription questions.
    Transfer to support-agent for technical questions.
  `,
  canTransferTo: () => [billingAgent, supportAgent]
})
```

**Example (delegate for subtask):**

```typescript
const researchAgent = subAgent({
  id: 'research',
  name: 'Research Agent',
  instructions: `
    Research topics thoroughly by gathering information from multiple sources.
    Delegate to web-search-agent for internet research.
    Delegate to docs-search-agent for internal documentation.
    Synthesize findings into a comprehensive response.
  `,
  canDelegateTo: () => [webSearchAgent, docsSearchAgent]
})
```
