---
title: "CLI inkeep list-agents Command"
description: "List all available agents for a specific project"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep list-agents Command

### `inkeep list-agents`

List all available agents for a specific project.

```bash
inkeep list-agents --project <project-id>
```

**Options:**

* `--project <project-id>` - **Required.** Project ID to list agents for
* `--tenant-id <tenant-id>` - Tenant ID
* `--agents-manage-api-url <url>` - Agents manage API URL
* `--config <path>` - Path to configuration file
* `--config-file-path <path>` - Path to configuration file (deprecated, use --config)

**Examples:**

```bash
# List agents for a specific project
inkeep list-agents --project my-project

# List agents using a specific config file
inkeep list-agents --project my-project --config ./inkeep.config.ts

# Override tenant and API URL
inkeep list-agents --project my-project --tenant-id my-tenant --agents-manage-api-url https://api.example.com
```
