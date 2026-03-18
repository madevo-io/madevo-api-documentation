# Authentication

The Madevo API uses JWT-based authentication for all secure endpoints.

---

## Overview

Authentication works as follows:

1. Login to obtain a JWT token  
2. Include the token in all secure requests  
3. Refresh the token when it expires  
4. Logout to revoke the session  

---

## Login

### Endpoint

```http
POST /api/v1/login
```

---

### Request

#### Headers

```http
Content-Type: application/json
```

#### Body

```json
{
  "email": "user@example.com",
  "password": "Password1!"
}
```

---

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
    "is_active": true,
    "meta": {},
    "created_at": "string"
  },
  "token": "<jwt>"
}
```

---

### Notes

- The `token` is required for all secure endpoints  
- Login fails if credentials are invalid or account is inactive  

---

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

## Using the token

All secure endpoints require:

```http
Authorization: Bearer <jwt>
```

---

### Example

```bash
curl https://api.example.com/api/v1/secure/currentuser \
  -H 'Authorization: Bearer <jwt>'
```

---

## Token lifecycle

- Access token lifetime: 15 minutes  
- Session lifetime: 7 days  

If the token is invalid:

```json
{
  "error": true,
  "message": "unauthorized - invalid token"
}
```

---

## Refresh token

### Endpoint

```http
POST /api/v1/refresh-token
```

---

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

---

### Response

```json
{
  "error": false,
  "message": "token refreshed",
  "user": {
    "id": "string",
    "email": "string"
  },
  "token": "<new-jwt>"
}
```

---

### Notes

- Token must be provided in both header and body  
- Session must still be valid  

---

## Logout

### Endpoint

```http
POST /api/v1/secure/logout
```

---

### Request

```http
Authorization: Bearer <jwt>
```

---

### Response

```json
{
  "error": false,
  "message": "logged out"
}
```

---

## WebSocket authentication

Use the token as a query parameter:

```text
wss://api.example.com/api/v1/stream/:room_id?token=<jwt>
```

---

## Best practices

- Store tokens securely  
- Refresh tokens before expiry  
- Do not expose tokens in logs  
- Handle authentication errors gracefully  
