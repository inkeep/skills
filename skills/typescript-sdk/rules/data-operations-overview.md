---
title: "Data Operations Overview"
description: "How to enable and use data operations for debugging and monitoring"
topic-path: "typescript-sdk/data-operations"
---

# Data Operations Overview

## Overview

Data operations are detailed, real-time events that provide visibility into what agents are doing during execution. They include agent reasoning, tool executions, transfers, delegations, and artifact creation. By default, these operations are hidden from end users to keep the interface clean, but they can be enabled for debugging and monitoring purposes.

## The x-emit-operations Header

The `x-emit-operations` header controls whether data operations are included in the response stream. When set to `true`, the system will emit detailed operational events alongside the regular response content.

### Usage

```bash
curl -N \
  -X POST "http://localhost:3003/api/chat" \
  -H "Authorization: Bearer $INKEEP_API_KEY" \
  -H "Content-Type: application/json" \
  -H "x-emit-operations: true" \
  -d '{
    "messages": [
      { "role": "user", "content": "What can you do?" }
    ],
    "conversationId": "chat-1234"
  }'
```

### CLI Usage

In the CLI, you can toggle data operations using the `operations` command:

```bash
# Start a chat session
inkeep chat

# Toggle data operations on/off
> operations
🔧 Emit operations: ON
Data operations will be shown during responses.

> operations
🔧 Emit operations: OFF
Data operations are hidden.
```
