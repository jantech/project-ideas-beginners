
# ⚡ TNEB Electricity Bill Calculator & Tracker

### 🏷️ Domain: Home Utility – Electricity Billing (Tamil Nadu Specific)

---

## 🎯 Project Objective

Develop a **full-stack web application** that:

* Calculates monthly electricity bills based on **TNEB slab rules**
* Stores **user consumption history**
* Allows **admins to update slabs and fixed charges**
* Offers **a responsive and user-friendly interface**
* Educates users on **how their bill is calculated**

---

## ✅ Core Features

### 👤 **User Module**

* Input:

  * Units consumed (monthly)
* Output:

  * Estimated bill amount
  * Fixed charges + slab breakdown
* Save consumption history (for logged-in users)

### 🔧 **Admin Module**

* CRUD for:

  * Rate slabs
  * Fixed charges
* Set/change billing rules dynamically
* View all user records (optional)

### 📊 **History Module**

* List past estimations with date, units, total amount
* Filter by date/month

---

## 🔢 TNEB Billing Logic

| Slab Range | Rate (₹/unit) |
| ---------- | ------------- |
| 0 – 100    | Free (₹0)     |
| 101 – 200  | ₹1.50         |
| 201 – 400  | ₹3.00         |
| 401 – 500  | ₹4.00         |
| 501 – 600  | ₹6.00         |
| 601 – 800  | ₹8.00         |
| 801 – 1000 | ₹9.00         |
| Above 1000 | ₹10.00        |

> Additional fixed charges may apply depending on slab range.

---

## 🧱 Tech Stack

| Layer        | Technology            |
| ------------ | --------------------- |
| Frontend     | React + Tailwind CSS  |
| Backend      | Node.js + Express.js  |
| Database     | MySQL                 |
| ORM          | Sequelize (preferred) |
| Auth (opt)   | JWT + bcrypt          |
| Charts (opt) | Recharts / Chart.js   |

---

## 🧾 Database Schema

### 1. `users` (optional for login)

```sql
id, name, email, password_hash, created_at
```

### 2. `rate_slabs`

```sql
id, min_units, max_units, rate_per_unit
-- E.g., 101–200 → ₹1.50
```

### 3. `fixed_charges`

```sql
id, applicable_above INT, charge DECIMAL(6,2)
-- E.g., applicable_above = 200, charge = ₹30
```

### 4. `consumption_records`

```sql
id, user_id (optional), units_consumed, total_amount, slab_breakdown TEXT, fixed_charge_applied, date
```

---

## 🔁 Backend API Design

### 🔹 Estimation

```
POST /estimate
Body: { units: 350 }
Response: {
  slab_breakdown: [
    { range: "101–200", rate: 1.5, units: 100, subtotal: 150 },
    { range: "201–350", rate: 3.0, units: 150, subtotal: 450 }
  ],
  fixed_charge: 30,
  total: 630
}
```

### 🔹 History

```
GET /history
GET /history?from=2024-01-01&to=2024-02-01
```

### 🔹 Admin APIs

```
GET    /admin/slabs
POST   /admin/slabs
PUT    /admin/slabs/:id
DELETE /admin/slabs/:id

GET    /admin/charges
POST   /admin/charges
```

---

## 🖥️ Frontend Implementation (React)

### Pages & Components

| Component             | Description                          |
| --------------------- | ------------------------------------ |
| EstimateForm.jsx      | Input units, trigger bill estimation |
| EstimateResult.jsx    | Show breakdown of slab + total       |
| HistoryTable.jsx      | List previous entries                |
| AdminSlabs.jsx        | Add/Edit/Delete rate slabs           |
| AdminCharges.jsx      | Manage fixed charges                 |
| Auth pages (optional) | Login/Register                       |

### Tools

* `Axios` for API requests
* `Formik` or `React Hook Form` for input validation
* `Tailwind CSS` for responsive design
* Optional: Charts for monthly usage summaries

---

## 💡 Sample Calculation (TNEB Style)

> Input: **350 units**

1. **0–100:** ₹0 → ₹0
2. **101–200:** ₹1.5 × 100 = ₹150
3. **201–350:** ₹3.0 × 150 = ₹450
4. **Fixed Charge:** ₹30 (applied above 200 units)

**Total Bill = ₹0 + ₹150 + ₹450 + ₹30 = ₹630**

---

## 🔁 Estimation Logic – Sample (Node.js)

```js
function calculateTNEBBill(units, slabs, fixedCharges) {
  let cost = 0;
  let remaining = units;
  const breakdown = [];

  for (let slab of slabs) {
    if (remaining <= 0) break;

    const min = slab.min_units;
    const max = slab.max_units || Infinity;
    const slabUnits = Math.min(remaining, max - min + 1);

    if (units > min) {
      const applicableUnits = Math.min(slabUnits, remaining);
      const subtotal = applicableUnits * slab.rate_per_unit;

      breakdown.push({
        range: `${min}-${max}`,
        rate: slab.rate_per_unit,
        units: applicableUnits,
        subtotal,
      });

      cost += subtotal;
      remaining -= applicableUnits;
    }
  }

  const fixed = fixedCharges.find(fc => units > fc.applicable_above)?.charge || 0;

  return {
    slab_breakdown: breakdown,
    fixed_charge: fixed,
    total: cost + fixed
  };
}
```

---

## 🗂 Project Structure

```
/tneb-bill-tracker
├── client/ (React app)
│   └── components, pages, routes
├── server/ (Express.js)
│   ├── routes/estimate.js, admin.js
│   ├── models/ (Sequelize)
│   └── controllers/
├── database/schema.sql
└── README.md
```

