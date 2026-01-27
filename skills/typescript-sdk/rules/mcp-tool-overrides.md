---
title: "MCP Tool Overrides"
description: "Customize how individual MCP tools appear to agents"
topic-path: "typescript-sdk/tools/mcp-tools"
---

# MCP Tool Overrides

## Tool Overrides

You can customize how individual tools from an MCP server appear to agents using tool overrides. This is useful for simplifying complex tools or providing better names and descriptions:

```typescript
import { mcpTool } from "@inkeep/agents-sdk";
import { z } from "zod";

const apiTool = mcpTool({
  id: "complex-api-tool",
  name: "enterprise_api",
  description: "Enterprise API with many complex tools",
  serverUrl: "https://api.enterprise.com/mcp",
  toolOverrides: {
    // Override a specific tool by its original name
    "fetch_user_analytics_data_with_filters": {
      displayName: "get_user_stats", // Simpler name for agents
      description: "Get user statistics and analytics data", // Clearer description
      schema: z.object({
        userId: z.string().describe("User ID to fetch stats for"),
        timeframe: z.enum(["daily", "weekly", "monthly"]).describe("Time period for stats")
      })
    }
  }
});
```

### Tool Override Options

| Field         | Description                                                            |
| ------------- | ---------------------------------------------------------------------- |
| `displayName` | Custom name that agents will see (simpler than the original tool name) |
| `description` | Custom description explaining what the tool does                       |
| `schema`      | Simplified parameter schema with only the fields agents need           |

<Note>Tool overrides are configured in the [Visual Builder](/visual-builder/tools/mcp-servers#tool-overrides) for easier management, but can also be defined programmatically as shown above.</Note>

<Note>Tip: When adding tools to an agent, you can also reference the tool in the agent's prompt to help the agent understand how to use the tool.</Note>
