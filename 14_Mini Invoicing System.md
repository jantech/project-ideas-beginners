## 🧾 Mini Invoicing System

**Domain:** Freelance / Business
**Goal:** Help freelancers generate, send, and track simple invoices for their clients.

---

## ✅ Core Features

* 🧑‍💼 Manage clients
* 🧮 Add services/products to invoices
* 💸 Mark invoices as Paid/Unpaid
* 📥 Download or email invoice as PDF (optional)
* 🔄 CRUD for: Clients, Services, Invoices

---

## 🧱 DATABASE DESIGN

### 1. **Clients**

```sql
Clients (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  company_name VARCHAR(100),
  address TEXT,
  phone VARCHAR(20),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 2. **Services**

```sql
Services (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  description TEXT,
  unit_price DECIMAL(10, 2),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 3. **Invoices**

```sql
Invoices (
  id UUID PRIMARY KEY,
  client_id UUID REFERENCES Clients(id),
  invoice_number VARCHAR(50) UNIQUE,
  issue_date DATE,
  due_date DATE,
  status ENUM('Paid', 'Unpaid') DEFAULT 'Unpaid',
  total_amount DECIMAL(10, 2),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 4. **Invoice\_Items**

```sql
Invoice_Items (
  id UUID PRIMARY KEY,
  invoice_id UUID REFERENCES Invoices(id) ON DELETE CASCADE,
  service_id UUID REFERENCES Services(id),
  quantity INT,
  unit_price DECIMAL(10, 2),
  total DECIMAL(10, 2)
)
```

---

## 🌐 API ENDPOINTS

### 📇 Clients

| Method | Endpoint           | Description     |
| ------ | ------------------ | --------------- |
| GET    | `/api/clients`     | Get all clients |
| POST   | `/api/clients`     | Add new client  |
| PUT    | `/api/clients/:id` | Update client   |
| DELETE | `/api/clients/:id` | Delete client   |

---

### 🛠️ Services

| Method | Endpoint            | Description      |
| ------ | ------------------- | ---------------- |
| GET    | `/api/services`     | Get all services |
| POST   | `/api/services`     | Add service      |
| PUT    | `/api/services/:id` | Edit service     |
| DELETE | `/api/services/:id` | Delete service   |

---

### 🧾 Invoices

| Method | Endpoint                | Description                                 |
| ------ | ----------------------- | ------------------------------------------- |
| GET    | `/api/invoices`         | List all invoices                           |
| GET    | `/api/invoices/:id`     | View single invoice                         |
| POST   | `/api/invoices`         | Create new invoice                          |
| PUT    | `/api/invoices/:id`     | Update invoice (mark as paid/unpaid)        |
| DELETE | `/api/invoices/:id`     | Delete invoice                              |
| GET    | `/api/invoices/:id/pdf` | Generate/download invoice as PDF (optional) |

---

## 🎨 UI SCREENS

### 1. **Dashboard**

* Quick stats:

  * Total Invoices
  * Unpaid Invoices
  * Total Revenue
* Recent activity

---

### 2. **Clients**

* Table: Name, Email, Company, Actions (Edit/Delete)

---

### 3. **Services**

* Table: Name, Unit Price, Actions

---

### 4. **Invoices**

* Table: Invoice #, Client, Status (Paid/Unpaid), Total, Due Date
* Filter by: Client, Status, Date
* Actions: View, Edit, Delete, Mark as Paid, Download PDF

---

### 5. **Invoice Creator Form**

* Select Client
* Add line items:

  * Service (dropdown), Quantity → Auto-calculate price
* Add notes, tax, discounts
* Generate invoice number auto-incrementally (e.g., `INV-2025-001`)
* Submit → Save & preview

---

### 6. **Invoice Preview (PDF-style)**

* Styled like a real invoice:

  * Logo, your business info
  * Client details
  * Line items with totals
  * Status: Paid/Unpaid
* Buttons:

  * Mark as Paid
  * Download PDF
  * Send by Email (optional)

---

## 📥 PDF Generation (Optional)

Use tools like:

* `pdfkit` (Node.js)
* `WeasyPrint` (Python)
* Or frontend tools like `html2pdf.js`
  Export styled invoice HTML into downloadable PDFs.

---

## ⚙️ TECH STACK

| Layer    | Stack Suggestion                            |
| -------- | ------------------------------------------- |
| Frontend | React + Tailwind CSS                        |
| Backend  | Node.js + Express             |
| Database | MySQL                          |
| Auth     | Optional: JWT                               |
| PDF Tool | `pdfkit`, `jsPDF`, `Puppeteer`              |

---
