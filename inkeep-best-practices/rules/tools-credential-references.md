---
title: Reference Credentials by ID
impact: MEDIUM
impactDescription: improves security and portability
tags: tools, credentials, security, configuration
---

## Reference Credentials by ID

Use `credentialRef()` to reference credentials by ID. Actual credentials are resolved from environment files at push time.

**Incorrect (hardcoded credentials):**

```typescript
const myMcp = mcpServer({
  name: 'API',
  serverUrl: 'https://api.example.com/mcp',
  credential: {
    id: 'api-key',
    type: 'memory',
    value: 'sk-1234567890'  // Never do this!
  }
})
```

**Correct (credential reference):**

```typescript
const myMcp = mcpServer({
  name: 'API',
  serverUrl: 'https://api.example.com/mcp',
  credential: credentialRef('api-key')  // Resolved at push time
})
```

Benefits:
- Credentials never appear in code
- Different credentials per environment
- Centralized credential management
- Easier rotation and updates
