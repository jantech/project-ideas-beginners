
## 📦 Courier Delivery Tracker

**Domain:** Logistics / Local Delivery
**Goal:** Track courier orders, their status, and assignment to delivery agents.

---

## ✅ Core Features

* 📝 Create delivery orders with recipient details
* 🚴 Assign courier agents to orders
* 🔄 Update and track delivery status (e.g., In Transit, Delivered)
* 📚 View delivery history/logs per order
* 🔄 CRUD for: Orders, Couriers, Status Logs

---

## 🧱 DATABASE DESIGN

### 1. **Couriers**

```sql
Couriers (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  phone VARCHAR(20),
  email VARCHAR(100),
  vehicle_type VARCHAR(50), -- e.g., Bike, Van
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 2. **Orders**

```sql
Orders (
  id UUID PRIMARY KEY,
  order_number VARCHAR(50) UNIQUE,
  sender_name VARCHAR(100),
  recipient_name VARCHAR(100),
  recipient_address TEXT,
  recipient_phone VARCHAR(20),
  courier_id UUID REFERENCES Couriers(id),
  status ENUM('Pending', 'In Transit', 'Delivered', 'Failed') DEFAULT 'Pending',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  delivery_date DATE
)
```

---

### 3. **StatusLogs**

```sql
StatusLogs (
  id UUID PRIMARY KEY,
  order_id UUID REFERENCES Orders(id) ON DELETE CASCADE,
  status ENUM('Pending', 'In Transit', 'Delivered', 'Failed'),
  message TEXT,
  updated_by VARCHAR(100), -- agent/admin
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

## 🌐 API ENDPOINTS

### 🚚 Couriers

| Method | Endpoint            | Description         |
| ------ | ------------------- | ------------------- |
| GET    | `/api/couriers`     | List all couriers   |
| POST   | `/api/couriers`     | Add new courier     |
| PUT    | `/api/couriers/:id` | Update courier info |
| DELETE | `/api/couriers/:id` | Remove courier      |

---

### 📦 Orders

| Method | Endpoint          | Description                    |
| ------ | ----------------- | ------------------------------ |
| GET    | `/api/orders`     | List all orders                |
| GET    | `/api/orders/:id` | View specific order            |
| POST   | `/api/orders`     | Create new order               |
| PUT    | `/api/orders/:id` | Update order details or status |
| DELETE | `/api/orders/:id` | Cancel/remove order            |

---

### 📄 Status Logs

| Method | Endpoint                           | Description                  |
| ------ | ---------------------------------- | ---------------------------- |
| GET    | `/api/orders/:orderId/status-logs` | Get status history for order |
| POST   | `/api/orders/:orderId/status-logs` | Add a new status update      |

---

## 🎨 UI SCREENS

### 1. **Dashboard**

* Overview:

  * Total Orders
  * In Transit / Delivered / Pending counts
* Table: Recent orders + current status
* Quick filters: `Status`, `Courier`, `Date`

---

### 2. **Orders**

#### List View

* Table Columns:

  * Order #
  * Recipient Name
  * Assigned Courier
  * Status
  * Actions: View / Update / Delete

#### Create/Edit Order

* Fields:

  * Order #
  * Recipient Info (name, address, phone)
  * Assign Courier (dropdown)
  * Status (default: Pending)

#### Order Detail

* Order info
* Assigned courier
* Status log timeline (e.g., like shipment tracking)
* Button: Add Status Update

---

### 3. **Couriers**

* List of couriers (name, phone, vehicle)
* Actions: Edit / Remove / View Assigned Orders

---

### 4. **Mobile Agent Panel (optional)**

* View assigned orders
* Update delivery status on-the-go (e.g., “Delivered” with photo proof)

---

## 📥 Status Flow Example

1. Order created → status: `Pending`
2. Courier assigned → status: `In Transit`
3. Courier updates → status: `Delivered`
4. Each update logs a message (e.g., "Package picked up")

---

## ⚙️ TECH STACK

| Layer           | Stack Suggestion                                  |
| --------------- | ------------------------------------------------- |
| Frontend        | React (or React Native for agents) + Tailwind CSS |
| Backend         | Node.js + Express OR Django REST                  |
| Database        | MySQL                              |
| Auth (optional) | JWT-based user access                             |

