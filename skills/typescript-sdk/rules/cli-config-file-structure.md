---
title: "CLI Configuration File Structure"
description: "Structure of inkeep.config.ts and configuration priority"
topic-path: "typescript-sdk/cli-reference"
---

# CLI Configuration File Structure

## Configuration File

The CLI uses a configuration file (typically `inkeep.config.ts`) to store settings:

```typescript
import { defineConfig } from "@inkeep/agents-cli/config";

export default defineConfig({
  tenantId: "your-tenant-id",
  agentsApi: {
    url: "http://localhost:3002",
  },
});
```

### Configuration Priority

The CLI resolves configuration values in the following order. Higher-priority sources override lower ones:

**Local development (with a profile):**

| Priority    | Source             | Example                                                                                |
| ----------- | ------------------ | -------------------------------------------------------------------------------------- |
| 1 (highest) | CLI flags          | `--tenant-id`, `--agents-api-url`                                                      |
| 2           | Active profile     | API URLs, API key, and tenant ID from `~/.inkeep/profiles.yaml` + keychain credentials |
| 3           | `inkeep.config.ts` | `tenantId`, `agentsApi.url`                                                            |
| 4 (lowest)  | Built-in defaults  | Default API URLs                                                                       |

When a profile is active, it overrides `inkeep.config.ts` for these fields:

* `agentsApiUrl` — from `profile.remote.api`
* `manageUiUrl` — from `profile.remote.manageUi`
* `agentsApiKey` — from the profile's keychain credential
* `tenantId` — from the profile's authenticated organization ID

Other `inkeep.config.ts` values (like `outputDirectory`) are still respected.

**CI/CD (no profile):**

In CI environments, profiles are skipped entirely. The precedence becomes:

| Priority    | Source                | Example                                                       |
| ----------- | --------------------- | ------------------------------------------------------------- |
| 1 (highest) | CLI flags             | `--tenant-id`, `--agents-api-url`                             |
| 2           | Environment variables | `INKEEP_API_KEY`, `INKEEP_TENANT_ID`, `INKEEP_AGENTS_API_URL` |
| 3           | `inkeep.config.ts`    | `tenantId`, `agentsApi.url`                                   |
| 4 (lowest)  | Built-in defaults     | Default API URLs                                              |

<Tip>
  Use `--profile <name>` on any command to override the active profile for a single invocation. This is useful for pushing to staging while your default profile points to production.
</Tip>
