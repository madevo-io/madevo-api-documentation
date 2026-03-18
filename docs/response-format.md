# Response Format

All Madevo API endpoints follow a consistent JSON response structure.

Understanding this format is important for handling both successful responses and errors.

---

## Standard response envelope

All secure endpoints return responses in the following structure:

```json
{
  "error": false,
  "message": "string",
  "...endpoint_specific_fields": "..."
}
```

### Fields

- `error`  
  Boolean indicating whether the request failed or succeeded

- `message`  
  Human-readable description of the result

- `...endpoint_specific_fields`  
  Additional fields depending on the endpoint

---

## Success response

When a request is successful:

```json
{
  "error": false,
  "message": "datasource created",
  "datasource_id": "65f0c0a01234567890abc222"
}
```

---

## Error response

When a request fails:

```json
{
  "error": true,
  "message": "invalid request body"
}
```

### Common error messages

- `unauthorized - invalid token`
- `invalid request body`
- `forbidden`
- `session expired`
- `invalid input`

---

## Partial success behavior

Some endpoints, especially data ingestion, may return an error even when part of the request succeeded.

### Example

```json
{
  "error": true,
  "message": "inserted 10 rows, skipped 2 rows: time field is empty at index 3"
}
```

### Important

- Valid rows may still be inserted even when `error = true`
- Always inspect the message to understand what succeeded and what failed

---

## Empty responses

Some endpoints may return empty arrays or zero values:

```json
{
  "error": false,
  "message": "users retrieved",
  "users": [],
  "total": 0
}
```

---

## Dynamic response fields

Some fields vary depending on the datasource or feature:

- `data` in query endpoints is dynamic
- `meta` objects may contain arbitrary key-value pairs
- assistant responses may include:
  - `query`
  - `dbquery`
  - `visualization`

### Example

```json
{
  "error": false,
  "message": "assistant response retrieved",
  "response": {
    "response": "Temperature increased after 11:55",
    "query": "What caused the spike?",
    "dbquery": "generated query",
    "visualization": "chart.png"
  }
}
```

---

## HTTP status vs API error

The API may return HTTP 200 even when:

```json
{
  "error": true
}
```

### Recommendation

Always check:

```text
error field in response body
```

Do not rely only on HTTP status codes.

---

## Best practices

- Always check the `error` field first
- Log the `message` for debugging
- Handle partial success cases explicitly
- Do not assume fixed schemas for dynamic fields
- Validate required fields before sending requests
