# Auth

This section covers authentication endpoints used to access the Madevo secure API.

---

## Overview

Authentication is based on JWT tokens.

Flow:

1. Login to obtain a token  
2. Use the token for secure endpoints  
3. Refresh the token when it expires  
4. Logout to revoke the session  

---

## POST /api/v1/login

Authenticate a user and return a JWT.

### Authentication required
No

### Request

#### Headers

```http
Content-Type: application/json
```

#### Body

```json
{
  "email": "string (required, email format)",
  "password": "string (required)"
}
```

### Response

```json
{
  "error": false,
  "message": "login successful",
  "user": {
    "id": "string",
    "email": "string",
    "first_name": "string",
    "last_name": "string",
    "company_id": "string",
    "role": "string",
    "avatar": "string",
    "is_active": "boolean",
    "meta": {
      "string": "string"
    },
    "last_login": "string | null",
    "last_logout": "string | null",
    "created_at": "string"
  },
  "token": "string (JWT)"
}
```

### Notes

- Returns a JWT used for all secure endpoints  
- Login fails if credentials are invalid or user is inactive  

### Example

```bash
curl -X POST https://api.example.com/api/v1/login \
  -H 'Content-Type: application/json' \
  -d '{
    "email": "user@example.com",
    "password": "Password1!"
  }'
```

---

## POST /api/v1/refresh-token

Refresh an expired JWT.

### Authentication required

Expired token must be provided in both:
- Authorization header  
- Request body  

### Request

#### Headers

```http
Authorization: Bearer <expired-jwt>
Content-Type: application/json
```

#### Body

```json
{
  "token": "<expired-jwt>"
}
```

### Response

```json
{
  "error": false,
  "message": "token refreshed",
  "user": {
    "id": "string",
    "email": "string",
    "first_name": "string",
    "last_name": "string",
    "company_id": "string",
    "role": "string",
    "is_active": "boolean",
    "meta": {},
    "created_at": "string"
  },
  "token": "<new-jwt>"
}
```

### Notes

- Both header and body must contain the token  
- Token must be valid and session must still exist  
- Returns a new JWT  

### Example

```bash
curl -X POST https://api.example.com/api/v1/refresh-token \
  -H 'Authorization: Bearer <expired-jwt>' \
  -H 'Content-Type: application/json' \
  -d '{"token":"<expired-jwt>"}'
```

### Example error

```json
{
  "error": true,
  "message": "session expired"
}
```

---

## POST /api/v1/secure/logout

Revoke the current session.

### Authentication required

Bearer JWT

### Request

#### Headers

```http
Authorization: Bearer <jwt>
```

### Response

```json
{
  "error": false,
  "message": "logged out"
}
```

### Notes

- Invalidates the current session  
- Token cannot be used after logout  

### Example

```bash
curl -X POST https://api.example.com/api/v1/secure/logout \
  -H 'Authorization: Bearer <jwt>'
```

---

## Token behavior

- Access token lifetime: 15 minutes  
- Session lifetime: 7 days  
- All secure endpoints require:

```http
Authorization: Bearer <jwt>
```

If the token is invalid:

```json
{
  "error": true,
  "message": "unauthorized - invalid token"
}
```
