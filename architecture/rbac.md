# TaskPlanory – Role Based Access Control (RBAC)

## Overview
TaskPlanory uses Role-Based Access Control (RBAC) to manage permissions within an organization.
Authorization is scoped per organization to support multi-tenancy and flexible user roles.

RBAC is enforced at the backend middleware level before any business logic is executed.

---

## Roles

TaskPlanory supports three roles:

- **OWNER**
- **ADMIN**
- **MEMBER**

Roles are intentionally limited to reduce complexity while covering real-world use cases.

---

## Role Storage Design

Roles are stored in the `organization_members` table.

### organization_members
- user_id
- organization_id
- role

### Why this design?
- A user can belong to multiple organizations
- A user can have different roles in different organizations
- Authorization is tenant-scoped, not global

> Roles are never stored in the users table.

---

## Permission Matrix

| Action | OWNER | ADMIN | MEMBER |
|------|------|------|-------|
| Delete organization | ✅ | ❌ | ❌ |
| Update organization | ✅ | ❌ | ❌ |
| Invite members | ✅ | ✅ | ❌ |
| Remove members | ✅ | ✅ | ❌ |
| Create project | ✅ | ✅ | ❌ |
| Update project | ✅ | ✅ | ❌ |
| Delete project | ✅ | ❌ | ❌ |
| Create task | ✅ | ✅ | ❌ |
| Update any task | ✅ | ✅ | ❌ |
| Update own assigned task | ✅ | ✅ | ✅ |
| Assign tasks | ✅ | ✅ | ❌ |
| View tasks and projects | ✅ | ✅ | ✅ |

---

## Authorization Enforcement Strategy

RBAC is enforced using middleware.

### Authorization Flow
    1. User authenticates using JWT
    2. `user_id` is extracted from token
    3. `organization_id` is resolved from request
    4. User role is fetched from `organization_members`
    5. Permission is validated against required action
    6. Request is allowed or rejected

> Authorization is always checked before controller logic executes.

---

## Access Rules

- A user can only access data for organizations they are a member of
- Every protected API requires an organization context
- Queries are always filtered by `organization_id`

---

## Edge Case Handling

### Owner leaves organization
- Not allowed unless ownership is transferred

### Admin tries to delete organization
- Forbidden

### Member updates a task not assigned to them
- Forbidden

### User removed from organization
- All access is revoked immediately

---

## Design Principles

- Authorization is data-driven, not hardcoded
- Tenant isolation is enforced at both application and database level
- Simplicity is preferred over over-engineering

---

## Interview Summary

> TaskPlanory enforces RBAC through an organization membership table, with permissions validated in middleware before business logic execution. This design supports multi-tenancy, scalability, and secure access control.
