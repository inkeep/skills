---
title: "CLI inkeep add Command"
description: "Pull a template project or MCP from the Inkeep Agents Cookbook"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep add Command

### `inkeep add`

Pull a template project or MCP from the [Inkeep Agents Cookbook](https://github.com/inkeep/agents/tree/main/agents-cookbook).

```bash
inkeep add [options]
```

**Options:**

* `--project <name>` - Add a project template
* `--mcp <name>` - Add a custom MCP server template for common use cases (adds server code to your project)
* `--target-path <path>` - Target path to add the template to
* `--config <path>` - Path to configuration file

**Examples:**

```bash
# List available templates (both project and MCP)
inkeep add

# Add a project template
inkeep add --project activities-planner

# Add project template to specific path
inkeep add --project activities-planner --target-path ./examples

# Add a custom MCP server template (auto-detects apps/mcp/app directory)
inkeep add --mcp zendesk

# Add MCP server template to specific path
inkeep add --mcp zendesk --target-path ./apps/mcp/app

# Using specific config file
inkeep add --project activities-planner --config ./my-config.ts
```

**Behavior:**

* When adding an MCP server template without `--target-path`, the CLI automatically searches for an `apps/mcp/app` directory in your project
* If no app directory is found, you'll be prompted to confirm whether to add to the current directory
* Project templates are added to the current directory or specified `--target-path`
* Model configurations are automatically injected based on available API keys in your environment (`ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, or `GOOGLE_GENERATIVE_AI_API_KEY`)
