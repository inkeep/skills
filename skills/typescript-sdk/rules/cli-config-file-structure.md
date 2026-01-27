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
  agentsManageApiUrl: "http://localhost:3002",
  agentsRunApiUrl: "http://localhost:3003",
});
```

### Configuration Priority

Effective resolution order:

1. Command-line flags (highest)
2. Environment variables (override config values)
3. `inkeep.config.ts` values
