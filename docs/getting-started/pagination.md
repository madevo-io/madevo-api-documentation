# Pagination

The Madevo API uses both **page-based** and **cursor-based** pagination depending on the endpoint.

---

## Page-based pagination

Most list endpoints use page-based pagination.

### Parameters

- `page`  
  Integer, zero-based index  
  Default: `0`

### Behavior

- Page numbering starts from **0**
- Default page size is **20 items**
- Results are typically sorted by newest first

---

### Example request

```bash
curl 'https://api.example.com/api/v1/secure/datasources?page=0' \
  -H 'Authorization: Bearer <jwt>'
```

### Example response

```json
{
  "error": false,
  "message": "datasources retrieved",
  "datasources": [],
  "total": 0
}
```

---

## Missing pagination parameters

Some endpoints paginate internally but do not expose a `page` parameter.

### Important

- The API still returns paginated results
- You may not be able to request specific pages

### Examples

- `/api/v1/secure/user/list`
- `/api/v1/secure/assistant/conversations`
- `/api/v1/secure/agent/tasks`

---

## Cursor-based pagination

Assistant history uses cursor-based pagination.

### Parameters

- `conversation_id` (required)
- `before` (optional, message ID cursor)

### Behavior

- Results are returned in batches of **20**
- Use `before=<message_id>` to fetch older messages
- Results are returned in **chronological order (oldest to newest within the page)**

---

### Example request

```bash
curl 'https://api.example.com/api/v1/secure/assistant/history?conversation_id=<id>' \
  -H 'Authorization: Bearer <jwt>'
```

### Example response

```json
{
  "error": false,
  "message": "conversation history retrieved",
  "history": [],
  "has_more": false
}
```

---

## has_more field

- `has_more = true` means more records are available
- `has_more = false` means you reached the end

### Important

- `has_more` becomes `true` only when exactly **20 records** are returned

---

## Page size behavior

- Default page size: **20**
- Some endpoints allow custom `page_size`
- Others use a fixed size internally

---

## Invalid page values

- If `page` cannot be parsed, the API defaults to `0`
- Negative or invalid values may not be validated consistently

---

## Best practices

- Always start with `page = 0`
- Use `total` to understand dataset size
- Use cursor pagination for assistant history
- Do not assume all endpoints support pagination parameters
- Handle empty responses gracefully
