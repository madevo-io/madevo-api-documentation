# Settings

This section covers endpoints related to company-level settings.

---

## GET /api/v1/secure/settings

Return the current company's system settings.

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
  "message": "settings retrieved",
  "settings": {
    "_id": "string",
    "company_id": "string",
    "name": "string",
    "config": {
      "string": "any"
    },
    "created_at": "string",
    "updated_at": "string"
  }
}
```

---

## Field definitions

- `_id`  
  Unique identifier of the settings record  

- `company_id`  
  ID of the company the settings belong to  

- `name`  
  Name of the settings profile  

- `config`  
  Dynamic object containing configuration values  
  Structure depends on your setup  

- `created_at`  
  Timestamp when the settings were created  

- `updated_at`  
  Timestamp when the settings were last updated  

---

## Notes

- The company is derived from the JWT  
- Only one settings record is typically returned  
- The `config` object is flexible and may vary between environments  

---

## Example

```bash
curl https://api.example.com/api/v1/secure/settings \
  -H 'Authorization: Bearer <jwt>'
```

---

## Example error

```json
{
  "error": true,
  "message": "unauthorized - invalid token"
}
```
