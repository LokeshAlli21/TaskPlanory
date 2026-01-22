# Database Tables – Phase 1 (tables.md)

This document defines the **initial database schema** for Phase 1. The goal is to support **authentication, core task management, and basic collaboration**, while keeping the schema clean and extensible for later phases.

---

## 1. users

Stores all registered users.

| Column Name   | Type      | Description               |
| ------------- | --------- | ------------------------- |
| id            | UUID (PK) | Unique user identifier    |
| full_name     | VARCHAR   | User full name            |
| email         | VARCHAR   | Unique email address      |
| password_hash | TEXT      | Encrypted password        |
| role          | VARCHAR   | user / admin              |
| is_verified   | BOOLEAN   | Email verification status |
| created_at    | TIMESTAMP | Account creation time     |
| updated_at    | TIMESTAMP | Last update time          |

---

## 2. workspaces

Represents a project space (similar to a team or organization).

| Column Name | Type                 | Description       |
| ----------- | -------------------- | ----------------- |
| id          | UUID (PK)            | Workspace ID      |
| name        | VARCHAR              | Workspace name    |
| owner_id    | UUID (FK → users.id) | Workspace creator |
| created_at  | TIMESTAMP            | Creation time     |

---

## 3. workspace_members

Links users to workspaces.

| Column Name  | Type                      | Description    |
| ------------ | ------------------------- | -------------- |
| id           | UUID (PK)                 | Membership ID  |
| workspace_id | UUID (FK → workspaces.id) | Workspace      |
| user_id      | UUID (FK → users.id)      | Member         |
| role         | VARCHAR                   | owner / member |
| joined_at    | TIMESTAMP                 | Join date      |

---

## 4. projects

Projects inside a workspace.

| Column Name  | Type                      | Description      |
| ------------ | ------------------------- | ---------------- |
| id           | UUID (PK)                 | Project ID       |
| workspace_id | UUID (FK → workspaces.id) | Parent workspace |
| name         | VARCHAR                   | Project name     |
| description  | TEXT                      | Project details  |
| created_at   | TIMESTAMP                 | Created time     |

---

## 5. tasks

Core task entity.

| Column Name | Type                    | Description               |
| ----------- | ----------------------- | ------------------------- |
| id          | UUID (PK)               | Task ID                   |
| project_id  | UUID (FK → projects.id) | Parent project            |
| title       | VARCHAR                 | Task title                |
| description | TEXT                    | Task details              |
| status      | VARCHAR                 | todo / in-progress / done |
| priority    | VARCHAR                 | low / medium / high       |
| due_date    | DATE                    | Deadline                  |
| created_by  | UUID (FK → users.id)    | Task creator              |
| created_at  | TIMESTAMP               | Created time              |

---

## 6. task_assignees

Allows multiple users per task.

| Column Name | Type                 | Description   |
| ----------- | -------------------- | ------------- |
| id          | UUID (PK)            | Assignment ID |
| task_id     | UUID (FK → tasks.id) | Task          |
| user_id     | UUID (FK → users.id) | Assigned user |

---

## 7. comments

Task discussion and updates.

| Column Name | Type                 | Description    |
| ----------- | -------------------- | -------------- |
| id          | UUID (PK)            | Comment ID     |
| task_id     | UUID (FK → tasks.id) | Task           |
| user_id     | UUID (FK → users.id) | Comment author |
| content     | TEXT                 | Comment text   |
| created_at  | TIMESTAMP            | Time posted    |

---

## 8. activity_logs

Tracks important actions (for future AI insights).

| Column Name  | Type                 | Description                 |
| ------------ | -------------------- | --------------------------- |
| id           | UUID (PK)            | Log ID                      |
| entity_type  | VARCHAR              | task / project / workspace  |
| entity_id    | UUID                 | Related entity              |
| action       | VARCHAR              | created / updated / deleted |
| performed_by | UUID (FK → users.id) | Actor                       |
| created_at   | TIMESTAMP            | Action time                 |

---

## Phase 1 Coverage

✔ Authentication & users
✔ Projects & tasks
✔ Team collaboration
✔ Activity tracking (basic)

This schema is **intentionally minimal but scalable** for Phase 2 (AI suggestions, analytics, smart timelines).
