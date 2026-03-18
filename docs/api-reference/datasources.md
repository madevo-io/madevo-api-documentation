# Datasources

Datasources are the core data layer in the Madevo platform. They allow you to upload, structure, store, and query data.

---

## Overview

Datasources support:

- File upload (CSV or ZIP)
- Schema definition
- Time-series and non-time-series data
- Direct row insertion
- Querying and visualization

---

## Datasource lifecycle

Typical flow:

1. Upload file  
2. Create datasource  
3. Data is processed asynchronously  
4. Query or insert additional data  

---

## Rules

### Upload

- Use `POST /api/v1/secure/datasource/upload`
- Accepts:
  - `.csv`
  - `.zip`
- Max file size: **10 GiB**
- Returns:
  - generated filename
  - detected columns
  - sample row

---

### Create

- Use `PUT /api/v1/secure/datasource`
- Requires:
  - `filename` from upload
  - original `file` name
- Triggers async import

---

### Insert

- Use `PATCH /api/v1/secure/datasource/insert`
- Max **1000 rows per request**
- Row type: `map[string]string`

#### Time-series requirements

- `timefield` must exist in each row
- `metafield` must exist in each row
- timestamp must match `timeformat`

#### Non-time-series

- timestamp is auto-generated
- `metafield` still required

---

## GET /api/v1/secure/datasource

Retrieve a datasource by ID.

### Query params

- `id` (required)

### Response

```json
{
  "error": false,
  "message": "datasource retrieved",
  "datasource": {
    "id": "string",
    "name": "string",
    "type": "string",
    "filename": "string",
    "files": ["string"],
    "d_files": [],
    "config": {},
    "doc_count": "integer",
    "updating": "boolean",
    "created_at": "string",
    "updated_at": "string"
  }
}
```

---

## GET /api/v1/secure/datasources

List datasources.

### Query params

- `page` (optional, default 0)

### Response

```json
{
  "error": false,
  "message": "datasources retrieved",
  "datasources": [],
  "total": 0
}
```

---

## POST /api/v1/secure/datasources/plain

Lightweight datasource list.

### Body

```json
{
  "search": "string",
  "page": 0
}
```

### Response

```json
{
  "error": false,
  "message": "plain datasources retrieved",
  "datasources": [
    {
      "id": "string",
      "name": "string"
    }
  ],
  "total": "integer"
}
```

---

## PUT /api/v1/secure/datasource

Create a datasource.

### Body

```json
{
  "name": "string",
  "type": "string",
  "filename": "string",
  "file": "string",
  "config": {}
}
```

### Response

```json
{
  "error": false,
  "message": "datasource created",
  "datasource_id": "string"
}
```

---

## PATCH /api/v1/secure/datasource

Update datasource.

### Body

```json
{
  "id": "string",
  "name": "string",
  "context": "string"
}
```

---

## PATCH /api/v1/secure/datasource/insert

Insert rows.

### Body

```json
{
  "datasource_id": "string",
  "data": [
    {
      "key": "value"
    }
  ]
}
```

### Response

```json
{
  "error": false,
  "message": "datasource updated",
  "inserted_rows": 1
}
```

### Partial success example

```json
{
  "error": true,
  "message": "inserted 10 rows, skipped 2 rows"
}
```

---

## DELETE /api/v1/secure/datasource

Delete datasource.

### Body

```json
{
  "id": "string"
}
```

---

## POST /api/v1/secure/datasource/upload

Upload file.

### Form data

- `file`

### Response

```json
{
  "error": false,
  "message": "datasource file uploaded",
  "filename": "string",
  "columns": [["string"]],
  "sample": ["string"],
  "file": "string"
}
```

---

## DELETE /api/v1/secure/datasource/upload

Delete uploaded file.

### Body

```json
{
  "filename": "string"
}
```

---

## POST /api/v1/secure/datasource/detect-timeformat

Detect timestamp format.

### Body

```json
{
  "timestamp": "string"
}
```

---

## GET /api/v1/secure/datasource/store-ds-schema-summary

Rebuild schema summary.

### Query params

- `id`

---

## POST /api/v1/secure/datasource/distinct-metadata

Get distinct metadata values.

### Body

```json
{
  "datasource_id": "string",
  "search": "string",
  "page": 0
}
```

---

## POST /api/v1/secure/data/plot

Query data.

### Body

```json
{
  "datasource_id": "string",
  "asset": "string",
  "from": "string",
  "to": "string",
  "value": "string"
}
```

### Response

```json
{
  "error": false,
  "message": "data view retrieved",
  "data": [],
  "total": 0
}
```

---

## Notes

- Many fields are dynamic depending on datasource config  
- Insert endpoint may partially succeed  
- Pagination defaults to 20 items  
- Some validation is minimal and handled at runtime  

---

## Best practices

- Always upload before creating a datasource  
- Validate timestamps before inserting data  
- Keep batches under 1000 rows  
- Handle partial success responses  
- Use `plain` endpoint for dropdowns or search  
