# WebSocket Events

This section lists all known WebSocket events and their payloads.

All events follow the same structure:

```json
{
  "event": "string",
  "data": {
    "string": "any"
  }
}
```

---

## Assistant events

### chat-loading

Indicates that assistant processing has started.

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

A partial piece of the assistant response.

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

Indicates that a visualization is being prepared.

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

Indicates that the final visualization is ready.

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

Indicates an error during assistant processing.

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

Marks the end of the assistant response stream.

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

## Suggestions events

### suggestions-updated

Triggered when suggested questions are updated.

```json
{
  "event": "suggestions-updated",
  "data": {
    "conversation_id": "string"
  }
}
```

---

## Context file events

### context-file-status

Indicates processing status of a context file.

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

## Datasource events

### datasource-updated

Triggered when a datasource is updated.

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

## Agent events

### agent-task

Triggered when an agent task changes state.

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

## Event usage patterns

### Assistant streaming

1. Receive `chat-loading`
2. Receive multiple `chat-chunk`
3. Optionally receive visualization events
4. Receive `chat-complete`

---

### File processing

- Upload file
- Wait for `context-file-status`

---

### Agent execution

- Run or schedule agent
- Listen for `agent-task`

---

## Best practices

- Always handle events asynchronously
- Use `message_id` to track responses
- Buffer `chat-chunk` events before rendering
- Handle `chat-error` gracefully
- Do not assume event order is guaranteed
```
