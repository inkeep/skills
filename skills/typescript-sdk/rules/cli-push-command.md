---
title: "CLI inkeep push Command"
description: "Push agent configurations to your server"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep push Command

### `inkeep push`

**Primary use case:** Push a project containing Agent configurations to your server. This command deploys your entire multi-agent project, including all Agents, Sub Agents, and tools.

```bash
inkeep push
```

**Options:**

* `--project <project-id>` - Project ID or path to project directory
* `--all` - Push all projects found in the current directory tree
* `--env <environment>` - Load environment-specific credentials from `environments/<environment>.env.ts`
* `--config <path>` - Override config file path (bypasses automatic config discovery)
* `--tenant-id <id>` - Override tenant ID
* `--agents-manage-api-url <url>` - Override the management API URL from config
* `--agents-run-api-url <url>` - Override agents run API URL
* `--json` - Generate project data as JSON file instead of pushing to server
* `--profile <name>` - Use a specific CLI profile (overrides the active profile)
* `--quiet` - Suppress interactive prompts and extra logs

#### Single Project Push

The most common workflow is to run `inkeep push` from inside a project directory. A project directory is identified by having an `index.ts` file that exports a project object (with `__type = "project"`).

```bash
# Navigate to your project directory
cd my-project

# Push the current project
inkeep push
```

The CLI will:

1. Detect `index.ts` in the current directory
2. Load the project export from `index.ts`
3. Search upward for `inkeep.config.ts` to get tenant/API configuration
4. Push the project to the server

**Examples:**

```bash
# Push project from current directory (most common)
cd my-project && inkeep push

# Push specific project directory
inkeep push --project ./my-project

# Push all projects in current directory tree
inkeep push --all

# Push with development environment credentials
inkeep push --env development

# Generate project JSON without pushing
inkeep push --json

# Override tenant ID
inkeep push --tenant-id my-tenant

# Override API URLs
inkeep push --agents-manage-api-url https://api.example.com
inkeep push --agents-run-api-url https://run.example.com

# Use specific config file
inkeep push --config ./custom-config/inkeep.config.ts

# Use a named profile (overrides the active profile)
inkeep push --profile staging

# Quiet non-essential output for CI
inkeep push --quiet
```

#### Batch Push with `--all`

The `--all` flag discovers and pushes all projects in the current directory tree. A directory is considered a project if its `index.ts` file exports an object with `__type = "project"`.

```bash
# From a workspace root containing multiple projects
inkeep push --all
```

**Project Discovery:**

1. Searches recursively for directories containing `index.ts` files
2. Validates each `index.ts` exports a project (checks for `__type = "project"`)
3. Ensures each project can access an `inkeep.config.ts` (in the same directory or a parent directory)

**Shared Configuration:**

You can use a single `inkeep.config.ts` at the workspace root for multiple projects:

```
workspace-root/
├── inkeep.config.ts          # Shared config for all projects
└── projects/
    ├── project-a/
    │   └── index.ts          # Project A
    ├── project-b/
    │   └── index.ts          # Project B
    └── project-c/
        └── index.ts          # Project C
```

Running `inkeep push --all` from `workspace-root/` or `workspace-root/projects/` will discover and push all three projects using the shared configuration.

**Output:**

```
🚀 Batch Push: Finding all projects...

Found 3 project(s) to push:

  • project-a
  • project-b
  • project-c

[1/3] Pushing project-a...
  ✓ Project A
[2/3] Pushing project-b...
  ✓ Project B
[3/3] Pushing project-c...
  ✓ Project C

📊 Batch Push Summary:
  ✓ Succeeded: 3
```

**Environment Credentials:**

The `--env` flag loads environment-specific credentials when pushing your project. This will look for files like `environments/development.env.ts` or `environments/production.env.ts` in your project directory and load the credential configurations defined there.

**Example environment file:**

```typescript
// environments/development.env.ts
import { CredentialStoreType } from "@inkeep/agents-core";
import { registerEnvironmentSettings } from "@inkeep/agents-sdk";

export const development = registerEnvironmentSettings({
  credentials: {
    "api-key-dev": {
      id: "api-key-dev",
      type: CredentialStoreType.memory,
      credentialStoreId: "memory-default",
      retrievalParams: {
        key: "API_KEY_DEV",
      },
    },
  },
});
```

#### Project Discovery and Structure

The `inkeep push` command follows this discovery process:

1. **Config File Discovery**: Searches for `inkeep.config.ts` using this pattern:

   * Starts from current working directory
   * Traverses **upward** through parent directories until found
   * Can be overridden by providing a path to the config file with the `--config` flag

2. **Workspace Structure**: Expects this directory layout:

   ```
   workspace-root/
   ├── package.json              # Workspace package.json
   ├── tsconfig.json             # Workspace TypeScript config
   ├── inkeep.config.ts          # Inkeep configuration file
   ├── my-project/               # Individual project directory
   │   ├── index.ts              # Project entry point
   │   ├── agents/               # Agent definitions
   │   │   └── *.ts
   │   ├── tools/                # Tool definitions
   │   │   └── *.ts
   │   ├── data-components/      # Data component definitions
   │   │   └── *.ts
   │   └── environments/         # Environment-specific configs
   │       ├── development.env.ts
   │       └── production.env.ts
   └── another-project/          # Additional projects
       └── index.ts
   ```

3. **Resource Compilation**: Automatically discovers and compiles:
   * All project directories containing `index.ts`
   * All TypeScript files within each project directory
   * Categorizes files by type (agents, Sub Agents, tools, data components)
   * Resolves dependencies and relationships within each project

#### Push Behavior

When pushing, the CLI:

* Finds and loads configuration from `inkeep.config.ts` at workspace root
* Discovers all project directories containing `index.ts`
* Applies environment-specific settings if `--env` is specified
* Compiles all project resources defined in each project's `index.ts`
* Validates Sub Agent relationships and tool configurations across all projects
* Deploys all projects to the management API
* Prints deployment summary with resource counts per project
