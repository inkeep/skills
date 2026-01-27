---
title: "Runtime Configuration Variables"
description: "All available environment variables for configuring runtime limits"
topic-path: "typescript-sdk/configure-runtime-limits"
---

# Runtime Configuration Variables

## All Available Configuration Variables

All runtime configuration variables are defined as TypeScript constants. To explore available overrides and their detailed documentation:

1. **View the constant files** (source of truth):
   * Schema validation constants: `packages/agents-core/src/constants/schema-validation/defaults.ts`
   * Runtime execution constants (shared): `packages/agents-core/src/constants/execution-limits-shared/defaults.ts`
   * Runtime execution constants (run-api): `packages/agents-run-api/src/constants/execution-limits/defaults.ts`
2. Each constant includes inline documentation explaining its purpose, behavior, and impact
3. Convert constant names to environment variables by prefixing with `AGENTS_`

### Configuration Categories

The runtime configuration is organized into functional categories:

**Schema Validation Constants** (API-level limits):

* **Agent Execution Transfer Count**: Limits transfers between sub-agents in a turn
* **Sub-Agent Turn Generation Steps**: LLM generation steps within a single sub-agent activation
* **Status Update Thresholds**: Event and time-based triggers for status updates
* **Prompt Validation**: Character limits for agent and sub-agent system prompts
* **Context Fetchers**: HTTP timeouts for external data fetching (CRM lookups, API calls)

**Runtime Execution Constants - Shared** (infrastructure-level limits):

* **MCP Tool Connection & Retry**: Connection timeouts and exponential backoff for MCP tools
* **Conversation History**: Token limits for conversation context windows

**Runtime Execution Constants - Run-API** (run-api service-specific limits):

* **LLM Generation Timeouts**: Timeout behavior for AI model calls
* **Function Tool Sandbox**: Execution limits for function tools in isolated sandboxes
* **Delegation & A2A**: Inter-agent communication settings
* **Artifact Processing**: UI component generation with retry mechanisms
* **Session Management**: Tool result caching and cleanup
* **Streaming**: Frequency, batching, and lifetime limits for streams
