
## 📚 Book Lending Library System

**Goal:** Manage a small-to-medium library's collection, borrowers, and book issuing/returning with status tracking.

---

## 🔑 CORE FEATURES

* Add/edit/delete books
* Register and manage borrowers
* Track issued books and return status
* Use **status field** (`Available`, `Issued`) for books
* Display lists with **React Table** (filter/sort)

---

## 🧱 DATABASE DESIGN

### 1. **Books**

```sql
Books (
  id UUID PRIMARY KEY,
  title VARCHAR(200),
  author VARCHAR(100),
  isbn VARCHAR(20),
  category VARCHAR(100),
  status ENUM('Available', 'Issued') DEFAULT 'Available',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 2. **Borrowers**

```sql
Borrowers (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  phone VARCHAR(20),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 3. **Lendings**

```sql
Lendings (
  id UUID PRIMARY KEY,
  book_id UUID REFERENCES Books(id) ON DELETE CASCADE,
  borrower_id UUID REFERENCES Borrowers(id) ON DELETE CASCADE,
  issue_date DATE,
  return_date DATE,
  status ENUM('Issued', 'Returned') DEFAULT 'Issued'
)
```

---

## 🌐 API ENDPOINTS

### 📘 Books

| Method | Endpoint         | Description      |
| ------ | ---------------- | ---------------- |
| GET    | `/api/books`     | Get all books    |
| POST   | `/api/books`     | Add a new book   |
| PUT    | `/api/books/:id` | Update book info |
| DELETE | `/api/books/:id` | Delete book      |

---

### 👤 Borrowers

| Method | Endpoint             | Description           |
| ------ | -------------------- | --------------------- |
| GET    | `/api/borrowers`     | List borrowers        |
| POST   | `/api/borrowers`     | Register new borrower |
| PUT    | `/api/borrowers/:id` | Edit borrower info    |
| DELETE | `/api/borrowers/:id` | Remove borrower       |

---

### 🔁 Lending

| Method | Endpoint               | Description              |
| ------ | ---------------------- | ------------------------ |
| POST   | `/api/lendings/issue`  | Issue a book to borrower |
| POST   | `/api/lendings/return` | Mark book as returned    |
| GET    | `/api/lendings`        | View lending history     |

---

## 🔄 ISSUE / RETURN FLOW

**Issue Logic:**

* POST `/api/lendings/issue`
* Update:

  * `Books.status = 'Issued'`
  * Insert record in `Lendings` with `status = 'Issued'`

**Return Logic:**

* POST `/api/lendings/return`
* Update:

  * `Books.status = 'Available'`
  * `Lendings.status = 'Returned'`, set `return_date`

---

## 🎨 UI SCREENS (React + React Table)

### 1. **Books List**

* Table columns:

  * Title, Author, ISBN, Category, Status
* Filters:

  * Category
  * Status (Available / Issued)
* Sort by title, author
* Actions:

  * Edit / Delete / Issue Book (if Available)

> 📘 Use: [React Table](https://tanstack.com/table) for pagination, filtering, sorting

---

### 2. **Borrower List**

* Columns: Name, Email, Phone
* Actions: Edit, Delete, View Borrowed Books

---

### 3. **Issue Book**

* Select Borrower (dropdown)
* Select Book (dropdown, only available ones)
* Confirm button triggers issue logic

---

### 4. **Return Book**

* List of currently issued books
* Button: Mark as Returned
* Updates `Lendings` & book status

---

### 5. **Lending History**

* Table: Book, Borrower, Issue Date, Return Date, Status
* Filters: Borrower, Date Range, Status

---

## 🧰 OPTIONAL FEATURES

* Search by book title or borrower name
* Email reminder on due return date
* Fine calculation for late returns
* QR code/book scanning support

---

## ⚙️ TECH STACK RECOMMENDATIONS

| Layer           | Tech                                        |
| --------------- | ------------------------------------------- |
| Frontend        | React + React Table + TailwindCSS           |
| Backend         | Node.js + Express            |
| Database        | MySQL                         |
| Auth (optional) | JWT                      |

