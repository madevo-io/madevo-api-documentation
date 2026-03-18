# Insert Data into a Datasource

This guide explains how to insert data directly into an existing datasource.

---

## Overview

You will:

1. Identify your datasource  
2. Prepare your data rows  
3. Send an insert request  
4. Handle partial success responses  

---

## Step 1: Get datasource details

Before inserting data, confirm:

- `datasource_id`
- `timefield`
- `metafield`
- `values`

### Example

```bash
curl 'https://api.example.com/api/v1/secure/datasource?id=<datasource_id>' \
  -H 'Authorization: Bearer <jwt>'
```

---

## Step 2: Prepare your data

Data must be sent as an array of rows.

Each row must be a map of string keys and values.

---

### Time-series example

```json
{
  "datasource_id": "65f0c0a01234567890abc222",
  "data": [
    {
      "timestamp": "2026-03-18 12:00:00",
      "asset": "motor-1",
      "temperature": "73.4"
    }
  ]
}
```

---

### Required fields

For time-series datasources:

- `timefield` (e.g. `timestamp`)
- `metafield` (e.g. `asset`)
- all fields listed in `values`

---

### Non-time-series

- `metafield` is still required  
- timestamp is generated automatically  

---

## Step 3: Send insert request

### Endpoint

```http
PATCH /api/v1/secure/datasource/insert
```

---

### Example

```bash
curl -X PATCH https://api.example.com/api/v1/secure/datasource/insert \
  -H 'Authorization: Bearer <jwt>' \
  -H 'Content-Type: application/json' \
  -d '{
    "datasource_id": "65f0c0a01234567890abc222",
    "data": [
      {
        "timestamp": "2026-03-18 12:00:00",
        "asset": "motor-1",
        "temperature": "73.4"
      }
    ]
  }'
```

---

## Step 4: Handle response

### Success

```json
{
  "error": false,
  "message": "datasource updated",
  "inserted_rows": 1
}
```

---

### Partial success

```json
{
  "error": true,
  "message": "inserted 10 rows, skipped 2 rows: time field is empty at index 1"
}
```

---

## Important behavior

- Valid rows are inserted even if `error = true`
- You must inspect the `message` to understand what happened
- Some rows may fail validation and be skipped

---

## Validation rules

- Max **1000 rows per request**
- Each row must include required fields
- Timestamp must match `timeformat`
- All values must be strings

---

## Common mistakes

### Missing timefield

- Required for time-series datasources  
- Missing values will cause row to be skipped  

---

### Missing metafield

- Required for all datasources  
- Rows without it are skipped  

---

### Incorrect timestamp format

- Must match datasource config exactly  
- Otherwise row is rejected  

---

### Too many rows

- Requests over 1000 rows will fail  

---

## Best practices

- Batch inserts in chunks under 1000 rows  
- Validate data before sending  
- Log partial success messages  
- Use consistent timestamp formats  
- Monitor ingestion errors  

---

## Example workflow

1. Create datasource  
2. Insert initial batch  
3. Stream or periodically insert updates  
4. Query or analyse using assistant  

---

## Next steps

- Query data using `/data/plot`  
- Use assistant for analysis  
- Automate ingestion pipelines  
