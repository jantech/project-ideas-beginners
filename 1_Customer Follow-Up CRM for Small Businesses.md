# **Customer Follow‑Up CRM for Small Businesses**

A small shop/business needs a simple CRM to **store leads/customers, plan & track follow‑ups (calls/meetings), and update lead status** (Interested, Not Interested, Converted).

---

## 1) Goal & Problem Statement

Build a **full‑stack web application** that lets small-business owners and sales reps:

* **Add & manage Leads/Contacts**
* **Schedule & complete Follow‑ups** (calls, meetings, WhatsApp, email, etc.)
* **Track status**: *Interested*, *Not Interested*, *Converted*
* **Get reminders** for upcoming follow‑ups
* **(Optional)** Simple dashboards & activity history

---

## 2) Tech Stack (must use)

* **Frontend**:

  * **Option A:** HTML, CSS, Bootstrap, Vanilla JS
  * **Option B:** React (with React Router)
* **Backend:** Node.js + Express.js
* **Database:** MySQL / MariaDB
* **Other libs/concepts:** JWT auth, bcrypt, validation (Joi/Zod/express-validator), dotenv, a migration tool (Sequelize or raw SQL + migration scripts)

---

## 3) Roles & Permissions

* **Admin/Owner**

  * Manage users (sales reps)
  * Full CRUD on leads, contacts, follow-ups
  * View all data & reports
* **Sales Rep**

  * CRUD on leads/contacts/follow-ups **they own or are assigned**
  * Read-only on others (optional)
* **(Optional) Viewer**

  * Read-only access

---

## 4) Core Entities & CRUD

| Entity                     | Description                                              | CRUD Required                                                  |
| -------------------------- | -------------------------------------------------------- | -------------------------------------------------------------- |
| **User**                   | Admin & Sales reps (login + roles)                       | Create (Admin), Read, Update (self/admin), Soft Delete (Admin) |
| **Lead**                   | Potential or existing customer                           | Full CRUD                                                      |
| **Contact**                | Contact details for a lead (primary/secondary)           | Full CRUD                                                      |
| **FollowUp**               | Scheduled task to reach out to a lead/contact            | Full CRUD                                                      |
| **ActivityLog** (Optional) | Auto-logged actions (status change, follow-up completed) | Create, Read                                                   |

**Lead Status values** (ENUM): `INTERESTED`, `NOT_INTERESTED`, `CONVERTED`, `NEW` (optional default)

---

## 5) High-level Features

1. **Authentication & Authorization**

   * JWT-based login/logout
   * Role-based guards

2. **Leads Management**

   * Create, edit, view, delete leads
   * Set & change status
   * (Optional) Tagging & search/filter

3. **Contacts Management**

   * Multiple contacts per lead
   * Phone, email, position, notes

4. **Follow-ups**

   * Schedule follow-ups for leads/contacts (date, time, type, notes)
   * Auto-reminders for due/overdue follow-ups
   * Mark as completed with outcome

5. **Dashboard (Optional)**

   * Today’s upcoming follow-ups
   * Leads by status
   * Converted leads count (by date range)

6. **Audit Trail / Activity Log (Optional)**

   * Track who changed what (status, notes, etc.)

---

## 6) REST API Design (sample)

### Auth

* `POST /api/auth/register` (Admin only, to create Sales reps)
* `POST /api/auth/login`
* `GET /api/auth/me` (profile)

### Users (Admin)

* `GET /api/users`
* `GET /api/users/:id`
* `PUT /api/users/:id`
* `PATCH /api/users/:id/disable`
* `DELETE /api/users/:id` (soft delete)

### Leads

* `POST /api/leads`
* `GET /api/leads?status=&search=&assignedTo=&page=&limit=`
* `GET /api/leads/:id`
* `PUT /api/leads/:id`
* `PATCH /api/leads/:id/status` (body: { status })
* `DELETE /api/leads/:id`

### Contacts

* `POST /api/leads/:leadId/contacts`
* `GET /api/leads/:leadId/contacts`
* `GET /api/contacts/:id`
* `PUT /api/contacts/:id`
* `DELETE /api/contacts/:id`

