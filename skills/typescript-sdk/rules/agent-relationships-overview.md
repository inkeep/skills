---
title: "Sub Agent Relationships Overview"
description: "Transfer and delegation relationships between Sub Agents"
topic-path: "typescript-sdk/agent-relationships"
---

# Sub Agent Relationships Overview

Sub Agent relationships are used to coordinate specialized Sub Agents for complex workflows. This framework implements Sub Agent relationships through using `canDelegateTo()` and `canTransferTo()`, allowing a parent Sub Agent to automatically coordinate specialized Sub Agents for complex workflows.

## Understanding Sub Agent Relationships

The framework supports two types of Sub Agent relationships:

### Transfer Relationships

Transfer relationships **completely relinquish control** from one Sub Agent to another. When a Sub Agent hands off to another:

* The source Sub Agent stops processing
* The target Sub Agent takes full control of the conversation
* Control is permanently transferred until the target Sub Agent hands back

```typescript
import { subAgent, agent } from "agent-sdk";

// Create specialized Sub Agents first
const qaSubAgent = subAgent({
  id: "qa-agent",
  name: "QA Sub Agent",
  description: "Answers product and service questions",
  prompt: "Provide accurate information using available tools. Hand back to router if unable to help.",
  canUse: () => [knowledgeBaseTool],
  canTransferTo: () => [routerSubAgent],
});

const orderSubAgent = subAgent({
  id: "order-agent",
  name: "Order Sub Agent",
  description: "Handles order-related inquiries and actions",
  prompt: "Assist with order tracking, modifications, and management.",
  canUse: () => [orderSystemTool],
  canTransferTo: () => [routerSubAgent],
});

// Create router Sub Agent that coordinates other Sub Agents
const routerSubAgent = subAgent({
  id: "router-agent",
  name: "Router Sub Agent",
  description: "Routes customer inquiries to specialized Sub Agents",
  prompt: `Analyze customer inquiries and route them appropriately:
    - Product questions → Hand off to QA Sub Agent
    - Order issues → Hand off to Order Sub Agent
    - Complex issues → Handle directly or escalate`,
  canTransferTo: () => [qaSubAgent, orderSubAgent],
});

// Create the agent with router as default entry point
const supportAgent = agent({
  id: "customer-support-agent",
  defaultSubAgent: routerSubAgent,
  subAgents: () => [routerSubAgent, qaSubAgent, orderSubAgent],
  models: {
    base: {
      model: "anthropic/claude-sonnet-4-5",
      providerOptions: {
        temperature: 0.5,
      },
    },
    structuredOutput: {
      model: "openai/gpt-4.1-mini",
    },
  },
});
```

### Delegation Relationships

Delegation relationships are used to **pass a task** from one Sub Agent to another while maintaining oversight:

* The source Sub Agent remains in control
* The target Sub Agent executes a specific task
* Results are returned to the source Sub Agent
* The source Sub Agent continues processing

```typescript
// Sub Agents for specific tasks
const numberProducerA = subAgent({
  id: "number-producer-a",
  name: "Number Producer A",
  description: "Produces low-range numbers (0-50)",
  prompt: "Generate numbers between 0 and 50. Respond with a single integer.",
});

const numberProducerB = subAgent({
  id: "number-producer-b",
  name: "Number Producer B",
  description: "Produces high-range numbers (50-100)",
  prompt: "Generate numbers between 50 and 100. Respond with a single integer.",
});

// Coordinating Sub Agent that delegates tasks
const mathSupervisor = subAgent({
  id: "math-supervisor",
  name: "Math Supervisor",
  description: "Coordinates mathematical operations",
  prompt: `When given a math task:
    1. Delegate to Number Producer A for a low number
    2. Delegate to Number Producer B for a high number
    3. Add the results together and provide the final answer`,
  canDelegateTo: () => [numberProducerA, numberProducerB],
});

const mathAgent = agent({
  id: "math-delegation-agent",
  defaultSubAgent: mathSupervisor,
  subAgents: () => [mathSupervisor, numberProducerA, numberProducerB],
  models: {
    base: {
      model: "anthropic/claude-haiku-4-5",
      providerOptions: {
        temperature: 0.1,
      }
    }
  },
});
```
