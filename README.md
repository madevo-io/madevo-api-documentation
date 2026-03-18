# Madevo API Documentation

This repository contains the official documentation for the Madevo secure API, built using MkDocs.

The documentation covers all customer-facing endpoints under `/api/v1/secure/*`, including authentication, datasources, assistant, agents, and real-time streaming.

---

## Overview

The Madevo API allows you to:

- Authenticate users and manage sessions
- Upload and manage datasources
- Insert and query time-series or structured data
- Interact with the AI assistant for analysis
- Create and manage autonomous agent tasks
- Receive real-time updates via WebSocket

---

## Documentation structure

The documentation is organized into the following sections:

- **Getting Started**
  - Authentication
  - Response format
  - Pagination

- **API Reference**
  - Auth
  - Settings
  - Context Files
  - Users and Companies
  - Datasources
  - Assistant
  - Agents
  - WebSocket

- **Guides**
  - Step-by-step workflows (e.g. upload data, run assistant, schedule agents)

- **Reference**
  - Error formats
  - Field notes and known behaviors
  - WebSocket event types

---

## Run locally

### 1. Install dependencies

```bash
pip install mkdocs-material
```

### 2. Start local server

```bash
mkdocs serve
```

Then open:

http://127.0.0.1:8000

---

## Build static site

```bash
mkdocs build
```

The generated site will be in:

site/

---

## Project structure

```text
.
├─ mkdocs.yml
├─ docs/
│  ├─ index.md
│  ├─ getting-started/
│  ├─ api-reference/
│  ├─ guides/
│  ├─ reference/
│  └─ assets/
└─ README.md
```

---

## Contributing

When updating documentation:

- Keep endpoint descriptions consistent across pages
- Use the same request/response structure for all endpoints
- Clearly mark required vs optional fields
- Avoid including internal or ambiguous behavior in customer-facing pages
- Add examples for every endpoint where possible

---

## Notes

- All secure endpoints require a Bearer JWT unless stated otherwise
- Response format follows a global envelope:

```json
{
  "error": false,
  "message": "string",
  "...data": "..."
}
```

- Some endpoints may return partial success (e.g. datasource insert)

---

## License

Internal use or company-specific license depending on your setup.
