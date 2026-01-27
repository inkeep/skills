---
title: "Selecting MCP Tools"
description: "How to selectively enable tools from MCP servers"
topic-path: "typescript-sdk/tools/mcp-tools"
---

# Selecting MCP Tools

## Selecting Tools

<Tip>
  If some tools are sensitive (delete, write, payments, etc.), you can require explicit user approval before they run. See
  [Tool approvals](/typescript-sdk/tools/tool-approvals).
</Tip>

### Selective Tool Activation

Enable only specific tools from a server using MCP Server tool names in the `activeTools` field.

```typescript
import { mcpTool } from "@inkeep/agents-sdk";

const selectiveTool = mcpTool({
  id: "limited-server",
  name: "analytics_readonly",
  description: "Analytics server with limited access",
  serverUrl: "https://analytics.example.com/mcp",
  activeTools: ["get_metrics", "generate_report"], // Only these tools
});
```

### Using `with` for Tool Selection

While `activeTools` in `mcpTool` limits which tools are available from the server, you can further refine tool access at the sub agent level using `mcpTool.with`. This allows different agents to use different subsets of tools from the same MCP server.

```typescript
import { agent, mcpTool } from "@inkeep/agents-sdk";

// Define the MCP server with all available tools
const customerSupportTool = mcpTool({
  id: "customer-support-tool",
  name: "Customer Support Tool",
  description: "Customer Support Tool",
  serverUrl: "https://customer-support.example.com/mcp",
});

// Agent that only uses specific tools from the server
const customerSupportAgent = subAgent({
  id: "customer-support-agent",
  name: "Customer Support Agent",
  description: "Responsible for customer support",
  prompt: "You are a helpful assistant",
  canUse: () => [customerSupportTool.with({
      selectedTools: ["customer-support-tool"],
      headers: { "X-Custom-Header": "custom-value" }
    })],
});
```
