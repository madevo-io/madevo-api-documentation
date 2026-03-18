# Field Notes

This section documents important behaviors, edge cases, and inconsistencies observed in the API.

These notes are intended to help developers avoid unexpected issues when integrating with the Madevo API.

---

## Pagination inconsistencies

- Some endpoints support pagination internally but do not expose a `page` parameter
- Default page size is typically 20
- Clients may receive partial datasets without being able to request more

### Affected endpoints

- `/api/v1/secure/user/list`
- `/api/v1/secure/assistant/conversations`
- `/api/v1/secure/agent/tasks`

---

## Partial success responses

Some endpoints return `error = true` even when part of the operation succeeds.

### Example

```json
{
  "error": true,
  "message": "inserted 10 rows, skipped 2 rows"
}
```

### Important

- Data may still be written successfully  
- Always inspect the message field  

---

## Dynamic schemas

Some fields are not strictly defined and vary depending on context.

### Examples

- `meta` objects
- datasource `config`
- query results in `/data/plot`
- agent `steps`

### Recommendation

- Treat these fields as flexible objects  
- Do not enforce strict schema validation  

---

## Assistant behavior quirks

- `skip_history` field exists but is not used  
- `suggested_questions` is returned as a string, not parsed JSON  
- Conversation status changes during processing (`ready`, `loading`)  

---

## Response inconsistencies

- Some endpoints return unexpected or misleading messages  
  - Example: conversation creation returns `"conversation history retrieved"`  
- Some optional fields may be omitted entirely instead of being null  

---

## Token handling

- Token must be passed in both header and body for refresh  
- Expired tokens still required for refresh flow  
- Invalid tokens return generic error messages  

---

## WebSocket behavior

- `room_id` is not used for access control  
- Subscriptions are derived from JWT  
- Client messages are ignored  
- Events may arrive out of order  

---

## Datasource quirks

- Upload returns inferred schema but does not validate correctness  
- Create step performs minimal validation  
- Insert endpoint:
  - skips invalid rows silently (reported only in message)
  - enforces max 1000 rows per request  

---

## Validation gaps

- Some endpoints accept invalid values without strict validation  
- Incorrect types may only fail at runtime  
- Missing fields may not always produce clear errors  

---

## Time handling

- Time parsing depends strictly on `timeformat`  
- Incorrect format causes silent row rejection  
- Timezone handling may not be explicit  

---

## Best practices

- Always validate inputs before sending requests  
- Do not rely on strict schema enforcement  
- Handle partial success responses carefully  
- Expect inconsistent pagination support  
- Log responses for debugging  
- Build defensive client-side logic  

---

## Summary

The API is flexible and powerful but requires careful handling due to:

- dynamic schemas  
- partial success responses  
- inconsistent pagination  
- minimal validation in some areas  

Understanding these behaviors will help you build more robust integrations.
