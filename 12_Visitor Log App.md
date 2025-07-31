## 🏢 Visitor Log App (Office/Clinic)

**Goal:** Allow front desk staff to log and track visitors daily with essential details and minimal hassle.

---

## 🔑 Core Features

* ✍️ Add visitor info: Name, reason, time in, time out
* 📅 Filter logs by date
* 🧹 Auto-delete or archive old logs monthly
* 🔄 CRUD for Visitor records
* 📋 Simple and responsive UI for reception usage

---

## 🧱 DATABASE DESIGN

### 1. **Visitors**

```sql
Visitors (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  reason TEXT,
  time_in TIMESTAMP,
  time_out TIMESTAMP,
  date DATE DEFAULT CURRENT_DATE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

## 🌐 API ENDPOINTS

### 🔹 Visitors (CRUD)

| Method | Endpoint                    | Description                                    |
| ------ | --------------------------- | ---------------------------------------------- |
| GET    | `/api/visitors`             | List visitors (optionally filter by date)      |
| POST   | `/api/visitors`             | Add new visitor entry (name, reason, time\_in) |
| PUT    | `/api/visitors/:id`         | Update visitor info (e.g. time\_out)           |
| DELETE | `/api/visitors/:id`         | Delete individual visitor                      |
| DELETE | `/api/visitors/archive-old` | Bulk archive/delete logs older than 30 days    |

---

## 🔁 AUTO-CLEAR LOGIC

### Run daily (backend cron job):

```sql
DELETE FROM Visitors
WHERE date < CURRENT_DATE - INTERVAL '30 days';
```

* Can be replaced with an **archiving strategy** if deletion isn't preferred.

---

## 🎨 UI SCREENS (Reception-Friendly)

### 1. **Daily Visitor Log View**

* Table with columns:

  * Name
  * Reason
  * Time In
  * Time Out
* Filters:

  * Date picker (default = today)
* Actions:

  * Mark “Time Out” for visitors still inside
  * Edit or Delete entry

---

### 2. **Add New Visitor Form**

* Fields:

  * Name (text input)
  * Reason (textarea or dropdown: Appointment, Delivery, Interview, etc.)
  * Time In (auto-filled with current time, editable)
* Submit button: “Log Entry”

---

### 3. **Edit Visitor Form**

* Mark or update time out
* Edit reason if needed

---

## 🧰 OPTIONAL FEATURES

* PDF/CSV export by date range
* Print badge or QR for visitor
* Host selection (who they are visiting)
* Signature or ID scan upload
* Email alert to host on check-in
* Access control hardware integration

---

## ⚙️ TECH STACK

| Layer      | Suggested Stack                                  |
| ---------- | ------------------------------------------------ |
| Frontend   | React + Tailwind CSS                             |
| Backend    | Node.js + Express or Django REST                 |
| Database   | PostgreSQL or SQLite                             |
| Hosting    | Vercel (frontend), Railway/Render (backend)      |
| Auth       | Optional (public kiosk mode or staff login)      |
| Automation | Cron job via `node-cron` or `Celery` for cleanup |

---

## 🖥️ Ideal Usage Scenarios

* Office reception desk
* Small clinic or dental office
* School entry/exit logging
* Coworking spaces tracking daily visitors

