---
title: Use Function Tools for Custom Logic
impact: MEDIUM
impactDescription: enables custom capabilities
tags: tools, function-tools, custom, sandbox
---

## Use Function Tools for Custom Logic

Function tools allow custom JavaScript that runs in a secure sandbox. Use them for business logic that doesn't need an external MCP server.

**Example:**

```typescript
const calculateDiscount = functionTool({
  name: 'calculate_discount',
  description: 'Calculate discount based on customer tier and order value',
  inputSchema: {
    type: 'object',
    properties: {
      customerTier: { type: 'string', enum: ['bronze', 'silver', 'gold'] },
      orderValue: { type: 'number' }
    },
    required: ['customerTier', 'orderValue']
  },
  execute: async ({ customerTier, orderValue }) => {
    const discounts = { bronze: 0.05, silver: 0.10, gold: 0.15 }
    const discount = discounts[customerTier] * orderValue
    return { discount, finalPrice: orderValue - discount }
  }
})
```

Function tools are ideal for:
- Business logic calculations
- Data transformations
- Validation logic
- Simple integrations without standing up an MCP server

You can also specify `dependencies` for npm packages to use in the sandbox:

```typescript
const formatDate = functionTool({
  name: 'format_date',
  description: 'Format a date string',
  inputSchema: { /* ... */ },
  dependencies: {
    'date-fns': '^3.0.0'
  },
  execute: async ({ date, format }) => {
    const { format: formatFn } = await import('date-fns')
    return formatFn(new Date(date), format)
  }
})
```
