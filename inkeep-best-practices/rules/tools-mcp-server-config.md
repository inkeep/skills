---
title: Configure MCP Servers Properly
impact: HIGH
impactDescription: affects tool availability and security
tags: tools, mcp, configuration, credentials
---

## Configure MCP Servers Properly

MCP servers should be configured with proper names, descriptions, and credentials.

**Incorrect (minimal configuration):**

```typescript
const myMcp = mcpServer({
  name: 'api',
  serverUrl: 'https://example.com/mcp'
})
```

**Correct (complete configuration):**

```typescript
const githubMcp = mcpServer({
  id: 'github-mcp',
  name: 'GitHub',
  description: 'Access GitHub repositories, issues, and pull requests',
  serverUrl: 'https://mcp.github.com/v1',
  credential: credential({
    id: 'github-token',
    name: 'GitHub Token',
    type: 'keychain',
    credentialStoreId: 'github-pat'
  })
})
```

A well-configured MCP server includes:
- Unique, stable ID
- Descriptive name
- Clear description of what it provides
- Appropriate credentials for authentication
- Transport configuration if non-default
