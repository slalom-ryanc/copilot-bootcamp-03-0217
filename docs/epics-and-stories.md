```markdown
-- Epic: Task Creation & Editing
  - Story: Add task creation form
    - Acceptance Criteria: Users can add a new task with a title; title is required; description is optional.
  - Story: Add optional task description field
    - Acceptance Criteria: Tasks support an optional description field saved with the task.
  - Story: Edit task title and description
    - Acceptance Criteria: Users can update a task's title and description and changes persist.
  - Story: Delete a task
    - Acceptance Criteria: Users can delete a task and it is removed from the list and persistence.
  - Technical Requirements:
    - Frontend: `packages/frontend/src/TaskForm.js` and `packages/frontend/src/App.js` must call the backend endpoints (`POST /api/tasks` for create, `PUT /api/tasks/:id` for edit) and validate the required `title` field.
    - Backend: `packages/backend/src/app.js` must accept `POST /api/tasks`, `PUT /api/tasks/:id`, and `DELETE /api/tasks/:id` as currently implemented to create, update, and delete tasks in the SQLite `tasks` table.

-- Epic: Task Listing & Sorting
  - Story: Display task list sorted by creation date
    - Acceptance Criteria: Task list is displayed and sorted by creation date as specified in the PRD.
  - Technical Requirements:
    - Frontend: `packages/frontend/src/TaskList.js` must fetch from `GET /api/tasks` and render tasks in the returned order.
    - Backend: `packages/backend/src/app.js` `GET /api/tasks` must return tasks ordered (current SQL orders by due date then created_at; front end expects sorting by creation date per PRD).

-- Epic: Task State Management
  - Story: Toggle task completion state
    - Acceptance Criteria: Users can toggle a task between completed and uncompleted states.
  - Story: Reflect completion state in task list
    - Acceptance Criteria: The task list visually indicates a task's completion state.
  - Technical Requirements:
    - Frontend: `packages/frontend/src/TaskList.js` must call `PATCH /api/tasks/:id` with `{ completed: boolean }` on checkbox toggle and update UI styling to reflect `completed`.
    - Backend: `packages/backend/src/app.js` must support `PATCH /api/tasks/:id` updating the `completed` column (already implemented).

-- Epic: Persistence
  - Story: Persist tasks across page refresh
    - Acceptance Criteria: Tasks persist across page refresh (using localStorage or a simple backend as noted in the PRD).
  - Technical Requirements:
    - Frontend: Current implementation uses the backend API; `packages/frontend/src/TaskList.js` should continue to fetch from `/api/tasks` on load.
    - Backend: `packages/backend/src/app.js` currently persists tasks in an in-memory SQLite DB initialized in `app.js`; to meet persistence across server restarts, consider replacing `:memory:` with a file DB (this note references current implementation only).

-- Epic: UI/UX & Validation
  - Story: Implement responsive task form and list
    - Acceptance Criteria: The task form and list are responsive across viewports as required in the PRD.
  - Story: Add validation for required fields
    - Acceptance Criteria: Simple validation is present for required fields (e.g., task title).
  - Story: Ensure accessibility for form and list
    - Acceptance Criteria: Form and list meet basic accessibility requirements called out in the PRD.
  - Technical Requirements:
    - Frontend: Use `packages/frontend/src/TaskForm.js` for field validation (title required) and MUI components for responsive layout; ensure ARIA attributes and `inputProps` are present where applicable.

-- Epic: Basic Filtering
  - Story: Add All / Active / Completed filters
    - Acceptance Criteria: All / Active / Completed filters are available and correctly filter the task list.
  - Technical Requirements:
    - Frontend: Implement filter UI that calls `GET /api/tasks?completed=true|false` or filters client-side using the `completed` flag returned by `GET /api/tasks` in `packages/frontend/src/TaskList.js`.
    - Backend: `packages/backend/src/app.js` already supports `completed` query filtering in `GET /api/tasks` via `buildTaskQuery`.

-- Epic: Priorities & Due Dates
  - Story: Add priority levels to tasks
    - Acceptance Criteria: Tasks support priority levels as described in the PRD.
  - Story: Add due date field to tasks
    - Acceptance Criteria: Tasks support a due date field as described in the PRD.
  - Story: Visual deadline warnings for near/overdue tasks
    - Acceptance Criteria: Visual warnings are shown for near or overdue tasks per the PRD.
  - Technical Requirements:
    - Frontend: `packages/frontend/src/TaskForm.js` already provides a `due_date` input; UI components (e.g., `packages/frontend/src/TaskList.js`) must render `due_date` and apply visual warning styles for near/overdue tasks.
    - Backend: `packages/backend/src/app.js` schema includes `due_date` column and `GET /api/tasks` returns `due_date` for rendering.

-- Epic: Search & Advanced Filters
  - Story: Implement full-text search
    - Acceptance Criteria: Full-text search is available as stated in the PRD.
  - Story: Add filters by priority, due date, and tags
    - Acceptance Criteria: Users can filter tasks by priority, due date, and tags as described in the PRD.
  - Technical Requirements:
    - Frontend: Add search UI to call `GET /api/tasks?search=<kw>` or filter client-side using returned task fields.
    - Backend: `packages/backend/src/app.js` `buildTaskQuery` supports `search` via `LIKE` on `title` and `description` (already present).

-- Epic: User Accounts & Sync
  - Story: Add user authentication
    - Acceptance Criteria: User accounts and authentication are supported as called out in the PRD.
  - Story: Store tasks server-side for multi-device sync
    - Acceptance Criteria: Tasks can be stored server-side and sync across devices as described in the PRD.
  - Technical Requirements:
    - Backend: Current backend (`packages/backend/src/app.js`) is single-tenant with no auth; adding auth would require new endpoints/middleware and a persistent database backing (beyond `:memory:`).
    - Frontend: `packages/frontend` would need to integrate auth flows and call authenticated API endpoints; reference `packages/frontend/src/App.js` and `TaskList.js` for where API calls occur.

-- Epic: Recurring Tasks & Reminders
  - Story: Support recurring task schedules
    - Acceptance Criteria: Recurring task scheduling is supported per the PRD.
  - Story: Add push/email reminders
    - Acceptance Criteria: Push and/or email reminders are supported as described in the PRD.
  - Technical Requirements:
    - Backend: Extend `packages/backend/src/app.js` data model and add scheduled job processing or integration with a job queue/service for reminders.
    - Frontend: Provide scheduling inputs in `packages/frontend/src/TaskForm.js` and display recurrence in `TaskList.js`.

-- Epic: Subtasks & Dependencies
  - Story: Support nested subtasks
    - Acceptance Criteria: Nested subtasks are supported as described in the PRD.
  - Story: Add task dependency relationships
    - Acceptance Criteria: Task dependencies are supported as described in the PRD.
  - Story: Calculate progress rollups for parent tasks
    - Acceptance Criteria: Progress rollups for parent tasks are available as described in the PRD.
  - Technical Requirements:
    - Backend: Update `packages/backend/src/app.js` schema (e.g., `parent_id` or a separate `subtasks` table) and endpoints to manage subtasks and dependencies.
    - Frontend: Update `packages/frontend/src/TaskList.js` and `TaskForm.js` to allow creating and rendering nested subtasks and show progress rollups.

-- Epic: Collaboration & Sharing
  - Story: Share lists with other users
    - Acceptance Criteria: Users can share lists with other users as called out in the PRD.
  - Story: Assign tasks to users
    - Acceptance Criteria: Tasks can be assigned to users as described in the PRD.
  - Story: Track activity and history audit
    - Acceptance Criteria: Activity/history audit is available as described in the PRD.
  - Technical Requirements:
    - Backend: Add users and sharing models to `packages/backend/src/app.js` and authenticated endpoints for list sharing and assignment.
    - Frontend: Add UI flows in `packages/frontend` to manage sharing and assignment; integrate with auth once implemented.

-- Epic: Integrations & Export
  - Story: Add calendar sync (iCal)
    - Acceptance Criteria: Calendar sync (iCal) capability is available as described in the PRD.
  - Story: Add CSV export
    - Acceptance Criteria: CSV export is available as described in the PRD.
  - Story: Implement third-party integrations
    - Acceptance Criteria: Third-party integrations are supported as described in the PRD.
  - Technical Requirements:
    - Backend: Provide export endpoints (e.g., `/api/tasks/export.csv`, `/api/tasks/ical`) implemented in `packages/backend/src/app.js` to generate CSV/iCal from stored tasks.
    - Frontend: Add UI actions in `packages/frontend` to trigger exports and integrate with third-party flows.

-- Epic: Bulk Actions & Undo
  - Story: Add multi-select for bulk edits
    - Acceptance Criteria: Multi-select and bulk edit actions are available as described in the PRD.
  - Story: Implement undo for destructive actions
    - Acceptance Criteria: Undo is available for destructive actions as described in the PRD.
  - Technical Requirements:
    - Frontend: Implement multi-select UI in `packages/frontend/src/TaskList.js` and send batch requests to API endpoints.
    - Backend: Add batch endpoints in `packages/backend/src/app.js` (e.g., `PATCH /api/tasks` with list of IDs) or support transactional updates.

-- Epic: Analytics & Reporting
  - Story: Add task completion metrics
    - Acceptance Criteria: Task completion metrics are available as described in the PRD.
  - Story: Generate trend reports
    - Acceptance Criteria: Trend reports are available as described in the PRD.
  - Technical Requirements:
    - Backend: Provide reporting endpoints in `packages/backend/src/app.js` that aggregate task metrics from the DB.
    - Frontend: Add reporting UI pages/components that call the reporting endpoints and render charts or tables.
``` 
