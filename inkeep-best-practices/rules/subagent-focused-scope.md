---
title: Keep Sub Agents Focused on Narrow Tasks
impact: CRITICAL
impactDescription: improves reliability and maintainability
tags: subagent, design, architecture, focus
---

## Keep Sub Agents Focused on Narrow Tasks

A sub agent should handle one well-defined responsibility. Broad, general-purpose sub agents become unpredictable and hard to debug.

**Incorrect (one sub agent does everything):**

```typescript
const doEverythingAgent = subAgent({
  id: 'general-agent',
  name: 'General Assistant',
  instructions: `
    You can help with customer support, process refunds, 
    look up orders, search documentation, send emails,
    create tickets, and anything else the user needs.
  `,
  canUse: () => [
    supportMcp,
    billingMcp,
    ordersMcp,
    docsMcp,
    emailMcp,
    ticketsMcp,
  ]
})
```

**Correct (specialized sub agents for each domain):**

```typescript
const supportAgent = subAgent({
  id: 'support-agent',
  name: 'Customer Support',
  instructions: `
    Help customers with support questions.
    Look up relevant documentation and provide helpful answers.
  `,
  canUse: () => [docsMcp, supportMcp]
})

const billingAgent = subAgent({
  id: 'billing-agent',
  name: 'Billing Specialist',
  instructions: `
    Handle billing inquiries and process refunds.
    Always verify the customer's identity first.
  `,
  canUse: () => [billingMcp, ordersMcp]
})
```

Focused sub agents are easier to test, debug, and iterate on. They also produce more predictable behavior.
