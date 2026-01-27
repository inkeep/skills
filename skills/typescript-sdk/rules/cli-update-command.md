---
title: "CLI inkeep update Command"
description: "Update the Inkeep CLI to the latest version"
topic-path: "typescript-sdk/cli-reference"
---

# CLI inkeep update Command

### `inkeep update`

Update the Inkeep CLI to the latest version from npm.

```bash
inkeep update
```

**Options:**

* `--check` - Check for updates without installing
* `--force` - Force update even if already on latest version

**How it Works:**

The update command automatically:

1. **Detects Package Manager**: Identifies which package manager (npm, pnpm, bun, or yarn) was used to install the CLI globally
2. **Checks Version**: Compares your current version with the latest available on npm
3. **Updates CLI**: Executes the appropriate update command for your package manager
4. **Displays Changelog**: Shows a link to the changelog for release notes

**Examples:**

```bash
# Check if an update is available (no installation)
inkeep update --check

# Update to latest version (with confirmation prompt)
inkeep update

# Force reinstall current version
inkeep update --force

# Skip confirmation prompt (useful for CI/CD)
inkeep update --force
```

**Output Example:**

```
📦 Version Information:
  • Current version: 0.22.3
  • Latest version:  0.23.0

📖 Changelog:
  • https://github.com/inkeep/agents/blob/main/agents-cli/CHANGELOG.md

🔍 Detected package manager: pnpm

✅ Updated to version 0.23.0
```

**Troubleshooting:**

If you encounter permission errors, try running with elevated permissions:

```bash
# For npm, pnpm, yarn
sudo inkeep update

# For bun
sudo -E bun add -g @inkeep/agents-cli@latest
```

**Package Manager Detection:**

The CLI automatically detects which package manager you used by checking global package installations:

* npm: Checks `npm list -g @inkeep/agents-cli`
* pnpm: Checks `pnpm list -g @inkeep/agents-cli`
* bun: Checks `bun pm ls -g`
* yarn: Checks `yarn global list`

If automatic detection fails, the CLI will prompt you to select your package manager.
