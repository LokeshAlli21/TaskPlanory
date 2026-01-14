# TaskPlanory — Product Notes (Phase 0)

## 1. Problem Statement

Teams spend a significant amount of time deciding **what to work on next** rather than actually executing work. Existing tools like Jira and Notion are excellent at tracking tasks and documentation, but they rely heavily on **manual prioritization and planning**, which increases cognitive load and planning overhead.

**TaskPlanory** aims to reduce this planning friction by using **AI-assisted decision-making** to help teams focus on execution instead of constant prioritization.

---

## 2. Product Vision

Build a modern, AI-powered work management platform that **assists users in planning, prioritizing, and executing tasks**, rather than acting as a passive task storage system.

> Vision: Help teams spend less time planning and more time executing.

---

## 3. Target Users

### Intended Users

* Small to mid-size teams
* Developers and startup teams
* Teams struggling with task prioritization and sprint planning

### Not Intended For (Initial Scope)

* Personal todo list users
* Large enterprise-scale organizations
* Highly complex project portfolios (out of scope for MVP)

---

## 4. Core User Journey

    1. User signs up and logs in
    2. Creates an organization (workspace)
    3. Invites team members
    4. Creates projects inside the organization
    5. Adds and assigns tasks
    6. AI assists with:

    * Task prioritization
    * Daily work recommendations
    7. Team completes a sprint
    8. AI generates sprint insights and summaries

---

## 5. Key Differentiators

### 1. AI-Driven Task Prioritization

Task priority is dynamically calculated using factors like due dates, workload, dependencies, and task age instead of being manually set.

### 2. Decision Assistance Over Task Tracking

The platform actively suggests **what the user should work on next**, rather than just displaying task lists.

### 3. Explainable AI

Every AI recommendation is accompanied by a clear explanation to maintain transparency and trust.

> TaskPlanory focuses on execution guidance, not just task storage.

---

## 6. AI Scope & Boundaries

### What AI Will Do

* Generate dynamic priority scores for tasks
* Recommend daily task execution order
* Convert natural language input into structured tasks
* Generate sprint summaries and bottleneck insights

### What AI Will NOT Do

* Automatically complete or modify tasks without user approval
* Replace human decision-making
* Act as a black-box system without explanations

---

## 7. Core Features (High-Level)

* Multi-tenant organizations
* Role-based access control (Owner / Admin / Member)
* Projects and task management
* Activity logging and audit trails
* AI-assisted prioritization and recommendations
* Sprint-level insights

---

## 8. Technology Stack (Finalized)

### Frontend

* React
* Redux Toolkit
* Tailwind CSS

### Backend

* Node.js
* Express.js
* PostgreSQL
* JWT Authentication with Refresh Tokens

### AI Layer

* Python (FastAPI) or Node-based AI service
* Rule-based + ML-lite logic

### Infrastructure

* AWS EC2
* AWS RDS
* Vercel (Frontend Deployment)

---

## 9. Non-Goals (Prevent Scope Creep)

The following features are intentionally excluded from the initial version:

* Real-time chat or messaging
* Live collaboration or WebSockets
* Mobile applications
* Complex Kanban animations
* Advanced custom ML model training

---

## 10. Phase Status

**Current Phase:** Phase 0 — Product Definition Complete

**Next Phase:** Phase 1 — System & Database Design
