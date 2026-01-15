# TaskPlanory – Core Entities (Phase 1 · Day 1)

This document defines the core entities of TaskPlanory and clearly explains
their responsibilities and purpose in the system.

The goal of this design is to support multi-tenancy, role-based access control,
auditability, and AI-assisted workflows in a scalable and maintainable way.

---

## 1. User

**Responsibility:**  
Stores authentication, identity, and profile information for a person using the platform.

**Why it exists:**  
Without this entity, authentication, authorization, and user-specific task ownership
would not be possible.

---

## 2. Organization

**Responsibility:**  
Defines the tenant boundary and ownership context for all projects, tasks, and members.

**Why it exists:**  
TaskPlanory is a multi-tenant system. Organizations ensure data isolation between
different teams and enable users to belong to multiple workspaces.

---

## 3. OrganizationMember

**Responsibility:**  
Maps users to organizations and enforces role-based access control (RBAC).

**Why it exists:**  
This entity enables many-to-many relationships between users and organizations
and allows roles (Owner, Admin, Member) to be assigned per organization.

> This table is the backbone of authorization in TaskPlanory.

---

## 4. Project

**Responsibility:**  
Groups related tasks within an organization to represent a logical unit of work.

**Why it exists:**  
Projects provide structure, ownership, and separation of work inside an organization,
similar to how teams operate in real-world project management systems.

---

## 5. Task

**Responsibility:**  
Represents an actionable unit of work with status, priority, due dates, and ownership.

**Why it exists:**  
Tasks are the core execution unit of TaskPlanory and the primary input for AI-based
prioritization and recommendations.

---

## 6. TaskComment

**Responsibility:**  
Stores user-generated discussions and context related to a task.

**Why it exists:**  
Comments enable collaboration and preserve decision context without cluttering
the task metadata itself.

---

## 7. ActivityLog

**Responsibility:**  
Stores an append-only audit trail of significant actions performed in the system.

**Why it exists:**  
Activity logs enable traceability, debugging, and future analytics while ensuring
accountability for changes made across organizations, projects, and tasks.

---

## 8. RefreshToken

**Responsibility:**  
Stores refresh tokens to manage secure, long-lived authentication sessions.

**Why it exists:**  
Refresh tokens allow secure logout, token rotation, and session revocation without
forcing users to re-authenticate frequently.

---

## Design Principles Followed

- Organizations act as strict tenant boundaries
- RBAC is enforced through data, not hardcoded logic
- Auditability is built-in via immutable activity logs
- Entities are intentionally scoped to avoid over-engineering in MVP

---

## Entity Boundaries

- **User**  
  Does NOT store role or authorization logic — separate table handles roles.

- **Organization**  
  Does NOT directly hold user data — membership table manages users.

- **OrganizationMember**  
  Only stores membership roles — does NOT store project/task data.

- **Project**  
  Does NOT handle tasks directly — tasks are separate.

- **Task**  
  Does NOT store comments — comments live in separate entity.

- **TaskComment**  
  Only stores comment text & references — NO business logic.

- **ActivityLog**  
  Append-only audit logs — Does NOT affect app state.

- **RefreshToken**  
  Only stores JWT refresh tokens — Does NOT store user secrets.


## Root vs Dependent Entities

- **Root Entities**  
  Entities that can be created without depending on another:  
  - User  
  - Organization

- **Dependent Entities**  
  Entities that depend on a parent entity’s existence:  
  - OrganizationMember  
  - Project  
  - Task  
  - TaskComment  
  - ActivityLog  
  - RefreshToken

## Design Assumptions

    1. Each task belongs to exactly one project and one organization.
    2. A user can belong to multiple organizations but only through OrganizationMember.
    3. AI-driven suggestions do not modify core data directly — user must approve.


---
End of Phase 1 – Day 1 Entity Definition
