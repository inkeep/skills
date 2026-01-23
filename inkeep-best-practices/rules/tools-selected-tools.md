---
title: Use selectedTools to Limit Exposed Tools
impact: HIGH
impactDescription: reduces tool confusion
tags: tools, mcp, configuration, selection
---

## Use selectedTools to Limit Exposed Tools

When an MCP server has many tools, use `selectedTools` to expose only what the agent needs.

**Incorrect (exposes all tools):**

```typescript
const supportAgent = subAgent({
  id: 'support',
  name: 'Support',
  instructions: '...',
  canUse: () => [githubMcp]  // Exposes all GitHub tools
})
```

**Correct (selective tool exposure):**

```typescript
const supportAgent = subAgent({
  id: 'support',
  name: 'Support',
  instructions: '...',
  canUse: () => [
    githubMcp.with({
      selectedTools: ['search_issues', 'get_issue', 'create_issue']
    })
  ]
})
```

Benefits of using `selectedTools`:
- Helps the agent focus on relevant tools
- Prevents accidental use of dangerous operations
- Reduces context window usage
- Improves tool selection accuracy
