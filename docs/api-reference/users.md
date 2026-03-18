# Users

This section covers endpoints related to users within a company.

---

## Overview

User endpoints allow you to:

- Retrieve the current authenticated user
- Fetch a specific user by ID
- List all users in the company

Most user operations require elevated permissions.

---

## GET /api/v1/secure/currentuser

Return the current authenticated user from JWT claims.

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
  "message": "current user",
  "user": {
    "id": "string",
    "email": "string",
    "first_name": "string",
    "last_name": "string",
    "company_id": "string",
    "role": "string"
  }
}
```

---

### Notes

- Data is extracted directly from the JWT  
- No database lookup is performed  
- Response is a simplified user object  

---

### Example

```bash
curl https://api.example.com/api/v1/secure/currentuser \
  -H 'Authorization: Bearer <jwt>'
```

---

## GET /api/v1/secure/user/:id

Retrieve a specific user by ID.

### Authentication required

Bearer JWT

---

### Request

#### Headers

```http
Authorization: Bearer <jwt>
```

#### Path params

- `id` (required)  
  User ID  

---

### Response

```json
{
  "error": false,
  "message": "user retrieved",
  "user": {
    "id": "string",
    "email": "string",
    "first_name": "string",
    "last_name": "string",
    "company_id": "string",
    "role": "string",
    "avatar": "string",
    "is_active": "boolean",
    "meta": {},
    "last_login": "string | null",
    "last_logout": "string | null",
    "created_at": "string"
  }
}
```

---

### Validation rules

- `id` is required  
- Must be a valid identifier  
- Caller must have manager-level or higher permissions  

---

### Example

```bash
curl https://api.example.com/api/v1/secure/user/<id> \
  -H 'Authorization: Bearer <jwt>'
```

---

### Example error

```json
{
  "error": true,
  "message": "forbidden"
}
```

---

## GET /api/v1/secure/user/list

List all users in the current company.

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
  "message": "users retrieved",
  "users": [
    {
      "id": "string",
      "email": "string",
      "first_name": "string",
      "last_name": "string",
      "company_id": "string",
      "role": "string",
      "avatar": "string",
      "is_active": "boolean",
      "meta": {},
      "last_login": "string | null",
      "last_logout": "string | null",
      "created_at": "string"
    }
  ],
  "total": "integer"
}
```

---

### Notes

- Returns users belonging to the same company  
- Requires manager-level or higher permissions  
- Pagination is applied internally but not exposed via request parameters  

---

### Example

```bash
curl https://api.example.com/api/v1/secure/user/list \
  -H 'Authorization: Bearer <jwt>'
```

---

### Example response

```json
{
  "error": false,
  "message": "users retrieved",
  "users": [],
  "total": 0
}
```

---

## Roles

The exact set of roles may vary, but commonly include:

- `system`
- `manager`
- `user`

Permissions for endpoints may depend on role level.

---

## Best practices

- Use `currentuser` for session-aware features  
- Use `user/:id` only when necessary  
- Handle authorization errors explicitly  
- Do not assume full user lists without pagination awareness  
