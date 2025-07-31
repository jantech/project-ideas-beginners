# **Warranty Tracker for Electronics**

**Domain:** Consumer Goods
**Use Case:** Track electronic devices’ purchase and warranty details, with reminders for expiring warranties.
**Tech Stack:**

* **Frontend:** HTML + CSS + Bootstrap OR React.
* **Backend:** Node.js + Express.
* **Database:** MySQL / MariaDB.

---

## **1. Project Scope**

**Goal:**
The app allows users to register their electronic devices with purchase and warranty details, view their warranty status (color-coded), and receive alerts when a warranty is about to expire.

**Target Users:**

* Consumers who want to track their gadgets’ warranties.
* Small businesses managing multiple electronic items.

---

## **2. Core Features**

### **Product Registration:**

* Add product details: Name, Brand, Model Number, Purchase Date, Price.
* Set warranty start and end dates (auto-calculate `end_date = start_date + duration`).

### **Warranty Status Dashboard:**

* List all devices with color-coded warranty status:

  * **Green:** Active warranty.
  * **Yellow:** Expiring within 30 days.
  * **Red:** Expired warranty.

### **Alerts & Reminders:**

* Highlight products with warranty expiring soon.
* Optional cron job to send email reminders (bonus).

### **Brand Management:**

* CRUD for brands (e.g., Samsung, LG, Sony).

### **CRUD Operations:**

* **Devices:** Add, view, edit, delete devices.
* **Brands:** Add, view, edit, delete brands.
* **Warranties:** Track warranty start & end dates for each device.

---

## **3. Database Schema (MySQL)**

```sql
-- Brands Table
CREATE TABLE brands (
  id          BIGINT PRIMARY KEY AUTO_INCREMENT,
  name        VARCHAR(100) NOT NULL UNIQUE,
  created_at  DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Devices Table
CREATE TABLE devices (
  id             BIGINT PRIMARY KEY AUTO_INCREMENT,
  name           VARCHAR(150) NOT NULL,
  brand_id       BIGINT NOT NULL,
  model_number   VARCHAR(100),
  purchase_date  DATE NOT NULL,
  price          DECIMAL(10,2),
  warranty_start DATE NOT NULL,
  warranty_end   DATE NOT NULL,
  created_at     DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (brand_id) REFERENCES brands(id) ON DELETE CASCADE
);
```

---

## **4. Warranty Logic**

* **Warranty Status:**

  ```sql
  SELECT *,
    CASE
      WHEN warranty_end < CURDATE() THEN 'Expired'
      WHEN warranty_end <= DATE_ADD(CURDATE(), INTERVAL 30 DAY) THEN 'Expiring Soon'
      ELSE 'Active'
    END AS warranty_status
  FROM devices;
  ```

* **Expiring Soon:** Devices with `warranty_end <= CURDATE() + 30 days`.

* **Color-Coding:**

  * **Green:** Active (`warranty_end > 30 days`).
  * **Yellow:** Expiring Soon (`0-30 days left`).
  * **Red:** Expired (`warranty_end < today`).

---

## **5. REST API Design**

### **Brands API**

* `POST /api/brands` – Add a new brand.
* `GET /api/brands` – Get all brands.
* `PUT /api/brands/:id` – Edit brand.
* `DELETE /api/brands/:id` – Delete brand.

### **Devices API**

* `POST /api/devices` – Add device with product + warranty details.
* `GET /api/devices` – Get all devices with warranty status.
* `GET /api/devices/:id` – Get device details.
* `PUT /api/devices/:id` – Edit device.
* `DELETE /api/devices/:id` – Remove device.

### **Alerts API (Bonus)**

* `GET /api/alerts/expiring` – List devices with warranty expiring in 30 days.

---

## **6. Frontend Pages**

1. **Dashboard:**

   * List all devices with color-coded warranty status.
   * Quick filter: “Expiring Soon” or “Expired”.

2. **Add/Edit Device Page:**

   * Form to enter product name, brand, model, purchase date, warranty start & end.

3. **Brand Management Page:**

   * Add, edit, delete brands.

4. **Alerts/Reminders Page (Optional):**

   * Highlight devices expiring soon.

---

## **7. Team Roles (3 Members)**

### **Week 1: Project Setup & Database**

* **Member A:** Setup Node.js + Express + routes.
* **Member B:** Create MySQL schema and seed brands.
* **Member C:** Create basic frontend template (HTML/Bootstrap).

### **Week 2: CRUD Implementation**

* **Member A:** API for Brands.
* **Member B:** API for Devices.
* **Member C:** Build UI forms for adding brands/devices.

### **Week 3: Dashboard & Warranty Logic**

* **Member A:** Implement warranty calculation on backend.
* **Member B:** Alerts (expiring soon devices).
* **Member C:** Dashboard UI with color-coded table.

### **Week 4: Testing & Polish**

* **All:** Connect frontend with backend (Fetch/Axios).
* Test CRUD and status logic.

---

## **8. Example Workflow**

1. User adds device:

   * **Name:** Washing Machine, **Brand:** LG, **Purchase Date:** 2024-02-01, **Warranty:** 2 years.
2. App calculates warranty end date: **2026-02-01**.
3. If today is 2026-01-10, status shows **Expiring Soon**.
4. After 2026-02-01, status changes to **Expired**.

---

## **9. Deliverables**

* Full source code (frontend + backend).
* SQL file for DB schema & sample data.
* README with setup instructions.
* Demo screenshots (Dashboard, Add Device).
* Team contribution report.

---
