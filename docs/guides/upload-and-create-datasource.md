# Upload and Create a Datasource

This guide walks you through the full process of uploading a file and creating a datasource in Madevo.

---

## Overview

You will:

1. Upload a file  
2. Inspect detected schema  
3. Create a datasource  
4. Wait for processing  
5. Verify the datasource  

---

## Step 1: Upload a file

Upload a CSV or ZIP file.

### Endpoint

```http
POST /api/v1/secure/datasource/upload
```

### Example

```bash
curl -X POST https://api.example.com/api/v1/secure/datasource/upload \
  -H 'Authorization: Bearer <jwt>' \
  -F 'file=@data.csv'
```

---

### Response

```json
{
  "error": false,
  "message": "datasource file uploaded",
  "filename": "generated-storage-key.csv",
  "columns": [["timestamp", "asset", "temperature"]],
  "sample": ["2026-03-18 12:00:00", "motor-1", "73.4"],
  "file": "data.csv"
}
```

---

### Important fields

- `filename` → required for the next step  
- `columns` → detected schema  
- `sample` → example row  

---

## Step 2: Define datasource config

Based on the uploaded file, define how the data should be interpreted.

### Example config

```json
{
  "assets": [],
  "columns": [["timestamp", "asset", "temperature"]],
  "timefield": "timestamp",
  "metafield": "asset",
  "values": ["temperature"],
  "timeformat": "2006-01-02 15:04:05",
  "granularity": "",
  "is_timeseries": true,
  "lifespan": 0,
  "context": "Factory telemetry"
}
```

---

## Step 3: Create the datasource

### Endpoint

```http
PUT /api/v1/secure/datasource
```

---

### Example

```bash
curl -X PUT https://api.example.com/api/v1/secure/datasource \
  -H 'Authorization: Bearer <jwt>' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Factory telemetry",
    "type": "csv",
    "filename": "generated-storage-key.csv",
    "file": "data.csv",
    "config": {
      "assets": [],
      "columns": [["timestamp", "asset", "temperature"]],
      "timefield": "timestamp",
      "metafield": "asset",
      "values": ["temperature"],
      "timeformat": "2006-01-02 15:04:05",
      "granularity": "",
      "is_timeseries": true,
      "lifespan": 0,
      "context": "Factory telemetry"
    }
  }'
```

---

### Response

```json
{
  "error": false,
  "message": "datasource created",
  "datasource_id": "65f0c0a01234567890abc222"
}
```

---

## Step 4: Wait for processing

After creation:

- Data import runs asynchronously  
- Datasource status may be updating  
- Data may not be immediately available  

---

## Step 5: Verify datasource

Retrieve the datasource:

```bash
curl 'https://api.example.com/api/v1/secure/datasource?id=<datasource_id>' \
  -H 'Authorization: Bearer <jwt>'
```

---

## Common mistakes

### Missing filename

- Must use the `filename` returned from upload  
- Not the original file name  

---

### Incorrect timeformat

- Must match the timestamp format exactly  
- Example:
  - `2006-01-02 15:04:05`

---

### Missing required fields

- `timefield` required for time-series  
- `metafield` required always  

---

## Best practices

- Always inspect `columns` and `sample` before creating  
- Start with small datasets for testing  
- Ensure timestamps are consistent  
- Use clear `context` for better assistant results  
- Keep column names simple and consistent  

---

## Next steps

- Insert additional data using `/datasource/insert`  
- Query data using `/data/plot`  
- Use the assistant to analyse your datasource  
