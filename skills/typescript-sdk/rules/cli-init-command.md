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

* `--local` - Initialize for local development (creates a local profile with no authentication required)
* `--no-interactive` - Skip interactive path selection
* `--config <path>` - Path to use as template for new configuration

**Cloud vs Local Initialization:**

By default, `inkeep init` runs the cloud onboarding wizard which:

* Opens browser-based authentication
* Fetches your organizations and projects from Inkeep Cloud
* Creates project directories with configuration files
* Sets up the `cloud` profile as the default in `~/.inkeep/profiles.yaml`

Use `--local` for local development:

* Prompts for tenant ID and API URL (defaults to `localhost:3002`); Manage UI defaults to `localhost:3000`
* Creates `inkeep.config.ts` with your local configuration
* Sets up a `local` profile as the default in `~/.inkeep/profiles.yaml`
* No authentication required

For self-hosted deployments that require authentication, use `inkeep profile add` and select **Custom**.

**Examples:**

```bash
# Cloud initialization (default) - opens browser for auth
inkeep init

# Local initialization - no auth required
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
