---
title: Limit Tools to 5-7 Per Sub Agent
impact: HIGH
impactDescription: reduces confusion and improves tool selection
tags: subagent, tools, design, best-practice
---

## Limit Tools to 5-7 Per Sub Agent

As a general rule, keep sub agents to 5-7 related tools. Too many tools overwhelm the model and lead to poor tool selection.

**Incorrect (too many unrelated tools):**

```typescript
const overloadedAgent = subAgent({
  id: 'overloaded',
  name: 'Overloaded Agent',
  instructions: 'Help users with their requests.',
  canUse: () => [
    githubMcp,
    slackMcp,
    linearMcp,
    stripeMcp,
    twilioMcp,
    sendgridMcp,
    algoliaSearch,
    pineconeSearch,
    postgresDb,
    redisCache,
    s3Storage,
    cloudflareWorkers,
  ]
})
```

**Correct (focused tool set):**

```typescript
const codeReviewAgent = subAgent({
  id: 'code-review',
  name: 'Code Review Agent',
  instructions: `
    Review pull requests and provide feedback.
    Create issues for follow-up work when needed.
  `,
  canUse: () => [
    githubMcp.with({ selectedTools: ['get_pull_request', 'create_review', 'create_issue'] }),
    linearMcp.with({ selectedTools: ['create_issue', 'search_issues'] })
  ]
})
```

If you need more tools, consider breaking into multiple sub agents with delegation relationships.