### Follow-ups

* `POST /api/leads/:leadId/follow-ups`
* `GET /api/follow-ups?dueFrom=&dueTo=&status=&mine=`
* `GET /api/follow-ups/:id`
* `PUT /api/follow-ups/:id`
* `PATCH /api/follow-ups/:id/complete` (body: { outcome, notes })
* `DELETE /api/follow-ups/:id`

### Activity Logs (Optional)

* `GET /api/leads/:leadId/activity-logs`

---

## 7) Database Schema (MySQL/MariaDB)

> **Prefer migrations**. Below is the **minimal** schema (expand as needed).

```sql
-- users
CREATE TABLE users (
  id            BIGINT PRIMARY KEY AUTO_INCREMENT,
  name          VARCHAR(120) NOT NULL,
  email         VARCHAR(120) NOT NULL UNIQUE,
  password_hash VARCHAR(255) NOT NULL,
  role          ENUM('ADMIN','REP') NOT NULL DEFAULT 'REP',
  status        ENUM('ACTIVE','DISABLED') NOT NULL DEFAULT 'ACTIVE',
  created_at    DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at    DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  deleted_at    DATETIME NULL
);

-- leads
CREATE TABLE leads (
  id             BIGINT PRIMARY KEY AUTO_INCREMENT,
  name           VARCHAR(150) NOT NULL,
  company        VARCHAR(150) NULL,
  source         VARCHAR(100) NULL,
  status         ENUM('NEW','INTERESTED','NOT_INTERESTED','CONVERTED') NOT NULL DEFAULT 'NEW',
  owner_id       BIGINT NOT NULL,        -- FK users.id
  created_by     BIGINT NOT NULL,        -- FK users.id
  notes          TEXT NULL,
  created_at     DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at     DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  deleted_at     DATETIME NULL,
  FOREIGN KEY (owner_id) REFERENCES users(id),
  FOREIGN KEY (created_by) REFERENCES users(id)
);

-- contacts
CREATE TABLE contacts (
  id          BIGINT PRIMARY KEY AUTO_INCREMENT,
  lead_id     BIGINT NOT NULL,
  name        VARCHAR(120) NOT NULL,
  email       VARCHAR(120) NULL,
  phone       VARCHAR(30)  NULL,
  position    VARCHAR(100) NULL,
  is_primary  BOOLEAN NOT NULL DEFAULT FALSE,
  created_at  DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at  DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  deleted_at  DATETIME NULL,
  FOREIGN KEY (lead_id) REFERENCES leads(id)
);

-- follow_ups
CREATE TABLE follow_ups (
  id              BIGINT PRIMARY KEY AUTO_INCREMENT,
  lead_id         BIGINT NOT NULL,
  contact_id      BIGINT NULL,
  assigned_to     BIGINT NOT NULL,  -- FK users.id (rep responsible)
  type            ENUM('CALL','MEETING','EMAIL','WHATSAPP','OTHER') NOT NULL,
  due_datetime    DATETIME NOT NULL,
  status          ENUM('PENDING','COMPLETED','CANCELLED','MISSED') NOT NULL DEFAULT 'PENDING',
  outcome         VARCHAR(255) NULL,
  notes           TEXT NULL,
  created_at      DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at      DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  FOREIGN KEY (lead_id) REFERENCES leads(id),
  FOREIGN KEY (contact_id) REFERENCES contacts(id),
  FOREIGN KEY (assigned_to) REFERENCES users(id)
);

-- activity_logs (optional)
CREATE TABLE activity_logs (
  id          BIGINT PRIMARY KEY AUTO_INCREMENT,
  lead_id     BIGINT NOT NULL,
  user_id     BIGINT NOT NULL,
  action      VARCHAR(100) NOT NULL,            -- e.g., "STATUS_CHANGE", "FOLLOWUP_CREATED"
  meta_json   JSON NULL,
  created_at  DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (lead_id) REFERENCES leads(id),
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

---

## 9) Validation Rules (examples)

* **User Register/Login**

  * Email required, proper format, unique
  * Password min length 8
* **Lead**

  * `name` required
  * `status` ∈ {NEW, INTERESTED, NOT\_INTERESTED, CONVERTED}
* **Contact**

  * `lead_id` must exist
  * `email` optional, but if provided must be valid
* **Follow-up**

  * `lead_id` must exist
  * `due_datetime` must be in the future when creating
  * `type` ∈ {CALL, MEETING, EMAIL, WHATSAPP, OTHER}

---

## 10) Frontend Pages / Screens

1. **Auth**

   * Login / (Admin) Create User
2. **Dashboard**

   * Today’s follow-ups
   * Leads by status
3. **Leads**

   * List with filters (status, search by name/company)
   * Create/Edit lead
   * Lead details: contacts, follow-ups, activity log
4. **Contacts**

   * Attached to a lead; Manage primary contact
5. **Follow-ups**

   * List (mine/all), calendar view (optional)
   * Create/Edit/Complete follow-up
6. **User Management** (Admin)

---

## 12) Security & Non-Functional Requirements

* JWT auth (short-lived access token + refresh token)
* Passwords hashed with bcrypt
* Role-based route protection
* Input validation (server & client)
* Pagination on list endpoints
* Consistent error format `{ message, code, details }`
* Basic logging (request + error)
* .env for secrets (never commit)
* Soft delete for critical tables (users, leads) if needed

---

## 13) Milestones & **Rotation Plan** (3 members: A, B, C)

> **Duration Suggestion:** 5–6 weeks
> **Rule:** Each week, **each member implements one full “feature slice” end-to-end** (DB → API → UI) for **different entities**, so everyone touches **frontend, backend, and DB**.

### Week 1 – Inception & Setup

* **All**

  * Finalize scope, roles, statuses, user stories
  * ER diagram, API blueprint (Swagger/OpenAPI)
  * Repo, Git strategy, ESLint/Prettier, commit hooks
* **A**: Express + base project + health route
* **B**: DB init + migration tooling
* **C**: Frontend scaffold (React or Bootstrap) + routing skeleton

**Deliverables**: SRS, ERD, API doc, wireframes, repo ready

### Week 2 – Auth & Users

* **A (feature slice)**: Users table migration + Users CRUD API + JWT auth + role middleware
* **B**: Frontend login/register/profile + protected routes
* **C**: Backend tests for auth routes + request validation

**Rotation next week**: B handles backend feature, C handles frontend, etc.

### Week 3 – Leads

* **B (feature slice)**: Leads table migration + Leads API (CRUD + status change)
* **C**: Leads UI (list with filters, create/edit, detail view)
* **A**: Integration tests for leads + error handling middleware

### Week 4 – Contacts

* **C (feature slice)**: Contacts migration + nested contacts API under leads
* **A**: Contacts UI on Lead Detail page
* **B**: Data access refactor (repositories/services) + pagination & search

### Week 5 – Follow-ups & Reminders

* **A (feature slice)**: Follow-ups migration + API (CRUD + complete)
* **B**: Follow-ups UI (list, create, complete, today’s overdue)
* **C**: Reminder worker/cron (optional) + Activity log (optional) + Swagger docs polish

---


## 14) User Stories (examples)

* **As a Sales Rep**, I can create a lead and mark it as *Interested* so I can prioritize it.
* **As an Admin**, I can create users and assign leads to them.
* **As a Sales Rep**, I can schedule a follow-up call for a lead and get reminded on the due date.
* **As a Sales Rep**, I can mark a follow-up as completed with an outcome to keep history.
* **As an Owner**, I can see how many leads were converted this month.

---

## 15) Acceptance Criteria (sample)

**Follow-up completion**

* **Given** a Sales Rep is logged in and has a pending follow-up for a lead
* **When** they call `PATCH /api/follow-ups/:id/complete` with `{ outcome, notes }`
* **Then** the follow-up status becomes `COMPLETED`, and an activity log entry is stored

**Lead status change**

* **Given** a lead exists with status `INTERESTED`
* **When** a Sales Rep calls `PATCH /api/leads/:id/status` with `{ status: "CONVERTED" }`
* **Then** the status updates and the system records the change

---