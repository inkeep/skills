---
title: "CLI inkeep pull Command"
description: "Pull project configuration from server and update local TypeScript files"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep pull Command

### `inkeep pull`

Pull project configuration from the server and update all TypeScript files in your local project using LLM generation.

```bash
inkeep pull
```

**Options:**

* `--project <project-id>` - Project ID to pull (or path to project directory). If in project directory, validates against local project ID.
* `--all` - Pull all projects from the server to the current directory
* `--config <path>` - Path to configuration file
* `--env <environment>` - Environment file to generate (development, staging, production). Defaults to development
* `--json` - Output project data as JSON instead of generating files
* `--debug` - Enable debug logging
* `--verbose` - Enable verbose logging
* `--force` - Force regeneration even if no changes detected
* `--introspect` - Completely regenerate all files from scratch (no comparison needed)
* `--profile <name>` - Use a specific CLI profile (overrides the active profile)
* `--quiet` - Suppress interactive prompts and extra logs

#### Single Project Pull

The most common workflow is to run `inkeep pull` from inside a project directory. A project directory is identified by having an `index.ts` file that exports a project object (with `__type = "project"`).

```bash
# Navigate to your project directory
cd my-project

# Pull updates from the server
inkeep pull
```

The CLI will:

1. Detect `index.ts` in the current directory
2. Load the project export to get the project ID
3. Fetch the latest configuration from the server
4. Update your local TypeScript files with any changes

**Smart Project Detection:**

The pull command intelligently handles different scenarios:

1. **Project Directory Detection**: If your current directory contains an `index.ts` file that exports a project, the command automatically uses that project's ID
2. **Project ID Validation**: If you specify `--project` while in a project directory, it validates that the ID matches your local project
3. **Subdirectory Creation**: When not in a project directory, creates a new subdirectory named after the project ID

**`--project` Flag Behavior:**

The `--project` flag has dual functionality:

* **Directory Path**: If the value points to a directory containing `index.ts`, it uses that directory
* **Project ID**: If the value doesn't match a valid directory path, it treats it as a project ID

**Pull Modes:**

| Scenario                 | Command                                | Behavior                                                         |
| ------------------------ | -------------------------------------- | ---------------------------------------------------------------- |
| In project directory     | `inkeep pull`                          | ✅ Automatically detects project, pulls to current directory      |
| In project directory     | `inkeep pull --project <matching-id>`  | ✅ Validates ID matches local project, pulls to current directory |
| In project directory     | `inkeep pull --project <different-id>` | ❌ Error: Project ID doesn't match local project                  |
| Not in project directory | `inkeep pull`                          | ❌ Error: Requires --project flag                                 |
| Not in project directory | `inkeep pull --project <id>`           | ✅ Creates `<id>/` subdirectory with project files                |

**How it Works:**

The pull command discovers and updates all TypeScript files in your project based on the latest configuration from the server:

1. **File Discovery**: Recursively finds all `.ts` files in your project (excluding `environments/` and `node_modules/`)
2. **Smart Categorization**: Categorizes files as index, agents, Sub Agents, tools, or other files
3. **Context-Aware Updates**: Updates each file with relevant context from the server:
   * **Agent files**: Updated with specific agent data
   * **Sub Agent files**: Updated with specific Sub Agent configurations
   * **Tool files**: Updated with specific tool definitions
   * **Other files**: Updated with full project context
4. **LLM Generation**: Uses AI to maintain code structure while updating with latest data

#### TypeScript Updates (Default)

By default, the pull command updates your existing TypeScript files using LLM generation:

1. **Context Preservation**: Maintains your existing code structure and patterns
2. **Selective Updates**: Only updates relevant parts based on server configuration changes
3. **File-Specific Context**: Each file type receives appropriate context (Agents get Agent data, Sub Agents get Sub Agent data, etc.)

**Examples:**

