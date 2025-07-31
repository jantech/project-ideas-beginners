## 🛒 Inventory Manager for Small Shops

**Goal:** Help small retail stores track inventory, manage categories, record sales, and get alerts for low stock.

---

## 🧱 DATABASE DESIGN

### 1. **Categories**

```sql
Categories (
  id UUID PRIMARY KEY,
  name VARCHAR(100) UNIQUE,
  description TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 2. **Products**

```sql
Products (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  sku VARCHAR(50) UNIQUE,
  category_id UUID REFERENCES Categories(id),
  price DECIMAL(10, 2),
  quantity INT,
  low_stock_threshold INT DEFAULT 5,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 3. **SalesLogs**

```sql
SalesLogs (
  id UUID PRIMARY KEY,
  product_id UUID REFERENCES Products(id),
  quantity_sold INT,
  sale_price DECIMAL(10, 2),
  total_amount DECIMAL(10, 2),
  sold_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

## 🌐 API ENDPOINTS

### 🔹 Categories

| Method | Endpoint              | Description         |
| ------ | --------------------- | ------------------- |
| GET    | `/api/categories`     | List all categories |
| POST   | `/api/categories`     | Add new category    |
| PUT    | `/api/categories/:id` | Update category     |
| DELETE | `/api/categories/:id` | Delete category     |

---

### 🔹 Products

| Method | Endpoint            | Description         |
| ------ | ------------------- | ------------------- |
| GET    | `/api/products`     | List all products   |
| GET    | `/api/products/:id` | Get product details |
| POST   | `/api/products`     | Add new product     |
| PUT    | `/api/products/:id` | Update product      |
| DELETE | `/api/products/:id` | Delete product      |

---

### 🔹 Sales

| Method | Endpoint             | Description                        |
| ------ | -------------------- | ---------------------------------- |
| GET    | `/api/sales`         | List all sales logs                |
| POST   | `/api/sales`         | Record a new sale and reduce stock |
| GET    | `/api/sales/summary` | Daily/weekly/monthly sales summary |

---

### 🔹 Alerts

| Method | Endpoint                | Description                   |
| ------ | ----------------------- | ----------------------------- |
| GET    | `/api/alerts/low-stock` | List products below threshold |

---

## 🔔 LOW STOCK ALERT LOGIC

Triggered:

* After each sale (`POST /api/sales`)
* Daily check via cron job

If:

```sql
product.quantity <= product.low_stock_threshold
```

Then:

* Mark in UI as low-stock
* Send in-app alert or email notification (optional)

---

## 🎨 UI SCREENS

### 1. **Dashboard**

* Summary widgets:

  * Total Products
  * Total Sales Today
  * Low Stock Alerts 🔔
* Recent sales (table)
* Button: "Add Product", "Sell Product"

---

### 2. **Products List**

* Table with:

  * Product Name
  * Category
  * Price
  * Quantity
  * Status (Low/OK)
* Filters:

  * Category, Low Stock only
* Actions: View, Edit, Delete

---

### 3. **Add / Edit Product**

* Fields:

  * Name, SKU
  * Category (dropdown)
  * Price, Quantity
  * Low Stock Threshold

---

### 4. **Sales Log**

* Table:

  * Product, Qty Sold, Date, Total Amount
* Filters:

  * Date range, Product
* Export to CSV option

---

### 5. **Sell Product (POS-lite)**

* Select product
* Input quantity sold
* Show price × qty = total
* Confirm button reduces stock & logs sale

---

### 6. **Low Stock Alerts Page**

* List of products below threshold
* Actions: Restock, Edit threshold

---

## ⚙️ TECH STACK

| Layer         | Tech                         |
| ------------- | ---------------------------- |
| Frontend      | React + Tailwind CSS         |
| Backend       | Node.js + Express            |
| Database      | MySQL                        |
| Auth          | JWT                          |
| Notifications | Email (Nodemailer) or in-app |

