# Start an Assistant Conversation

This guide shows how to create a conversation and interact with the assistant to analyse your data.

---

## Overview

You will:

1. Create a conversation  
2. Send a message  
3. Receive a response  
4. (Optional) Retrieve history  

---

## Step 1: Create a conversation

A conversation is required before sending messages.

### Endpoint

```http
POST /api/v1/secure/assistant/conversation
```

---

### Example

```bash
curl -X POST https://api.example.com/api/v1/secure/assistant/conversation \
  -H 'Authorization: Bearer <jwt>' \
  -H 'Content-Type: application/json' \
  -d '{
    "title": "Investigate motor temperature",
    "datasource_id": "<datasource_id>",
    "context_only": false
  }'
```

---

### Response

```json
{
  "error": false,
  "message": "conversation history retrieved",
  "id": "conversation_id"
}
```

---

### Important fields

- `id` → conversation ID (used in next step)

---

## Step 2: Send a message

Send a natural language query to the assistant.

### Endpoint

```http
POST /api/v1/secure/assistant/chat
```

---

### Example

```bash
curl -X POST https://api.example.com/api/v1/secure/assistant/chat \
  -H 'Authorization: Bearer <jwt>' \
  -H 'Content-Type: application/json' \
  -d '{
    "conversation_id": "<conversation_id>",
    "message": "What caused the temperature spike?",
    "skip_history": false,
    "skip_save_history": false,
    "context_only": false
  }'
```

---

### Response

```json
{
  "error": false,
  "message": "assistant response retrieved",
  "response": {
    "response": "The spike started after 11:55 and was concentrated on motor-1.",
    "query": "What caused the temperature spike?",
    "dbquery": "generated query",
    "visualization": "chart.png"
  }
}
```

---

## Step 3: Interpret the response

The assistant response includes:

- `response` → human-readable answer  
- `query` → interpreted user query  
- `dbquery` → generated data query (if available)  
- `visualization` → optional visualization reference  

---

## Step 4: Retrieve conversation history (optional)

### Endpoint

```http
GET /api/v1/secure/assistant/history
```

---

### Example

```bash
curl 'https://api.example.com/api/v1/secure/assistant/history?conversation_id=<conversation_id>' \
  -H 'Authorization: Bearer <jwt>'
```

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

## Optional: Stream responses

Instead of waiting for full responses, you can stream results using:

```http
POST /api/v1/secure/assistant/chatstream
```

Then listen via WebSocket.

---

## Common mistakes

### Missing conversation

- You must create a conversation before sending messages  

---

### Invalid datasource

- Ensure `datasource_id` exists and belongs to your company  

---

### Ignoring response fields

- Important data may be in `dbquery` or `visualization`  

---

## Best practices

- Use clear, specific questions  
- Keep conversation context focused  
- Use `context_only` when you want data-only responses  
- Use streaming for better user experience  
- Store conversation IDs for reuse  

---

## Example workflow

1. Create datasource  
2. Create conversation  
3. Ask questions  
4. Analyse results  
5. Continue conversation  

---

## Next steps

- Stream assistant responses  
- Use context files to enrich answers  
- Automate analysis using agents  
