# Assistant

The assistant provides a natural language interface for querying and analysing datasources.

It allows you to create conversations, send messages, retrieve history, and receive both synchronous and streaming responses.

---

## Overview

The assistant supports:

- Conversation management
- Synchronous chat responses
- Streaming responses via WebSocket
- Conversation history
- Suggested questions
- Generated visualizations

---

## Key concepts

### Conversation

A conversation is linked to:
- a datasource
- a user
- a company

It maintains context across messages.

---

### Chat flow

1. Create a conversation  
2. Send a message  
3. Receive response  
4. Optionally stream response via WebSocket  

---

## GET /api/v1/secure/assistant/conversations

List all conversations.

### Authentication required

Bearer JWT

---

### Response

```json
{
  "error": false,
  "message": "conversations retrieved",
  "conversations": [],
  "total": 0
}
```

---

### Notes

- Pagination exists internally but is not exposed  
- Default page size is 20  

---

## GET /api/v1/secure/assistant/conversation/:id

Get a single conversation.

### Path params

- `id` (required)

---

### Response

```json
{
  "error": false,
  "message": "conversation retrieved",
  "conversation": {
    "_id": "string",
    "title": "string",
    "datasource_id": "string",
    "status": "string",
    "created_at": "string",
    "updated_at": "string"
  }
}
```

---

## POST /api/v1/secure/assistant/conversation

Create a new conversation.

### Body

```json
{
  "title": "string",
  "datasource_id": "string",
  "context_only": false
}
```

---

### Response

```json
{
  "error": false,
  "message": "conversation history retrieved",
  "id": "string"
}
```

---

### Notes

- `title` must not be empty  
- `context_only` defaults to false  

---

## PUT /api/v1/secure/assistant/conversation

Update a conversation.

### Body

```json
{
  "id": "string",
  "title": "string",
  "datasource_id": "string",
  "pinned": true,
  "context_only": false
}
```

---

## DELETE /api/v1/secure/assistant/conversation

Delete a conversation.

### Body

```json
{
  "id": "string"
}
```

---

## GET /api/v1/secure/assistant/history

Retrieve conversation history.

### Query params

- `conversation_id` (required)
- `before` (optional)

---

### Response

```json
{
  "error": false,
  "message": "conversation history retrieved",
  "history": [],
  "has_more": false
}
```

---

### Notes

- Returns up to 20 messages  
- Use `before` for pagination  
- Results are chronological  

---

## POST /api/v1/secure/assistant/chat

Send a message and receive a response.

### Body

```json
{
  "conversation_id": "string",
  "message": "string",
  "skip_history": false,
  "skip_save_history": false,
  "context_only": false
}
```

---

### Response

```json
{
  "error": false,
  "message": "assistant response retrieved",
  "response": {
    "response": "string",
    "query": "string",
    "dbquery": "string",
    "visualization": "string"
  }
}
```

---

### Notes

- Timeout: ~2 minutes  
- Conversation status changes during processing  
- `skip_save_history` prevents storing messages  
- `skip_history` exists but is not used  

---

## POST /api/v1/secure/assistant/chatstream

Start streaming response.

### Body

```json
{
  "conversation_id": "string",
  "message": "string"
}
```

---

### Response

```json
{
  "error": false,
  "message": "assistant message stream started",
  "message_id": "string"
}
```

---

### Notes

- Actual response is delivered via WebSocket  
- Use `message_id` to track the stream  

---

## GET /api/v1/secure/assistant/suggestionsquery

Get suggested questions.

### Query params

- `conversation_id`

---

### Response

```json
{
  "error": false,
  "message": "suggestions query retrieved",
  "suggested_questions": "string"
}
```

---

### Notes

- Returned as JSON string, not parsed array  

---

## GET /api/v1/secure/assistant/suggestionsquery/reload

Trigger regeneration of suggestions.

### Query params

- `conversation_id`

---

### Response

```json
{
  "error": false,
  "message": "suggestions reloading started"
}
```

---

## GET /api/v1/secure/assistant/visualizations/:filename

Retrieve visualization file.

### Response

- Returns binary image  

---

## WebSocket integration

Used for:

- streaming responses  
- assistant events  

Connection:

```text
wss://api.example.com/api/v1/stream/:room_id?token=<jwt>
```

---

### Events

- `chat-loading`
- `chat-chunk`
- `chat-visualization`
- `chat-error`
- `chat-complete`

---

## Notes

- Conversations move between states: `ready`, `loading`  
- Some fields may be missing or dynamic  
- Assistant responses may include generated queries  

---

## Best practices

- Always create a conversation before sending messages  
- Use streaming for better UX  
- Handle partial or delayed responses  
- Do not rely on fixed response schema  
```
