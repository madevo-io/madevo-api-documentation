# Response Format

All Madevo API endpoints follow a consistent JSON response structure.

Understanding this format helps you correctly handle both successful responses and errors.

---

## Standard response structure

All secure endpoints return responses in the following format:

```json
{
  "error": false,
  "message": "string",
  "...endpoint_specific_fields": "..."
}
```

---

## Fields

- `error`  
  Indicates whether the request failed or succeeded  

- `message`  
  Human-readable description of the result  

- `...endpoint_specific_fields`  
  Additional data specific to the endpoint  

---

## Success response

Example:

```json
{
  "error": false,
  "message": "datasource created",
  "datasource_id": "65f0c0a01234567890abc222"
}
```

---

## Error response

Example:

```json
{
  "error": true,
  "message": "invalid request body"
}
```

---

## Common error messages

- `unauthorized - invalid token`
- `invalid request body`
- `invalid input`
- `forbidden`
- `session expired`

---

## Partial success behavior

Some endpoints, especially data ingestion, may return an error even when part of the operation succeeded.

### Example

```json
{
  "error": true,
  "message": "inserted 10 rows, skipped 2 rows"
}
```

---

### Important

- Some data may still be processed successfully  
- Always inspect the `message` field  

---

## Empty responses

Some endpoints may return empty results:

```json
{
  "error": false,
  "message": "users retrieved",
  "users": [],
  "total": 0
}
```

---

## Dynamic fields

Some response fields vary depending on context:

- Datasource query results  
- Metadata fields  
- Assistant responses  

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

---

### Important

- Always check the `error` field  
- Do not rely only on HTTP status codes  

---

## Best practices

- Check `error` before processing response  
- Log the `message` for debugging  
- Handle partial success cases explicitly  
- Do not assume fixed schemas for dynamic fields  
