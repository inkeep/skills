---
title: Reference Context as Variables in Prompts
impact: HIGH
impactDescription: enables dynamic prompts
tags: context, variables, prompts, personalization
---

## Reference Context as Variables in Prompts

Reference headers and fetched context using `{{variable}}` syntax in sub agent instructions.

**Example:**

```typescript
const personalizedAgent = subAgent({
  id: 'personalized',
  name: 'Personalized Assistant',
  instructions: `
    You are helping {{headers.user-name}}, a {{context.user-profile.tier}} customer.
    
    Their account was created on: {{context.user-profile.createdAt}}
    Their recent orders: {{context.user-profile.recentOrders}}
    
    Provide personalized assistance based on their history and tier level.
    
    {{#if context.user-profile.isVip}}
    This is a VIP customer - prioritize their requests and offer premium support.
    {{/if}}
  `
})
```

Variable sources:
- `{{headers.xxx}}` - Request headers passed at invocation
- `{{context.xxx}}` - Data from context fetchers
- `{{credentials.xxx}}` - Credential values (use carefully)

This enables dynamic, personalized agent behavior without code changes.
