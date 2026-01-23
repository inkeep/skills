---
title: Pass Request Context via Headers
impact: HIGH
impactDescription: enables personalization and authorization
tags: context, headers, personalization, authorization
---

## Pass Request Context via Headers

Headers allow the calling application to pass context to agents at request time.

**Example (passing headers when calling agent):**

```typescript
// When calling the agent API
const response = await agent.generate('Help me with my order', {
  customBodyParams: {
    headers: {
      'user-id': 'user_123',
      'session-id': 'sess_abc',
      'customer-tier': 'gold'
    }
  }
})
```

**Using headers in sub agent instructions:**

```typescript
const personalizedAgent = subAgent({
  id: 'personalized',
  name: 'Personalized Assistant',
  instructions: `
    You are helping a {{headers.customer-tier}} tier customer.
    User ID: {{headers.user-id}}
    
    Provide service appropriate to their tier level.
  `
})
```

Headers are useful for:
- User identification
- Session tracking
- Authorization context
- Feature flags
- Personalization data
