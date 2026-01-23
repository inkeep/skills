---
title: Avoid Unnecessary Multi-Agent Complexity
impact: MEDIUM
impactDescription: prevents over-engineering
tags: relationship, architecture, simplicity, design
---

## Avoid Unnecessary Multi-Agent Complexity

Don't create a multi-agent system when a single agent with the right tools will suffice. Multi-agent adds latency and complexity.

**Incorrect (over-engineered for a simple task):**

```typescript
// Unnecessary for a simple Q&A bot
const questionRouter = subAgent({ /* routes questions */ })
const questionAnalyzer = subAgent({ /* analyzes questions */ })
const answerGenerator = subAgent({ /* generates answers */ })
const answerValidator = subAgent({ /* validates answers */ })
const responseFormatter = subAgent({ /* formats responses */ })
```

**Correct (simple agent for simple task):**

```typescript
const qaAgent = subAgent({
  id: 'qa-agent',
  name: 'Q&A Agent',
  instructions: `
    Answer user questions using the documentation.
    Search for relevant information before answering.
  `,
  canUse: () => [docsSearchMcp]
})
```

Add multi-agent complexity only when you genuinely need:
- Different domains requiring specialized knowledge
- Parallel task execution
- Clear separation of concerns for maintainability
- Different permission levels for different operations
