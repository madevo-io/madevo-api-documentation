# WebSocket

The WebSocket API provides real-time updates for asynchronous operations such as assistant streaming, datasource processing, and agent execution.

---

## Endpoint

```text
GET /api/v1/stream/:room_id?token=<jwt>
```

### Example

```javascript
const ws = new WebSocket(
  `wss://api.example.com/api/v1/stream/any-room-id?token=${encodeURIComponent(jwt)}`
);

ws.onmessage = (event) => {
  const payload = JSON.parse(event.data);
  console.log(payload.event, payload.data);
};
```

---

## Authentication

Authentication is required via JWT.

### Method

Pass the token as a query parameter:

```text
?token=<jwt>
```

---

### Errors

Missing token:

```json
{
  "error": "token required"
}
```

Invalid or expired token:

```json
{
  "error": "invalid or expired token"
}
```

---

## Connection behavior

- Connection is tied to the authenticated user and company
- Server sends periodic ping frames (~45 seconds)
- Maximum inbound message size: 512 KB
- Client messages are currently ignored by the server

---

## Room behavior

- `room_id` is part of the URL but not used for authorization
- Subscriptions are determined by JWT claims:

```text
user:<user_id>
company:<company_id>
```

---

## Message format

All WebSocket messages follow this structure:

```json
{
  "event": "string",
  "data": {
    "string": "any"
  }
}
```

---

## Assistant streaming events

### chat-loading

```json
{
  "event": "chat-loading",
  "data": {
    "message_id": "string",
    "conversation_id": "string"
  }
}
```

---

### chat-chunk

```json
{
  "event": "chat-chunk",
  "data": {
    "message_id": "string",
    "conversation_id": "string",
    "chunk": "string"
  }
}
```

---

### chat-visualization-expect

```json
{
  "event": "chat-visualization-expect",
  "data": {
    "message_id": "string",
    "conversation_id": "string",
    "visualization": "plain:/loading_chart.png"
  }
}
```

---

### chat-visualization

```json
{
  "event": "chat-visualization",
  "data": {
    "message_id": "string",
    "conversation_id": "string",
    "visualization": "string"
  }
}
```

---

### chat-error

```json
{
  "event": "chat-error",
  "data": {
    "message_id": "string",
    "conversation_id": "string",
    "error": "string"
  }
}
```

---

### chat-complete

```json
{
  "event": "chat-complete",
  "data": {
    "message_id": "string",
    "conversation_id": "string"
  }
}
```

---

## Other events

### suggestions-updated

```json
{
  "event": "suggestions-updated",
  "data": {
    "conversation_id": "string"
  }
}
```

---

### context-file-status

```json
{
  "event": "context-file-status",
  "data": {
    "file_id": "string",
    "status": "ready | failed"
  }
}
```

---

### datasource-updated

```json
{
  "event": "datasource-updated",
  "data": {
    "datasource_id": "string",
    "action": "updated"
  }
}
```

---

### agent-task

```json
{
  "event": "agent-task",
  "data": {
    "id": "string",
    "action": "updated"
  }
}
```

---

## Usage patterns

### Assistant streaming

1. Call `/assistant/chatstream`
2. Receive `message_id`
3. Listen for:
   - `chat-loading`
   - `chat-chunk`
   - `chat-complete`

---

### File processing

- Upload context file
- Listen for `context-file-status`

---

### Agent execution

- Run or schedule task
- Listen for `agent-task` updates

---

## Best practices

- Always handle reconnect logic on the client
- Buffer streamed chunks before rendering final output
- Use `message_id` to track streams
- Do not rely on `room_id` for access control
- Expect events to arrive asynchronously and out of order
- Handle errors gracefully
