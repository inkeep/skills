---
title: "When to Use Transfers vs Delegation"
description: "Guidelines for choosing between transfer and delegation relationships"
topic-path: "typescript-sdk/agent-relationships"
---

# When to Use Transfers vs Delegation

## When to Use Each Relationship

### Use Transfers for Complex Tasks

Use `canTransferTo` when the task is complex and the user will likely want to ask follow-up questions to the specialized Sub Agent:

* **Customer support conversations** - User may have multiple related questions
* **Technical troubleshooting** - Requires back-and-forth interaction
* **Order management** - User might want to modify, track, or ask about multiple aspects
* **Product consultations** - Users often have follow-up questions

### Use Delegation for Simple Tasks

Use `canDelegateTo` when the task is simple and self-contained:

* **Data retrieval** - Get a specific piece of information and return it
* **Calculations** - Perform a computation and return the result
* **Single API calls** - Make one external request and return the data
* **Simple transformations** - Convert data from one format to another

```typescript
// TRANSFER: User will likely have follow-up questions about their order
const routerSubAgent = subAgent({
  id: "router",
  prompt: "For order inquiries, transfer to order specialist",
  canTransferTo: () => [orderSubAgent],
});

// DELEGATION: Just need a quick calculation, then continue
const mathSupervisor = subAgent({
  id: "supervisor",
  prompt: "Delegate to number producers, then add results together",
  canDelegateTo: () => [numberProducerA, numberProducerB],
});
```
