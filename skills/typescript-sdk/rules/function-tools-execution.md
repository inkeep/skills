---
title: "Function Tools Execution Environment"
description: "Sandbox providers, runtime configuration, and security guarantees"
topic-path: "typescript-sdk/tools/function-tools"
---

# Function Tools Execution Environment

## Execution Environment

Function tools run in secure, isolated sandboxes that provide a safe execution environment for your code.

### Sandbox Providers

The framework supports multiple sandbox providers:

* **Native** (default) - Uses Node.js child processes, works in most environments including cloud VMs, Docker, Kubernetes, and traditional hosting
* **Vercel** - Uses Vercel Sandbox MicroVMs for serverless environments where child process spawning is restricted (Vercel, AWS Lambda, etc.)

The sandbox provider is configured at the application level when deploying. See [Deploy to Vercel](/deployment/vercel#step-2-configure-sandbox-in-your-application) for serverless deployment configuration.

### Runtime Configuration

Configure execution settings in your application:

```typescript
import { createAgentsApp } from "@inkeep/agents-api";

const app = createAgentsApp({
  sandboxConfig: {
    provider: "native", // or 'vercel' for serverless
    runtime: "node22", // Node.js 22 or 'typescript'
    timeout: 30000, // 30 second timeout
    vcpus: 2, // Max 2 concurrent executions
  },
});
```

### Security & Isolation

Function tools execute with strong security guarantees:

* **Isolated execution** - Each function runs in its own sandbox
* **No file system access** - Cannot read or write outside the sandbox
* **Network restrictions** - Only through explicitly declared dependencies
* **Resource limits** - CPU, memory, and execution time constraints enforced
* **No state persistence** - Functions are stateless between executions

## Tool Output Pipelines

Agents can pass the output of one tool directly as the input to another — no artifact creation required. This is useful when intermediate results are purely processing steps that don't need to be saved or shown to the user.

For example, a pipeline that fetches a webpage and then processes its content:

```typescript
const fetchPage = functionTool({
  name: "fetch-page",
  description: "Fetch the raw HTML content of a URL",
  inputSchema: {
    type: "object",
    properties: {
      url: { type: "string", description: "URL to fetch" },
    },
    required: ["url"],
  },
  execute: async ({ url }) => {
    const response = await fetch(url);
    return await response.text();
  },
});

const extractText = functionTool({
  name: "extract-text",
  description: "Extract readable text from HTML content",
  inputSchema: {
    type: "object",
    properties: {
      html: { type: "string", description: "HTML content to extract text from" },
    },
    required: ["html"],
  },
  execute: async ({ html }) => {
    // Strip tags and return plain text
    return html.replace(/<[^>]+>/g, " ").replace(/\s+/g, " ").trim();
  },
});
```

When the agent calls `fetch-page` and then `extract-text`, it passes the HTML output directly into the next call. The intermediate HTML never surfaces to the user. Design tools meant to be used in sequence so their return types match the input types of downstream tools.

### Best Practices

* **Keep functions focused** - Each function should do one thing well
* **Minimize dependencies** - Only include packages you actually need
* **Handle errors gracefully** - Always catch and return meaningful errors
* **Test independently** - Functions should be testable in isolation
* **Document thoroughly** - Clear descriptions help agents use tools effectively
* **Design for composability** - When tools form a pipeline, align return types with downstream input types
