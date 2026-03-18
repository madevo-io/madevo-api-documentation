# Getting Started

This section helps you quickly understand how to use the Madevo API and how the core pieces fit together.

---

## How the API works

The Madevo API is built around a few core concepts:

1. **Authenticate**
2. **Upload and define data (datasources)**
3. **Query or analyse data (assistant)**
4. **Automate workflows (agents)**
5. **Receive real-time updates (WebSocket)**

---

## Basic flow

A typical integration follows this flow:

### 1. Authenticate

Start by logging in to obtain a JWT:

```http
POST /api/v1/login
```

You will use this token for all secure endpoints.

---

### 2. Upload data

Upload a CSV or ZIP file:

```http
POST /api/v1/secure/datasource/upload
```

This returns:
- a storage filename
- detected columns
- a sample of your data

---

### 3. Create a datasource

Define how your data should be interpreted:

```http
PUT /api/v1/secure/datasource
```

This step:
- stores metadata
- defines schema (timefield, metafield, values)
- triggers background data import

---

### 4. Insert or query data

You can either:

- Insert rows directly:

```http
PATCH /api/v1/secure/datasource/insert
```

- Or query data:

```http
POST /api/v1/secure/data/plot
```

---

### 5. Use the assistant

Create a conversation:

```http
POST /api/v1/secure/assistant/conversation
```

Send a message:

```http
POST /api/v1/secure/assistant/chat
```

The assistant will:
- analyse your data
- generate insights
- optionally return queries and visualizations

---

### 6. Automate with agents

Create an agent task:

```http
POST /api/v1/secure/agent/task
```

Run it manually or schedule it:

```http
POST /api/v1/secure/agent/task/schedule
```

---

### 7. Listen for real-time updates

Connect to the WebSocket:

```text
GET /api/v1/stream/:room_id?token=<jwt>
```

Receive events for:
- assistant streaming
- datasource processing
- agent execution

---

## Key components

### Authentication
Handles login, token refresh, and session management.

### Datasources
Where your data is stored and structured.

### Assistant
Natural language interface for querying and analysing data.

### Agents
Automated workflows that run tasks using your data.

### WebSocket
Real-time event stream for async operations.

---

## What to read next

- **Authentication** to understand how to securely access the API
- **Response Format** to understand how responses are structured
- **Pagination** to handle large datasets
```
