---
title: "MCP Tool Authentication"
description: "Authentication methods for MCP servers including API keys, OAuth, and custom headers"
topic-path: "typescript-sdk/tools/mcp-tools"
---

# MCP Tool Authentication

## Authentication

If your MCP server requires authentication, we support various authentication methods (via credentials):

* **No Authentication** - For public APIs or internal services
* **API Key** - For services requiring API keys
* **Bearer Token** - For JWT or similar token-based authentication
* **Token Authentication** - For custom token schemes
* **OAuth Flows** - For standard OAuth 2.0 authentication
* **OAuth 2.1 Flows** - For modern OAuth 2.1 "1-click" authentication

For OAuth flows and OAuth 2.1 flows, it's recommended to use the [Visual Builder](/visual-builder/tools/mcp-servers) for easier configuration.

See [Credentials](/typescript-sdk/credentials/overview) for detailed examples and implementation guidance for each authentication type.

### Custom Headers

You can configure custom headers for your MCP server requests. Use credentials for sensitive information (API keys, tokens) and headers for non-sensitive metadata (user agent, version info, etc.).

```typescript
import { mcpTool } from "@inkeep/agents-sdk";

const customHeadersTool = mcpTool({
  id: "enterprise-api",
  name: "enterprise_data",
  description: "Enterprise API with custom headers",
  serverUrl: "https://enterprise.example.com/mcp",
  headers: {
    "User-Agent": "Inkeep-Agent/1.0",
    "X-Client-Version": "2024.1",
  },
});
```
