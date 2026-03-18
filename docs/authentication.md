# Authentication

The Madevo API uses JWT-based authentication for all secure endpoints.

---

## Overview

The authentication flow consists of:

1. Logging in to obtain a JWT
2. Using the JWT in all secure requests
3. Refreshing the token when it expires
4. Logging out to revoke the session

---

## Login

### Endpoint

```http
POST /api/v1/login
```

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
  "email": "user@example.com",
  "password": "Password1!"
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
    "is_active": true,
    "meta": {},
    "last_login": null,
    "last_logout": null,
    "created_at": "2026-03-18T12:00:00Z"
  },
  "token": "<jwt>"
}
```

### Notes

- The returned `token` is a JWT used for all secure endpoints
- Login fails if:
  - user does not exist
  - password is incorrect
  - account is inactive

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

All secure endpoints require the JWT in the `Authorization` header:

```http
Authorization: Bearer <jwt>
```

Example:

```bash
curl https://api.example.com/api/v1/secure/currentuser \
  -H 'Authorization: Bearer <jwt>'
```

---

## Token lifecycle

- Access token lifetime: **15 minutes**
- Session validity (refresh window): **7 days**
- Tokens are validated on every secure request

If a token is invalid or expired, the API returns:

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

### Authentication required

Expired JWT must be provided in both:

- Header
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
    "is_active": true,
    "meta": {},
    "created_at": "string"
  },
  "token": "<new-jwt>"
}
```

### Notes

- Both header and body must contain the token
- The token must:
  - have a valid signature
  - belong to an active session
- If the session has expired, the request fails

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

## Logout

### Endpoint

```http
POST /api/v1/secure/logout
```

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

- The current session is revoked
- The token can no longer be used after logout

### Example

```bash
curl -X POST https://api.example.com/api/v1/secure/logout \
  -H 'Authorization: Bearer <jwt>'
```

---

## WebSocket authentication

For WebSocket connections, the JWT is passed as a query parameter:

```text
wss://api.example.com/api/v1/stream/:room_id?token=<jwt>
```

### Errors

Missing token:

```json
{
  "error": "token required"
}
```

Invalid or expired token:

```json
{
  "error": "invalid or expired token"
}
```

---

## Best practices

- Always store tokens securely
- Refresh tokens before they expire during long sessions
- Do not expose tokens in client-side logs
- Re-authenticate if refresh fails
```
