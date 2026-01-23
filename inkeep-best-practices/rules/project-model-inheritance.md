---
title: Configure Models at Project Level
impact: MEDIUM
impactDescription: enables inheritance
tags: project, models, configuration, inheritance
---

## Configure Models at Project Level

Set default models at the project level. Agents and sub agents inherit these unless overridden.

**Example:**

```typescript
const myProject = project({
  id: 'my-project',
  name: 'My Project',
  models: {
    base: { model: 'gpt-4.1-mini' },
    structuredOutput: { model: 'gpt-4.1' },
    summarizer: { model: 'gpt-4.1-mini' }
  },
  agents: () => [myAgent]
})
```

**Model types:**
- `base` - Default model for conversation
- `structuredOutput` - Model for generating structured data
- `summarizer` - Model for summarizing conversation history

**Override at agent level when needed:**

```typescript
const complexAgent = agent({
  id: 'complex-agent',
  name: 'Complex Reasoning Agent',
  defaultSubAgent: reasoningSubAgent,
  models: {
    base: { model: 'gpt-4.1' }  // Override with more capable model
  }
})
```

This pattern allows:
- Consistent defaults across the project
- Easy model upgrades in one place
- Selective overrides for specific agents
