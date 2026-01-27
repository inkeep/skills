---
title: "CLI inkeep init Command"
description: "Initialize a new Inkeep configuration file in your project"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep init Command

## Commands

### `inkeep init`

Initialize a new Inkeep configuration file in your project.

```bash
inkeep init [path]
```

**Options:**

* `--local` - Initialize for local/self-hosted development (creates a local profile as default)
* `--no-interactive` - Skip interactive path selection
* `--config <path>` - Path to use as template for new configuration

**Cloud vs Local Initialization:**

By default, `inkeep init` runs the cloud onboarding wizard which:

* Opens browser-based authentication
* Fetches your organizations and projects from Inkeep Cloud
* Creates project directories with configuration files
* Sets up the `cloud` profile as the default in `~/.inkeep/profiles.yaml`

Use `--local` for self-hosted or local development:

* Prompts for tenant ID and API URLs (defaults to `localhost:3002` and `localhost:3003`)
* Creates `inkeep.config.ts` with your local configuration
* Sets up a `local` profile as the default in `~/.inkeep/profiles.yaml`
* No authentication required

**Examples:**

```bash
# Cloud initialization (default) - opens browser for auth
inkeep init

# Local/self-hosted initialization - no auth required
inkeep init --local

# Initialize in specific directory
inkeep init ./my-project

# Local init in specific directory
inkeep init --local ./my-project

# Non-interactive mode
inkeep init --no-interactive

# Use specific config as template
inkeep init --config ./template-config.ts
```
