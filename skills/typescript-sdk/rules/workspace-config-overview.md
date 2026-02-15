---
title: "Workspace Configuration File"
description: "Structure and options for inkeep.config.ts"
topic-path: "typescript-sdk/workspace-configuration"
---

# Workspace Configuration File

## Overview

The `inkeep.config.ts` file at the workspace root defines settings for all projects in this workspace. See [Project Management](/typescript-sdk/project-management) for where this file should be placed.

```typescript
import { defineConfig } from "@inkeep/agents-cli/config";
import "dotenv/config";

export default defineConfig({
  tenantId: 'my-company',
  agentsApi: {
    url: 'http://localhost:3002',
    apiKey: process.env.API_KEY, // Optional
  },
  outputDirectory: "./output",
});
```

<AutoTypeTable name="default" type="export { NestedInkeepConfig as default } from '@inkeep/agents-cli/config'" />

<AutoTypeTable name="default" type="export { ApiConfig as default } from '@inkeep/agents-cli/config'" />
