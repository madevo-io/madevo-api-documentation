# Schedule an Agent Task

This guide shows how to create an agent and schedule it to run automatically.

---

## Overview

You will:

1. Create an agent task  
2. Enable scheduling  
3. Configure execution interval  
4. Monitor task execution  

---

## Step 1: Create an agent task

Before scheduling, you must create a task.

### Endpoint

```http
POST /api/v1/secure/agent/task
```

---

### Example

```bash
curl -X POST https://api.example.com/api/v1/secure/agent/task \
  -H 'Authorization: Bearer <jwt>' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Daily anomaly scan",
    "datasources": ["<datasource_id>"],
    "prompt": "Summarise anomalies in the last 24 hours",
    "thinking": false
  }'
```

---

### Response

```json
{
  "error": false,
  "message": "agent task created",
  "task": {
    "id": "task_id",
    "status": "ready"
  }
}
```

---

## Step 2: Enable scheduling

### Endpoint

```http
POST /api/v1/secure/agent/task/schedule
```

---

### Example

```bash
curl -X POST https://api.example.com/api/v1/secure/agent/task/schedule \
  -H 'Authorization: Bearer <jwt>' \
  -H 'Content-Type: application/json' \
  -d '{
    "task_id": "<task_id>",
    "automation_enabled": true,
    "automation_interval": 60
  }'
```

---

### Response

```json
{
  "error": false,
  "message": "agent task scheduled"
}
```

---

## Step 3: Choose interval

### Interval unit

- Minutes

---

### Rules

#### When `thinking = true`

Allowed values:

- 15
- 30
- 60
- 120
- 1440

---

#### When `thinking = false`

Allowed range:

- 1 to 1440 minutes

---

## Step 4: Monitor execution

### Option 1: Fetch task

```bash
curl https://api.example.com/api/v1/secure/agent/task/<task_id> \
  -H 'Authorization: Bearer <jwt>'
```

---

### Option 2: Fetch history

```bash
curl https://api.example.com/api/v1/secure/agent/taskhistory/<task_id> \
  -H 'Authorization: Bearer <jwt>'
```

---

### Option 3: Use WebSocket

Listen for:

```json
{
  "event": "agent-task",
  "data": {
    "id": "task_id",
    "action": "updated"
  }
}
```

---

## Execution behavior

- Tasks run in the background  
- Execution updates are asynchronous  
- Results are stored in task history  

---

## Common mistakes

### Invalid interval

- Ensure interval matches rules  
- Especially for `thinking = true`  

---

### Scheduling before creation

- Task must exist before scheduling  

---

### Ignoring execution state

- Task may still be running  
- Avoid overlapping runs  

---

## Best practices

- Use clear and focused prompts  
- Choose appropriate intervals  
- Avoid very frequent schedules unless needed  
- Monitor history for validation  
- Combine with alerts or dashboards  

---

## Example workflow

1. Create datasource  
2. Create agent  
3. Schedule task  
4. Monitor results  
5. Adjust interval or prompt  

---

## Next steps

- Combine agents with assistant insights  
- Build automated monitoring workflows  
- Integrate with alerting systems  
