# TaskPlanory – Relationships & Multi-Tenancy Design

## 1. Multi-Tenancy Rule (Core Principle)

TaskPlanory follows **strict multi-tenancy**.

**Golden Rule:**

> Every business entity in the system belongs to exactly one organization.

This ensures:

* Strong data isolation
* Security at scale
* Simple access control

Entities scoped by `organization_id`:

* Projects
* Tasks
* TaskComments
* ActivityLogs

---

## 2. User ↔ Organization (Many-to-Many)

### Relationship

* A user can belong to multiple organizations
* An organization can have multiple users

### Implementation

Handled via the `organization_members` table.

```
User (1) <--> (M) OrganizationMember (M) <--> (1) Organization
```

### Why this design?

* Roles vary per organization
* Avoids role duplication in `users`
* Enables flexible RBAC

---

## 3. Organization → Project (One-to-Many)

### Relationship

* One organization can have many projects
* A project belongs to exactly one organization

```
Organization (1) ---> (M) Project
```

### Rule

* A project **cannot exist** without an organization

---

## 4. Project → Task (One-to-Many)

### Relationship

* One project can have many tasks
* A task belongs to exactly one project

```
Project (1) ---> (M) Task
```

### Important Design Choice

Each task stores:

* `project_id`
* `organization_id`

This redundancy:

* Prevents cross-organization access bugs
* Improves query performance

---

## 5. Task → TaskComment (One-to-Many)

### Relationship

* One task can have many comments
* A comment belongs to exactly one task

```
Task (1) ---> (M) TaskComment
```

### Purpose

* Enables collaboration and discussion
* Keeps comments scoped to tasks

---

## 6. User → Task (Assignment Relationship)

### Relationship

* A user can be assigned many tasks
* A task is assigned to one user (MVP scope)

```
User (1) ---> (M) Task
```

### Notes

* Implemented using `task.assigned_to`
* Can be extended to multiple assignees in future

---

## 7. ActivityLog Relationships

Each activity log references:

* `organization_id`
* `user_id`
* `entity_type` (task, project, etc.)
* `entity_id`

### Purpose

* Audit trail
* Debugging
* Compliance

Logs are **append-only** and never updated.

---

## 8. Access Control Rules (Critical)

* A user can access data **only** for organizations they belong to
* Every API request must resolve `organization_id`
* No database query runs without org context

### Enforcement Strategy

* Organization membership checked at middleware level
* Queries always include `organization_id` filter

---

## 9. Summary

TaskPlanory enforces tenant isolation using:

* Organization-scoped entities
* Membership-driven RBAC
* Mandatory org-level filtering

This design ensures security, scalability, and clean authorization boundaries.
