# Agents

Agents are automated tasks that run against your datasources to generate insights, perform analysis, and execute workflows.

---

## Overview

Agents allow you to:

- Define reusable analysis tasks
- Run tasks on demand
- Schedule tasks to run automatically
- Track execution history and results

---

## Key concepts

### Task

An agent task includes:

- `name`
- `datasources`
- `prompt`
- execution settings (thinking, mode)
- automation configuration

---

### Execution

Tasks can be:

- run manually
- triggered automatically on a schedule

---

### Status

Common task states:

- `ready`
- `running`
- `error`

---

## GET /api/v1/secure/agent/tasks

List all agent tasks.

### Authentication required

Bearer JWT

---

### Response

```json
{
  "error": false,
  "message": "agent tasks retrieved",
  "tasks": [],
  "total": 0
}
```

---

### Notes

- Pagination exists internally but is not exposed  
- Default page size is 20  

---

## GET /api/v1/secure/agent/task/:task_id

Retrieve a single agent task.

### Path params

- `task_id` (required)

---

### Response

```json
{
  "error": false,
  "message": "agent task retrieved",
  "task": {
    "id": "string",
    "name": "string",
    "prompt": "string",
    "datasources": ["string"],
    "status": "string",
    "steps": [],
    "answer": "string",
    "created_at": "string",
    "updated_at": "string",
    "last_run_at": "string | null",
    "automation_enabled": "boolean",
    "automation_interval": "integer"
  }
}
```

---

## POST /api/v1/secure/agent/task

Create a new agent task.

### Body

```json
{
  "name": "string",
  "datasources": ["string"],
  "prompt": "string",
  "thinking": false
}
```

---

### Response

```json
{
  "error": false,
  "message": "agent task created",
  "task": {
    "id": "string",
    "name": "string",
    "status": "ready"
  }
}
```

---

### Notes

- `name`, `datasources`, and `prompt` are required  
- Default values:
  - `mode`: `solve`
  - `status`: `ready`
  - `automation_enabled`: false  
  - `automation_interval`: 15  

---

## GET /api/v1/secure/agent/task/run/:task_id

Run an agent task manually.

### Path params

- `task_id`

---

### Response

```json
{
  "error": false,
  "message": "agent task run",
  "task": {
    "id": "string",
    "status": "running"
  }
}
```

---

### Notes

- Task transitions to `running` state  
- Execution happens asynchronously  

---

## POST /api/v1/secure/agent/task/schedule

Enable or update task automation.

### Body

```json
{
  "task_id": "string",
  "automation_enabled": true,
  "automation_interval": 60
}
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

### Validation rules

- `task_id` and `automation_interval` are required  

#### Interval rules

If `thinking = true`, allowed values:

- 15
- 30
- 60
- 120
- 1440

If `thinking = false`:

- range: 1 to 1440 minutes  

---

### Notes

- Interval is defined in minutes  
- Scheduling runs in background  

---

## DELETE /api/v1/secure/agent/task/delete/:task_id

Delete an agent task.

---

### Response

```json
{
  "error": false,
  "message": "agent task deleted"
}
```

---

### Validation rules

Task can be deleted only if:

- status is `ready` or `error`  
- OR `running` but last run was more than 10 minutes ago  

---

## GET /api/v1/secure/agent/taskhistory/:task_id

Retrieve execution history for a task.

---

### Response

```json
{
  "error": false,
  "message": "agent task history retrieved",
  "history": [],
  "total": 0
}
```

---

### Notes

- History includes steps and outputs  
- Pagination exists internally but not exposed  
- `total` may not strictly match filtered task results  

---

## Lifecycle

1. Create task  
2. Run manually or schedule  
3. Task executes  
4. Results stored in history  

---

## Best practices

- Keep prompts clear and focused  
- Use specific datasources  
- Validate scheduling intervals  
- Monitor task status and history  
- Avoid running overlapping long tasks  
