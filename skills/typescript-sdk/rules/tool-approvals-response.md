---
title: "Responding to Tool Approval Requests"
description: "How to approve or deny tool execution requests"
topic-path: "typescript-sdk/tools/tool-approvals"
---

# Responding to Tool Approval Requests

## Responding to an approval request

You can approve/deny in two ways.

### Option A: Dedicated endpoint

`POST /run/api/tool-approvals` (or `POST /api/tool-approvals` if your Run API is not mounted under `/run`).

```json
{
  "conversationId": "conv_xyz789",
  "toolCallId": "call_abc123def456",
  "approved": true,
  "reason": "User confirmed the operation" // Optional
}
```

### Option B: Respond via the chat endpoint (message part)

This is useful if your UI already models tool parts (for example, using the Vercel AI SDK UI message stream).

`POST /run/api/chat` with an assistant message that includes a `tool-*` part:

```json
{
  "conversationId": "conv_xyz789",
  "messages": [
    {
      "role": "assistant",
      "content": null,
      "parts": [
        {
          "type": "tool-get_weather_forecast",
          "toolCallId": "call_abc123def456",
          "state": "approval-responded",
          "approval": {
            "id": "aitxt-call_abc123def456",
            "approved": true,
            "reason": "User confirmed"
          }
        }
      ]
    }
  ]
}
```

### Vercel AI SDK (`useChat`) example

If you’re using the Vercel AI SDK UI message stream, approval requests show up as `tool-*` parts (e.g. `tool-getWeather`) with `state: "approval-requested"`.

After calling `addToolApprovalResponse(...)`, call `sendMessage()` so the updated messages (including the approval response) are POSTed back to `/run/api/chat` and the run can continue.

```tsx
'use client';

import { useChat } from '@ai-sdk/react';

export default function Chat() {
  const { messages, addToolApprovalResponse, sendMessage } = useChat();

  return (
          {messages.map((message) => (
        <div key={message.id}>
          {message.parts.map((part) => {
            if (part.type === 'tool-getWeather') {
              switch (part.state) {
                case 'approval-requested':
                  return (
                    <div key={part.toolCallId}>
                      <p>Get weather for {part.input.city}?</p>
                      <button
                        type="button"
                        onClick={() => {
                          addToolApprovalResponse({
                            id: part.approval.id,
                            approved: true,
                          });
                          sendMessage();
                        }}
                      >
                        Approve
                      </button>
                      <button
                        type="button"
                        onClick={() => {
                          addToolApprovalResponse({
                            id: part.approval.id,
                            approved: false,
                          });
                          sendMessage();
                        }}
                      >
                        Deny
                      </button>
                    </div>
                  );
                case 'output-available':
                  return (
                    <div key={part.toolCallId}>
                      Weather in {part.input.city}: {String(part.output)}
                    </div>
                  );
              }
            }

            return null;
          })}
        </div>
      ))}
    </>
  );
}
```

<Note>
  Only `state: "approval-responded"` triggers an approval/denial. If your client includes other tool approval parts in message
  history (for example `state: "approval-requested"`), the server ignores them.
</Note>

## Related docs

* [Chat API](/talk-to-your-agents/chat-api)
* [Data operations](/typescript-sdk/data-operations)
