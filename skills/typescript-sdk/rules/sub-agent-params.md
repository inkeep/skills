---
title: "Sub Agent Parameters Reference"
description: "Complete parameter reference for subAgent() configuration"
topic-path: "typescript-sdk/agent-settings"
---

# Sub Agent Parameters Reference

## Sub Agent overview

Beyond model configuration, Sub Agents define tools, structured outputs, and agent-to-agent relationships available to the Sub Agent.

| Parameter            | Type     | Required | Description                                                                                                                                                         |
| -------------------- | -------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                 | string   | Yes      | Stable Sub Agent identifier used for consistency and persistence                                                                                                    |
| `name`               | string   | Yes      | Human-readable name for the Sub Agent                                                                                                                               |
| `prompt`             | string   | Yes      | Detailed behavior guidelines and system prompt for the Sub Agent                                                                                                    |
| `description`        | string   | No       | Brief description of the Sub Agent's purpose and capabilities                                                                                                       |
| `models`             | object   | No       | Model configuration for this Sub Agent. See [Model Configuration](/typescript-sdk/models)                                                                           |
| `stopWhen`           | object   | No       | Stop conditions (`stepCountIs`). See [Configuring StopWhen](#configuring-stopwhen)                                                                                  |
| `canUse`             | function | No       | Returns the list of MCP/tools the Sub Agent can use. See [MCP Servers](/tutorials/mcp-servers/overview) for how to find or build MCP servers                        |
| `dataComponents`     | array    | No       | Structured output components for rich, interactive responses. See [Data Components](/typescript-sdk/structured-outputs/data-components)                             |
| `artifactComponents` | array    | No       | Components for handling tool or Sub Agent outputs. See [Artifact Components](/typescript-sdk/structured-outputs/artifact-components)                                |
| `canTransferTo`      | function | No       | Function returning array of Sub Agents this Sub Agent can transfer to. See [Transfer Relationships](/typescript-sdk/agent-relationships#transfer-relationships)     |
| `canDelegateTo`      | function | No       | Function returning array of Sub Agents this Sub Agent can delegate to. See [Delegation Relationships](/typescript-sdk/agent-relationships#delegation-relationships) |
