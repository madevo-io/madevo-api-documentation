# API Reference

This section contains the endpoint-by-endpoint reference for the Madevo secure API.

Use these pages when you need exact request formats, response shapes, required fields, and example calls.

---

## What is covered

The API reference documents customer-facing endpoints under:

```text
/api/v1/secure/*
```

It also includes the public authentication endpoints used to access secure routes:

- `POST /api/v1/login`
- `POST /api/v1/refresh-token`

And the real-time streaming endpoint:

- `/api/v1/stream/:room_id`

---

## How to use this section

Each page is grouped by feature area so you can quickly find the endpoints you need.

### Auth
Authentication, token refresh, and logout.

### Settings
Company-level settings and configuration retrieval.

### Context Files
Upload, list, and delete context files used for embedding and assistant context.

### Users
Current user lookup, user retrieval, and user listing.

### Companies
Company information associated with the authenticated user.

### Datasources
Datasource upload, creation, updates, inserts, metadata lookup, plotting, and deletion.

### Assistant
Conversation management, chat, streaming, history, suggestions, and generated visualizations.

### Agents
Agent task creation, execution, scheduling, deletion, and task history.

### WebSocket
Real-time events for assistant streaming, datasource updates, context file processing, and agent activity.

---

## Authentication

Unless stated otherwise, all endpoints in this section require:

```http
Authorization: Bearer <jwt>
```

If authentication fails, the API typically returns:

```json
{
  "error": true,
  "message": "unauthorized - invalid token"
}
```

---

## Response format

Most endpoints follow the standard response envelope:

```json
{
  "error": false,
  "message": "string",
  "...endpoint_specific_fields": "..."
}
```

For error responses:

```json
{
  "error": true,
  "message": "string"
}
```

Some endpoints may return partial-success style errors, especially during row insertion.

---

## Notes before you integrate

- Page-based pagination is zero-based where supported
- Default page size is typically 20
- Some endpoints paginate internally without exposing a page parameter
- Assistant history uses cursor-based pagination with `before=<message_id>`
- Some fields are dynamic and depend on datasource schema or stored metadata

---

## Recommended reading order

If you are new to the API, start with:

1. **Auth**
2. **Datasources**
3. **Assistant**
4. **Agents**
5. **WebSocket**

---

## Reference pages

- **Auth**
- **Settings**
- **Context Files**
- **Users**
- **Companies**
- **Datasources**
- **Assistant**
- **Agents**
- **WebSocket**
