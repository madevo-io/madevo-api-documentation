# Errors

This section describes common error responses returned by the Madevo API and how to handle them.

---

## Error format

All errors follow the standard response structure:

```json
{
  "error": true,
  "message": "string"
}
```

---

## Common error messages

### Authentication errors

#### Unauthorized

```json
{
  "error": true,
  "message": "unauthorized - invalid token"
}
```

**Cause**

- Missing token  
- Invalid token  
- Expired token  

---

#### Session expired

```json
{
  "error": true,
  "message": "session expired"
}
```

**Cause**

- Token refresh attempted after session expiry  

---

### Authorization errors

#### Forbidden

```json
{
  "error": true,
  "message": "forbidden"
}
```

**Cause**

- Insufficient permissions  
- Role does not allow access  

---

### Validation errors

#### Invalid request body

```json
{
  "error": true,
  "message": "invalid request body"
}
```

**Cause**

- Missing required fields  
- Incorrect JSON structure  

---

#### Invalid input

```json
{
  "error": true,
  "message": "invalid input"
}
```

**Cause**

- Invalid parameter values  
- Unsupported formats  

---

### Datasource errors

#### Missing required fields

```json
{
  "error": true,
  "message": "time field is empty at index 0"
}
```

**Cause**

- Missing `timefield` or `metafield`  
- Invalid row data  

---

#### Partial success

```json
{
  "error": true,
  "message": "inserted 10 rows, skipped 2 rows"
}
```

**Cause**

- Some rows failed validation  

**Important**

- Valid rows are still inserted  
- You must inspect the message  

---

### File upload errors

#### Unsupported file type

```json
{
  "error": true,
  "message": "invalid input: unsupported file type image/png"
}
```

**Cause**

- File type not supported  

---

### Assistant errors

#### Assistant query failed

```json
{
  "error": true,
  "message": "assistant query failed"
}
```

**Cause**

- Internal processing error  
- Timeout or data issue  

---

### Agent errors

#### Invalid task request

```json
{
  "error": true,
  "message": "Incorrect agent task request"
}
```

**Cause**

- Missing or invalid fields  

---

## HTTP status vs API error

The API may return HTTP 200 even when:

```json
{
  "error": true
}
```

### Important

- Always check the `error` field  
- Do not rely only on HTTP status  

---

## Handling errors

### Recommended approach

1. Check `error` field  
2. Log the `message`  
3. Handle specific cases based on message  
4. Retry or correct input  

---

### Example

```javascript
if (response.error) {
  console.error(response.message);
}
```

---

## Best practices

- Validate inputs before sending requests  
- Handle partial success explicitly  
- Implement retry logic where appropriate  
- Log all error messages for debugging  
- Do not assume all failures are fatal  

---

## Debugging tips

- Check authentication first  
- Verify required fields  
- Confirm datasource configuration  
- Validate timestamps and formats  
- Inspect API response messages carefully  
