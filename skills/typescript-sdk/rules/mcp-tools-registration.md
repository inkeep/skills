---
title: "Registering MCP Servers as Tools"
description: "How to register and configure MCP tools for your agents"
topic-path: "typescript-sdk/tools/mcp-tools"
---

# Registering MCP Servers as Tools

MCP tools connect to external MCP servers, enabling integration with a wide ecosystem of tools and services. Registering them as tools is a two step process:

### Step 1: Get an MCP server URL

You need access to an MCP server before you can use its tools. Learn about connecting to Native servers, using Composio, using Gram, or building Custom servers in the [MCP Servers tutorial](/tutorials/mcp-servers/overview).

### Step 2: Register the MCP server as a tool in your agent

```typescript
import { subAgent, mcpTool } from "@inkeep/agents-sdk";

// Register an existing MCP server as a tool
const knowledgeBaseTool = mcpTool({
  id: "knowledge-base-tool",
  name: "knowledge_base",
  description: "Search the company knowledge base for information",
  serverUrl: "https://kb.yourcompany.com/mcp", // URL of your deployed MCP server
});

const qaAgent = subAgent({
  id: "qa-agent",
  name: "QA Agent",
  description: "Responsible for answering questions about the company knowledge base.",
  prompt: `You are a Question-Answer agent that can answer questions about the company knowledge base. You have access to the knowledge base tool to search for information.`,
  canUse: () => [knowledgeBaseTool],
});
```

## Custom Usage Instructions

You can provide custom instructions for how agents should use tools from an MCP server by adding a `prompt` field:

```typescript
import { mcpTool } from "@inkeep/agents-sdk";

const weatherTool = mcpTool({
  id: "weather-tool",
  name: "weather_api",
  description: "Get current weather information",
  serverUrl: "https://weather.example.com/mcp",
  prompt: `When using weather tools, always ask for the user's location first if not provided. 
           Format temperature responses in both Celsius and Fahrenheit for better user experience.
           Include weather alerts if any severe conditions are detected.`,
});
```

The custom prompt is provided to agents as usage guidelines for all tools from this server, helping them understand the specific context and best practices for using these tools.
