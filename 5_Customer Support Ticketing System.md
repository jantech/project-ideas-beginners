
## 🛠️ Customer Support Ticketing System

**Goal:** Provide customers with a streamlined way to submit support requests and allow support agents/admins to track, manage, and respond to tickets.

---

## ✅ Core Features

* ✍️ Customers submit support tickets (with issue type & message)
* 📋 View ticket status: `Open`, `In Progress`, `Closed`
* 💬 Admin/Agent reply panel with threaded replies
* 👥 Basic user management (optional roles: Customer, Admin)

---

## 🧱 DATABASE DESIGN

### 1. **Users**

```sql
Users (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE,
  password_hash TEXT,
  role ENUM('Customer', 'Admin') DEFAULT 'Customer',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 2. **Tickets**

```sql
Tickets (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES Users(id),
  issue_type VARCHAR(100), -- e.g. 'Billing', 'Bug', 'Feature Request'
  subject VARCHAR(150),
  description TEXT,
  status ENUM('Open', 'In Progress', 'Closed') DEFAULT 'Open',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 3. **Replies**

```sql
Replies (
  id UUID PRIMARY KEY,
  ticket_id UUID REFERENCES Tickets(id) ON DELETE CASCADE,
  sender_id UUID REFERENCES Users(id),
  message TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

## 🌐 API ENDPOINTS

### 🔹 User Auth

| Method | Endpoint             | Description              |
| ------ | -------------------- | ------------------------ |
| POST   | `/api/auth/register` | Register a new user      |
| POST   | `/api/auth/login`    | Login (get JWT/token)    |
| GET    | `/api/users/me`      | Get current user profile |

---

### 🔹 Tickets

| Method | Endpoint           | Description                                     |
| ------ | ------------------ | ----------------------------------------------- |
| GET    | `/api/tickets`     | List tickets (admin: all, customer: own)        |
| GET    | `/api/tickets/:id` | View ticket details and replies                 |
| POST   | `/api/tickets`     | Submit a new ticket                             |
| PUT    | `/api/tickets/:id` | Update ticket (admin only — status, issue type) |
| DELETE | `/api/tickets/:id` | Delete ticket (admin only or ticket owner)      |

---

### 🔹 Replies

| Method | Endpoint                         | Description                  |
| ------ | -------------------------------- | ---------------------------- |
| POST   | `/api/tickets/:ticketId/replies` | Add reply to a ticket        |
| GET    | `/api/tickets/:ticketId/replies` | Get all replies for a ticket |

---

## 🎨 UI SCREENS

### 1. **Customer Portal**

#### Submit Ticket

* Fields:

  * Subject
  * Issue Type (dropdown)
  * Description
* Button: Submit

#### My Tickets

* Table: Subject, Status, Last Updated
* Filter: Status
* Click to view full thread & reply

---

### 2. **Admin Panel**

#### Ticket Management

* Dashboard: Open / In Progress / Closed counts
* Table: All tickets

  * Columns: User, Subject, Status, Created At
* Filters: Status, Date, Issue Type
* Actions: Change status, reply, delete

#### Reply to Ticket

* Threaded view of ticket + replies
* Message input for reply
* Status change dropdown (Open, In Progress, Closed)

---

### 3. **Login / Register**

* Email + Password
* Register form with basic validation

---

## 🔁 Ticket Flow Example

1. Customer submits ticket → status = `Open`
2. Admin responds → status may become `In Progress`
3. Customer replies → status remains `In Progress`
4. Admin closes ticket → status = `Closed`

---

## ⚙️ TECH STACK

| Layer         | Recommended Tech                            |
| ------------- | ------------------------------------------- |
| Frontend      | React + Tailwind CSS                        |
| Backend       | Node.js + Express            |
| Database      | MySQL                       |
| Auth          | JWT-based Auth                |
| Notifications | In-app, optional email (e.g. Nodemailer)    |

---
