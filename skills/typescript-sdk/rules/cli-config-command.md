---
title: "CLI inkeep config Command"
description: "Manage Inkeep configuration values"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep config Command

### `inkeep config`

Manage Inkeep configuration values.

**Subcommands:**

#### `inkeep config get [key]`

Get configuration value(s).

```bash
inkeep config get [key]
```

**Options:**

* `--config <path>` - Path to configuration file
* `--config-file-path <path>` - Path to configuration file (deprecated, use --config)

**Examples:**

```bash
# Get all config values
inkeep config get

# Get specific value
inkeep config get tenantId
```

#### `inkeep config set <key> <value>`

Set a configuration value.

```bash
inkeep config set <key> <value>
```

**Options:**

* `--config <path>` - Path to configuration file
* `--config-file-path <path>` - Path to configuration file (deprecated, use --config)

**Examples:**

```bash
inkeep config set tenantId my-tenant-id
inkeep config set apiUrl http://localhost:3002
```

#### `inkeep config list`

List all configuration values.

```bash
inkeep config list
```

**Options:**

* `--config <path>` - Path to configuration file
* `--config-file-path <path>` - Path to configuration file (deprecated, use --config)
