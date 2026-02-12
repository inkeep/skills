---
title: "CLI inkeep list-agent Command"
description: "List all available agents for a specific project"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep list-agent Command

### `inkeep list-agent`

List all available agents for a specific project.

```bash
inkeep list-agent --project <project-id>
```

**Options:**

* `--project <project-id>` - **Required.** Project ID to list agents for
* `--tenant-id <tenant-id>` - Tenant ID
* `--agents-api-url <url>` - Agents API URL
* `--config <path>` - Path to configuration file
* `--config-file-path <path>` - Path to configuration file (deprecated, use --config)

**Examples:**

```bash
# List agents for a specific project
inkeep list-agent --project my-project

# List agents using a specific config file
inkeep list-agent --project my-project --config ./inkeep.config.ts

# Override tenant and API URL
inkeep list-agent --project my-project --tenant-id my-tenant --agents-api-url https://api.example.com
```
