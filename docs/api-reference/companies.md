# Companies

This section covers endpoints related to the company associated with the authenticated user.

---

## Overview

Company endpoints allow you to retrieve information about the organisation linked to the current session.

---

## GET /api/v1/secure/currentcompany

Return the company associated with the current authenticated user.

### Authentication required

Bearer JWT

---

## Request

### Headers

```http
Authorization: Bearer <jwt>
```

### Query params

None

### Body

None

---

## Response

```json
{
  "error": false,
  "message": "company retrieved",
  "company": {
    "id": "string",
    "name": "string",
    "meta": {
      "string": "any"
    },
    "is_active": "boolean | null",
    "created_at": "string",
    "updated_at": "string",
    "deleted_at": "string"
  }
}
```

---

## Field definitions

- `id`  
  Unique company identifier  

- `name`  
  Company name  

- `meta`  
  Dynamic object containing additional company metadata  
  Structure depends on your environment  

- `is_active`  
  Indicates whether the company is active  
  May be `null`  

- `created_at`  
  Timestamp when the company was created  

- `updated_at`  
  Timestamp of last update  

- `deleted_at`  
  Timestamp of deletion or default zero value  

---

## Notes

- Company ID is derived from the JWT  
- No additional parameters are required  
- `meta` structure is not fixed  
- `deleted_at` may contain a default value if not deleted  

---

## Example

```bash
curl https://api.example.com/api/v1/secure/currentcompany \
  -H 'Authorization: Bearer <jwt>'
```

---

## Example response

```json
{
  "error": false,
  "message": "company retrieved",
  "company": {
    "id": "65f0c0a01234567890abc111",
    "name": "Acme IoT",
    "meta": {},
    "is_active": true,
    "created_at": "2026-03-18T12:00:00Z",
    "updated_at": "2026-03-18T12:00:00Z",
    "deleted_at": "0001-01-01T00:00:00Z"
  }
}
```

---

## Example error

```json
{
  "error": true,
  "message": "unauthorized - invalid token"
}
```

---

## Best practices

- Use this endpoint to determine the current company context  
- Do not rely on `meta` having a fixed structure  
- Always validate authentication before making requests  
