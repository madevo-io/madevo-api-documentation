# Stream Assistant Responses

This guide explains how to receive assistant responses in real time using streaming and WebSocket.

---

## Overview

Streaming allows you to:

- Receive assistant responses progressively
- Improve user experience with live updates
- Display partial results while processing continues

---

## How it works

Streaming uses two parts:

1. HTTP request to start the stream  
2. WebSocket connection to receive events  

---

## Step 1: Start streaming request

### Endpoint

```http
POST /api/v1/secure/assistant/chatstream
```

---

### Example

```bash
curl -X POST https://api.example.com/api/v1/secure/assistant/chatstream \
  -H 'Authorization: Bearer <jwt>' \
  -H 'Content-Type: application/json' \
  -d '{
    "conversation_id": "<conversation_id>",
    "message": "Analyse the temperature trend"
  }'
```

---

### Response

```json
{
  "error": false,
  "message": "assistant message stream started",
  "message_id": "message_id"
}
```

---

### Important

- `message_id` identifies the streaming response  
- The actual response is NOT returned here  
- You must use WebSocket to receive it  

---

## Step 2: Connect to WebSocket

### Endpoint

```text
wss://api.example.com/api/v1/stream/:room_id?token=<jwt>
```

---

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

## Step 3: Handle streaming events

You will receive a sequence of events.

---

### chat-loading

Indicates processing has started.

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

Partial response chunk.

```json
{
  "event": "chat-chunk",
  "data": {
    "message_id": "string",
    "conversation_id": "string",
    "chunk": "partial text"
  }
}
```

---

### chat-visualization-expect

Indicates a visualization is being prepared.

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

Final visualization is ready.

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

Indicates an error during processing.

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

Marks the end of the stream.

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

## Step 4: Build the full response

To reconstruct the final message:

1. Listen for `chat-chunk` events  
2. Append each `chunk` to a buffer  
3. Stop when `chat-complete` is received  

---

## Example flow

1. Call `chatstream`  
2. Receive `message_id`  
3. Connect to WebSocket  
4. Receive:
   - `chat-loading`
   - multiple `chat-chunk`
   - optional visualization events
   - `chat-complete`  

---

## Common mistakes

### Not connecting WebSocket

- You will not receive any data without WebSocket  

---

### Ignoring message_id

- Needed to track the correct stream  

---

### Rendering chunks incorrectly

- Always append in order  
- Do not overwrite previous chunks  

---

## Best practices

- Buffer chunks before displaying final output  
- Handle reconnect logic  
- Show loading indicators on `chat-loading`  
- Handle `chat-error` gracefully  
- Use streaming for long or complex queries  

---

## When to use streaming

Use streaming when:

- Responses are large  
- You want real-time UI updates  
- You need better perceived performance  

Use normal chat when:

- Responses are short  
- Simplicity is preferred  

---

## Next steps

- Use assistant for real-time analytics  
- Combine streaming with visualizations  
- Integrate into UI dashboards  
