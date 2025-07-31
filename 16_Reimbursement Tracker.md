## 💼 Expense Reimbursement Tracker

**Domain:** Office / Freelance
**Purpose:** Submit expenses for approval with receipts, and track status through the reimbursement workflow.

---

## ✅ Core Features

* 💸 Submit expense with amount, category, and description
* 📎 Upload receipt (image or PDF)
* ✅ Approve or reject submitted expenses (Manager/Admin only)
* 🔄 Track status: `Pending`, `Approved`, `Rejected`
* 📊 View history by user, category, or date
* 🔄 CRUD: Users, Expenses, Receipts, Categories, Approvals

---

## 🧱 DATABASE DESIGN

### 1. **Users**

```sql
Users (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE,
  role ENUM('Employee', 'Manager'),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 2. **ExpenseCategories**

```sql
ExpenseCategories (
  id UUID PRIMARY KEY,
  name VARCHAR(100),     -- e.g., Travel, Food, Office Supplies
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 3. **Expenses**

```sql
Expenses (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES Users(id),
  category_id UUID REFERENCES ExpenseCategories(id),
  amount DECIMAL(10, 2),
  description TEXT,
  receipt_url TEXT,
  status ENUM('Pending', 'Approved', 'Rejected') DEFAULT 'Pending',
  submitted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  approved_at TIMESTAMP,
  approved_by UUID REFERENCES Users(id)
)
```

---

## 🌐 API ENDPOINTS

### 👥 Users

| Method | Endpoint         | Description       |
| ------ | ---------------- | ----------------- |
| POST   | `/api/register`  | Register new user |
| POST   | `/api/login`     | User login        |
| GET    | `/api/users/:id` | View user profile |

---

### 💳 Expenses

| Method | Endpoint            | Description                                    |
| ------ | ------------------- | ---------------------------------------------- |
| GET    | `/api/expenses`     | List all expenses (filter by user/status/date) |
| POST   | `/api/expenses`     | Submit a new expense                           |
| GET    | `/api/expenses/:id` | View single expense                            |
| PUT    | `/api/expenses/:id` | Update expense (employee or manager)           |
| DELETE | `/api/expenses/:id` | Delete (if pending and by owner)               |

---

### 📂 Categories

| Method | Endpoint              | Description                 |
| ------ | --------------------- | --------------------------- |
| GET    | `/api/categories`     | List all categories         |
| POST   | `/api/categories`     | Add category (manager only) |
| PUT    | `/api/categories/:id` | Edit category               |
| DELETE | `/api/categories/:id` | Remove category             |

---

## 🎨 UI SCREENS

### 1. **Dashboard**

* Tabs:

  * My Expenses (employee)
  * All Expenses (manager)
* Stats:

  * Total Submitted
  * Pending / Approved
  * Filters: Date, Category, Status

---

### 2. **Submit Expense**

* Fields:

  * Amount, Category (dropdown), Description
  * Upload receipt (file input → image/PDF preview)
* Submit → Sets status to `Pending`

---

### 3. **Expense Table**

* Columns:

  * Date | Category | Amount | Status | Actions (View/Edit/Delete)
* Color-coded status badges:

  * 🟡 Pending, ✅ Approved, ❌ Rejected

---

### 4. **Manager Panel**

* View all submitted expenses
* Filter: By user, category, or status
* Action buttons: `Approve`, `Reject`
* Optional comment on rejection

---

### 5. **Receipt Viewer**

* Inline preview of image or PDF receipt
* Download or open in new tab

---

## 📥 Receipt Upload & Storage

* Use Cloudinary, Firebase Storage, or Amazon S3
* Frontend: drag-and-drop file uploader
* Restrict file type to image/pdf and max size (e.g., 5MB)

---

## ⚙️ TECH STACK

| Layer       | Stack Suggestion                 |
| ----------- | -------------------------------- |
| Frontend    | React + Tailwind CSS             |
| Backend     | Node.js + Express                |
| Database    | MySQL                            |
| Auth        | JWT-based                        |
| File Upload | Multer (Node), or base64 images  |

---
