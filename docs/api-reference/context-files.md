# Context Files

Context files are documents uploaded to enrich assistant responses through embeddings and contextual knowledge.

---

## Overview

Context files allow you to:

- Upload documents (PDF, text, DOCX)
- Process them into embeddings
- Use them as context for assistant queries

Files are processed asynchronously after upload.

---

## GET /api/v1/secure/contextfiles

List all context files for the authenticated company.

### Authentication required

Bearer JWT

---

### Request

#### Headers

```http
Authorization: Bearer <jwt>
```

---

### Response

```json
{
  "error": false,
  "message": "context files retrieved",
  "files": [
    {
      "id": "string",
      "filename": "string",
      "uploaded_date": "string",
      "metadata": {
        "company_id": "string",
        "status": "string",
        "collection_id": "string",
        "created_at": "string"
      }
    }
  ]
}
```

---

### Field definitions

- `files`  
  Array of context file records  

- `metadata.status`  
  Processing status of the file  
  Possible values:
  - `pending`
  - `ready`
  - `failed`

- `metadata.collection_id`  
  Identifier for the embedding collection  
  May be empty depending on processing stage  

---

### Example

```bash
curl https://api.example.com/api/v1/secure/contextfiles \
  -H 'Authorization: Bearer <jwt>'
```

---

## PUT /api/v1/secure/contextfiles

Upload one or more context files.

### Authentication required

Bearer JWT

---

### Request

#### Headers

```http
Authorization: Bearer <jwt>
Content-Type: multipart/form-data
```

#### Form data

- `files` (required)  
  One or more files

---

### Supported file types

- `text/plain`
- `application/pdf`
- `application/vnd.openxmlformats-officedocument.wordprocessingml.document`

---

### Response

```json
{
  "error": false,
  "message": "context files saved"
}
```

---

### Notes

- Files are stored immediately and processed asynchronously  
- Initial status is `pending`  
- Unsupported file types will return an error  

---

### Example

```bash
curl -X PUT https://api.example.com/api/v1/secure/contextfiles \
  -H 'Authorization: Bearer <jwt>' \
  -F 'files=@document.pdf' \
  -F 'files=@notes.txt'
```

---

### Example error

```json
{
  "error": true,
  "message": "invalid input: unsupported file type image/png"
}
```

---

## DELETE /api/v1/secure/contextfiles/:fileId

Delete a context file and its associated embeddings.

### Authentication required

Bearer JWT

---

### Request

#### Headers

```http
Authorization: Bearer <jwt>
```

#### Path params

- `fileId` (required)  
  ID of the file to delete  

---

### Response

```json
{
  "error": false,
  "message": "context file deleted"
}
```

---

### Validation rules

- `fileId` must be valid  
- If the file is still processing (`pending`), deletion may fail  
- Files recently uploaded may be locked during processing  

---

### Example

```bash
curl -X DELETE https://api.example.com/api/v1/secure/contextfiles/<fileId> \
  -H 'Authorization: Bearer <jwt>'
```

---

### Example error

```json
{
  "error": true,
  "message": "file is still processing"
}
```

---

## Lifecycle

1. Upload file  
2. File enters `pending` state  
3. Background processing generates embeddings  
4. Status becomes `ready` or `failed`  

---

## Best practices

- Wait until file status is `ready` before relying on it in assistant queries  
- Upload only supported file types  
- Handle asynchronous processing in your UI or integration  
- Avoid deleting files immediately after upload  
