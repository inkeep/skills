---
name: inkeep-best-practices
description: Inkeep Agents SDK best practices for building multi-agent systems. This skill should be used when creating, configuring, or refactoring Inkeep agents, sub agents, tools, and projects. Triggers on tasks involving agent design, MCP servers, transfers/delegates, structured outputs, or context management.
license: MIT
metadata:
  author: inkeep
  version: "1.0.0"
---

# Inkeep Agents SDK Best Practices

Comprehensive guide for building effective multi-agent systems with the Inkeep Agents SDK. Contains best practices across 7 categories, prioritized by impact to guide agent design, tool configuration, and context management.

## When to Apply

Reference these guidelines when:
- Creating new Agents or Sub Agents
- Configuring MCP servers or function tools
- Deciding between transfer vs delegate patterns
- Implementing structured outputs (data/artifact/status components)
- Managing context via headers and context fetchers
- Organizing projects and credentials
- Using the CLI for push/pull workflows

## Rule Categories by Priority

| Priority | Category | Impact | Prefix |
|----------|----------|--------|--------|
| 1 | Sub Agent Design | CRITICAL | `subagent-` |
| 2 | Multi-Agent Relationships | HIGH | `relationship-` |
| 3 | Tools & MCP Servers | HIGH | `tools-` |
| 4 | Context Management | MEDIUM-HIGH | `context-` |
| 5 | Structured Outputs | MEDIUM | `outputs-` |
| 6 | Project Organization | MEDIUM | `project-` |
| 7 | Development Workflow | LOW-MEDIUM | `workflow-` |

## Quick Reference

### 1. Sub Agent Design (CRITICAL)

- `subagent-focused-scope` - Keep sub agents focused on narrow, well-defined tasks
- `subagent-tool-limit` - Limit sub agents to 5-7 related tools
- `subagent-clear-instructions` - Write clear, specific instructions in prompts
- `subagent-stable-ids` - Use explicit stable IDs for agents across deployments

### 2. Multi-Agent Relationships (HIGH)

- `relationship-transfer-vs-delegate` - Use transfer for handoff, delegate for subtasks
- `relationship-delegate-no-user-response` - Delegates cannot respond directly to users
- `relationship-transfer-control` - Transfers make receiving agent the primary driver
- `relationship-minimal-complexity` - Avoid multi-agent when single agent suffices

### 3. Tools & MCP Servers (HIGH)

- `tools-mcp-server-config` - Configure MCP servers with proper credentials
- `tools-selected-tools` - Use selectedTools to limit exposed tools
- `tools-function-tools` - Use function tools for custom sandboxed logic
- `tools-credential-references` - Reference credentials by ID, resolve at push time

### 4. Context Management (MEDIUM-HIGH)

- `context-headers` - Pass request-time context via headers
- `context-fetchers` - Auto-fetch external data at conversation start
- `context-variables` - Reference headers/context as {{variables}} in prompts
- `context-artifacts` - Save tool call results as artifacts for sharing

### 5. Structured Outputs (MEDIUM)

- `outputs-data-components` - Use data components for rich UI rendering
- `outputs-artifact-components` - Define artifact structure with props schema
- `outputs-status-updates` - Provide real-time progress during long operations
- `outputs-zod-schemas` - Use Zod schemas for type-safe component props

### 6. Project Organization (MEDIUM)

- `project-logical-grouping` - Group related agents/tools in projects
- `project-model-inheritance` - Configure models at project level for inheritance
- `project-credential-management` - Manage credentials per environment
- `project-stop-conditions` - Set stopWhen limits at project/agent level

### 7. Development Workflow (LOW-MEDIUM)

- `workflow-push-pull` - Use CLI push/pull to sync code with Visual Builder
- `workflow-environment-settings` - Use createEnvironmentSettings for env-specific config
- `workflow-minimal-config` - Don't add unnecessary properties or fake MCPs

## How to Use

Read individual rule files for detailed explanations and code examples:

```
rules/subagent-focused-scope.md
rules/relationship-transfer-vs-delegate.md
rules/tools-mcp-server-config.md
```

Each rule file contains:
- Brief explanation of why it matters
- Incorrect code example with explanation
- Correct code example with explanation
- Additional context and references