# Cloud Architecture Overview

A simple system-context diagram for this monorepo: React frontend + Express API + in-memory store.

```mermaid
flowchart LR
  Browser["User Browser"]
  React["React Frontend\n(packages/frontend)"]
  API["Express API\n(packages/backend)"]
  Store["In-Memory Store\n(volatile)"]

  Browser --> React
  React -->|HTTP / REST| API
  API -->|read / write| Store
```

Notes:
- Frontend: single-page React app that runs in the user's browser.
- API: Express server serving JSON endpoints used by the frontend.
- Store: simple in-memory data store kept by the API (no external DB in this setup).

## Sequence: Creating a TODO

```mermaid
sequenceDiagram
  participant U as User (Browser)
  participant F as React Frontend
  participant S as Express API
  participant M as In-Memory Store

  U->>F: Fill form + click "Create" (form data)
  F->>S: POST /tasks { title, ... }
  S->>M: createTask(data)
  M-->>S: taskCreated { id, title, ... }
  S-->>F: 201 Created, task JSON
  F-->>U: show new task in UI
```

This sequence shows the user submitting a task from the React frontend, the frontend calling the Express API, the API persisting in the in-memory store, and the responses flowing back to update the UI.
