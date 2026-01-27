---
title: "Environment-Aware MCP Servers"
description: "Configure MCP tools to switch based on environment (dev vs production)"
topic-path: "typescript-sdk/tools/mcp-tools"
---

# Environment-Aware MCP Servers

## Environment-Aware MCP Servers

You can also configure MCP tools to switch based on your environment. This is useful when you want to use different MCP tool configurations for development vs production.

Creating environment-aware MCP tools is a two step process:

### Step 1: Define environment configurations

<Tabs>
  <Tab title="Development Config">
    ```typescript title="projects/my-project/environments/development.env.ts"
    import { registerEnvironmentSettings, mcpTool } from '@inkeep/agents-sdk';
    import { CredentialStoreType } from '@inkeep/agents-core';

    export const development = registerEnvironmentSettings({
    mcpServers: {
    'customer_support_tool': mcpTool({
      id: 'customer-support-tool',
      name: 'Customer Support Tool',
      description: 'Customer Support Tool for development',
      serverUrl: 'http://localhost:3000/customer-support/mcp',
    })
    },
    });
    ```
  </Tab>

  <Tab title="Production Config">
    ```typescript title="projects/my-project/environments/production.env.ts"
    import { registerEnvironmentSettings, mcpTool } from '@inkeep/agents-sdk';
    import { CredentialStoreType } from '@inkeep/agents-core';

    export const production = registerEnvironmentSettings({
    mcpServers: {
    'customer_support_tool': mcpTool({
      id: 'customer-support-tool',
      name: 'Customer Support Tool',
      description: 'Customer Support Tool for production',
      serverUrl: 'https://customer-support.example.com/mcp',
    })
    },
    });
    ```
  </Tab>

  <Tab title="Index File">
    ```typescript title="projects/my-project/environments/index.ts"
    import { createEnvironmentSettings } from '@inkeep/agents-sdk';
    import { development } from './development.env';
    import { production } from './production.env';

    export const envSettings = createEnvironmentSettings({
    development,
    production,
    });
    ```
  </Tab>
</Tabs>

### Step 2: Use the MCP tool in your sub agent

```typescript title="projects/my-project/agents/customer-support-agent.ts"
import { subAgent } from "@inkeep/agents-sdk";
import { envSettings } from "../environments";

const customerSupportAgent = subAgent({
  id: "customer-support-agent",
  name: "Customer Support Agent",
  description: "Responsible for customer support",
  prompt: "You are a helpful assistant",
  canUse: () => [envSettings.getEnvironmentMcp('customer_support_tool')],
});
```

This pattern is useful if you want to keep track of different credentials for different environments. When you push your project using the [Inkeep CLI](/typescript-sdk/cli-reference#inkeep-push) `inkeep push` command with the `--env` flag, the credentials will be loaded from the appropriate environment file. For example, if you run `inkeep push --env development`, the credentials will be loaded from the `environments/development.env.ts` file.

### CLI Environment Variables

The CLI respects these environment variables when using the `--env` flag:

```bash
# Set environment name via environment variable
export INKEEP_ENV=production
inkeep push  # Uses production environment automatically

# Override via CLI (takes precedence)
inkeep push --env development  # Uses development instead
```