```bash
# Directory-aware pull: Automatically detects project from current directory
cd my-project  # Directory contains index.ts with project export
inkeep pull    # Pulls to current directory, no subdirectory created

# Validate project ID while in project directory
cd my-project  # Directory contains index.ts
inkeep pull --project my-project  # Validates ID matches, pulls to current directory

# Error case: Wrong project ID in project directory
cd my-project  # Directory contains index.ts with different project ID
inkeep pull --project different-project  # ERROR: Project ID mismatch

# Pull when NOT in a project directory (requires --project)
cd ~/projects
inkeep pull  # ERROR: Requires --project flag

# Pull specific project to new subdirectory
cd ~/projects
inkeep pull --project my-project  # Creates ./my-project/ subdirectory

# Save project data as JSON file instead of updating files
inkeep pull --json

# Enable debug logging
inkeep pull --debug

# Enable verbose logging
inkeep pull --verbose

# Force regeneration even if no changes detected
inkeep pull --force

# Completely regenerate all files from scratch
inkeep pull --introspect

# Generate environment-specific credentials
inkeep pull --env production

# Use specific config file
inkeep pull --config ./custom-config/inkeep.config.ts

# Pull all projects from server
inkeep pull --all

# Pull using a specific profile (overrides active)
inkeep pull --profile staging

# Quiet non-essential output for CI
inkeep pull --quiet
```

#### Batch Pull with `--all`

The `--all` flag fetches all projects from your tenant and pulls them to the current directory. Each project is created as a subdirectory.

```bash
# From a workspace directory
inkeep pull --all
```

**How it Works:**

1. Fetches all projects from the API for your tenant
2. Categorizes each project as "existing" or "new":
   * **Existing**: Directory with `index.ts` already exists → uses smart comparison mode
   * **New**: No local directory → uses introspect mode for fresh generation
3. Processes each project sequentially

**Output:**

```
🔄 Batch Pull: Sequential processing with smart comparison

  • Existing projects: Smart comparison + LLM merging + confirmation prompts
  • New projects: Fresh generation with introspect mode

Projects to pull:

  Existing (smart comparison):
    • My Agent (my-agent)
  New (introspect):
    • New Project (new-project)

──────────────────────────────────────────────────────────
[1/2] Pulling My Agent...
  ✓ My Agent → ./my-agent

──────────────────────────────────────────────────────────
[2/2] Pulling New Project...
  ✓ New Project → ./new-project

════════════════════════════════════════════════════════════
📊 Batch Pull Summary:
  ✓ Succeeded: 2
```

#### Model Configuration

The `inkeep pull` command automatically selects the best available model for LLM generation based on your API keys:

1. **Anthropic Claude** (if `ANTHROPIC_API_KEY` is set): `claude-sonnet-4-5`
2. **OpenAI GPT** (if `OPENAI_API_KEY` is set): `gpt-5.1`
3. **Google Gemini** (if `GOOGLE_GENERATIVE_AI_API_KEY` is set): `gemini-2.5-flash`

The models are used for intelligent content merging when updating modified components, ensuring your local customizations are preserved while incorporating upstream changes.

#### Validation Process

The `inkeep pull` command includes a two-stage validation process to ensure generated TypeScript files accurately represent your backend configuration:

**1. Basic File Verification**

* Checks that all expected files exist (index.ts, agent files, tool files, component files)
* Verifies file naming conventions match (kebab-case)
* Ensures project export is present in index.ts

**2. Round-Trip Validation** *(New in v0.24.0)*

* Loads the generated TypeScript using the same logic as `inkeep push`
* Serializes it back to JSON format
* Compares the serialized JSON with the original backend data
* Reports any differences found

This round-trip validation ensures:

* ✅ Generated TypeScript can be successfully loaded and imported
* ✅ The serialization logic (TS → JSON) works correctly
* ✅ Generated files will work with `inkeep push` without errors
* ✅ No data loss or corruption during the pull process

**Validation Output:**

```bash
✓ Basic file verification passed
✓ Round-trip validation passed - generated TS matches backend data
```

**If validation fails:**

The CLI will display specific differences between the generated and expected data:

```bash
✗ Round-trip validation failed

❌ Round-trip validation errors:
   The generated TypeScript does not serialize back to match the original backend data.
  • Value mismatch at agents.my-agent.name: "Original Name" vs "Generated Name"
  • Missing tool in generated: tool-id

⚠️  This indicates an issue with LLM generation or schema mappings.
The generated files may not work correctly with `inkeep push`.
```

**TypeScript generation fails:**

* Ensure your network connectivity and API endpoints are correct
* Check that your model provider credentials (if required by backend) are set up
* Try using `--json` flag as a fallback to get the raw project data

**Validation errors during pull:**

* The generated TypeScript may have syntax errors or missing dependencies
* Check the generated file manually for obvious issues
* Try pulling as JSON first to verify the source data: `inkeep pull --json`
* If round-trip validation fails, report the issue with the error details
