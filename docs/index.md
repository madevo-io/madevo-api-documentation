# Madevo API

Welcome to the Madevo secure API documentation.

This API enables you to interact with the Madevo platform programmatically, including managing data, running AI-powered analysis, and automating workflows using agents.

---

## What you can do with the API

With the Madevo API, you can:

- Authenticate users and manage sessions
- Upload and manage datasources
- Insert and query structured and time-series data
- Interact with the AI assistant for analysis and insights
- Create and manage autonomous agent tasks
- Receive real-time updates via WebSocket

---

## Base URL

All requests should be made to:

```
https://api.example.com
```

---

## Authentication

Most endpoints require a Bearer token.

1. Authenticate using:

```
POST /api/v1/login
```

2. Use the returned JWT in all secure requests:

```
Authorization: Bearer <jwt>
```

3. Refresh tokens using:

```
POST /api/v1/refresh-token
```

---

## Response format

All secure endpoints return responses in a consistent format:

```json
{
  "error": false,
  "message": "string",
  "...data": "..."
}
```

If an error occurs:

```json
{
  "error": true,
  "message": "string"
}
```

---

## Key concepts

### Datasources

Datasources are the foundation of the platform. They allow you to:

- Upload CSV or ZIP files
- Define schema and metadata
- Store and query structured or time-series data

### Assistant

The assistant allows you to:

- Ask natural language questions about your data
- Generate insights and summaries
- Produce queries and visualizations

### Agents

Agents are automated workflows that:

- Execute tasks using your datasources
- Run on-demand or on a schedule
- Produce structured outputs and insights

### WebSocket

The WebSocket API provides real-time updates for:

- Assistant streaming responses
- Datasource processing status
- Agent execution updates

---

## Quickstart

### 1. Login

```bash
curl -X POST https://api.example.com/api/v1/login \
  -H 'Content-Type: application/json' \
  -d '{
    "email": "user@example.com",
    "password": "Password1!"
  }'
```

### 2. Upload a datasource file

```bash
curl -X POST https://api.example.com/api/v1/secure/datasource/upload \
  -H 'Authorization: Bearer <jwt>' \
  -F 'file=@data.csv'
```

### 3. Create a datasource

```bash
curl -X PUT https://api.example.com/api/v1/secure/datasource \
  -H 'Authorization: Bearer <jwt>' \
  -H 'Content-Type: application/json' \
  -d '{ ... }'
```

### 4. Start a conversation

```bash
curl -X POST https://api.example.com/api/v1/secure/assistant/conversation \
  -H 'Authorization: Bearer <jwt>' \
  -H 'Content-Type: application/json' \
  -d '{ ... }'
```

---

## Next steps

- Start with **Getting Started → Authentication**
- Explore the **API Reference** for full endpoint details
- Follow **Guides** for common workflows
```
