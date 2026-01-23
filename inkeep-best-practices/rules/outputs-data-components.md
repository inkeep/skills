---
title: Use Data Components for Rich UI
impact: MEDIUM
impactDescription: improves user experience
tags: outputs, data-components, ui, structured
---

## Use Data Components for Rich UI

Data components allow sub agents to output structured data that renders as rich UI elements.

**Example:**

```typescript
import { z } from 'zod'

const productCard = dataComponent({
  name: 'Product Card',
  description: 'Display a product with image, price, and buy button',
  props: z.object({
    name: z.string(),
    price: z.number(),
    imageUrl: z.string().url(),
    inStock: z.boolean(),
    rating: z.number().min(0).max(5).optional()
  }),
  render: {
    component: 'ProductCard',
    mockData: { 
      name: 'Widget', 
      price: 29.99, 
      imageUrl: 'https://example.com/widget.jpg', 
      inStock: true,
      rating: 4.5
    }
  }
})

const shopAgent = subAgent({
  id: 'shop',
  name: 'Shopping Assistant',
  instructions: `
    Help users find products. 
    Use the Product Card to display items you recommend.
    Always show the product card when discussing a specific product.
  `,
  dataComponents: () => [productCard]
})
```

Data components enable:
- Rich, interactive UI elements in chat
- Consistent rendering across platforms
- Type-safe structured data
- Better user experience than plain text
