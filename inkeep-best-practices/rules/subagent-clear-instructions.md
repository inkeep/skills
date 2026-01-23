---
title: Write Clear, Specific Instructions
impact: HIGH
impactDescription: directly affects agent behavior quality
tags: subagent, instructions, prompts, design
---

## Write Clear, Specific Instructions

Instructions (prompts) should be specific about what the agent does, how it should behave, and what it should avoid.

**Incorrect (vague instructions):**

```typescript
const vagueAgent = subAgent({
  id: 'vague-agent',
  name: 'Helper',
  instructions: 'Help the user with their questions.'
})
```

**Correct (specific, actionable instructions):**

```typescript
const supportAgent = subAgent({
  id: 'support-agent',
  name: 'Technical Support',
  instructions: `
    You are a technical support agent for Acme Software.
    
    Your responsibilities:
    - Answer questions about product features and usage
    - Help troubleshoot common issues
    - Search documentation for relevant information
    
    Guidelines:
    - Always be polite and professional
    - If you don't know something, say so rather than guessing
    - For billing or account issues, transfer to the billing agent
    - For bug reports, create a ticket and provide the ticket number
    
    You have access to the documentation search tool. Use it to find
    relevant articles before answering technical questions.
  `
})
```

Good instructions include:
- Clear role definition
- Specific responsibilities
- Behavioral guidelines
- Edge case handling
- Tool usage guidance
