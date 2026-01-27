---
title: "Tool Approvals Configuration"
description: "How to configure tools to require user approval before execution"
topic-path: "typescript-sdk/tools/tool-approvals"
---

# Tool Approvals Configuration

## Overview

Tool approvals let you mark specific tools as “requires approval”. When the agent tries to run one, execution pauses and your client must approve or deny before the run can continue.

## Configure tools to require approval

### TypeScript SDK (MCP tools)

```typescript
import { agent, tool } from "@inkeep/agents-sdk";

const weatherAgent = agent("weather-forecast")
  .prompt("You help users get weather information.")
  .canUse(
    tool("weather-mcp").with({
      selectedTools: [
        "get_current_weather", // No approval required
        {
          name: "get_weather_forecast",
          needsApproval: true, // Requires user approval
        },
      ],
    })
  );
```

See also: [MCP tools](/typescript-sdk/tools/mcp-tools)

### Visual Builder

In the Visual Builder, select an MCP tool node and toggle approval for individual tools in the tools table.

See also: [Tools in the Visual Builder](/visual-builder/tools)

## Runtime Behavior

When a tool requires approval, you’ll see a `tool-approval-request` in the stream.

See [Chat API](/talk-to-your-agents/chat-api) for the tool event payloads (including approval requests, tool inputs, and tool outputs).
