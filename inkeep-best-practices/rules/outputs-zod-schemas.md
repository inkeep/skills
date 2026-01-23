---
title: Use Zod Schemas for Type Safety
impact: MEDIUM
impactDescription: improves reliability
tags: outputs, zod, schemas, type-safety
---

## Use Zod Schemas for Type Safety

Always define component props with Zod schemas for validation and type inference.

**Incorrect (plain object):**

```typescript
const card = dataComponent({
  name: 'Card',
  description: 'A card',
  props: {
    title: { type: 'string' },
    content: { type: 'string' }
  }
})
```

**Correct (Zod schema):**

```typescript
import { z } from 'zod'

const card = dataComponent({
  name: 'Card',
  description: 'A card',
  props: z.object({
    title: z.string().min(1).max(100),
    content: z.string(),
    priority: z.enum(['low', 'medium', 'high']).optional(),
    tags: z.array(z.string()).optional()
  })
})
```

Benefits of Zod schemas:
- Runtime validation of component data
- TypeScript type inference
- Clear documentation of expected shape
- Built-in validation (min, max, email, url, etc.)
- Optional fields with `.optional()`
- Enums for constrained values
