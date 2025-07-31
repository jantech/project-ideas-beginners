
## 📚 Second-hand Book Selling App

**Domain:** Student Life
**Goal:** Allow students to easily list, browse, and connect to buy/sell used books.

---

## ✅ Core Features

* 📖 List books for sale with details & images
* 🔍 Search by title, subject, or author
* 📩 Contact seller via WhatsApp or email
* 🧾 Track book listings (sold/available)
* 🔄 CRUD for: Books, Sellers, Buyers

---

## 🧱 DATABASE DESIGN

### 1. **Users**

```sql
Users (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE,
  phone VARCHAR(20),
  role ENUM('Seller', 'Buyer', 'Both') DEFAULT 'Both',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 2. **Books**

```sql
Books (
  id UUID PRIMARY KEY,
  seller_id UUID REFERENCES Users(id) ON DELETE CASCADE,
  title VARCHAR(150),
  author VARCHAR(100),
  subject VARCHAR(100),
  condition VARCHAR(50), -- e.g., New, Good, Worn
  price DECIMAL(10, 2),
  image_url TEXT,
  status ENUM('Available', 'Sold') DEFAULT 'Available',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 3. **Messages (Optional)**

If you want in-app contact in addition to WhatsApp:

```sql
Messages (
  id UUID PRIMARY KEY,
  book_id UUID REFERENCES Books(id),
  sender_id UUID REFERENCES Users(id),
  message TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

## 🌐 API ENDPOINTS

### 📘 Books

| Method | Endpoint         | Description                                  |
| ------ | ---------------- | -------------------------------------------- |
| GET    | `/api/books`     | Get all available books (with search/filter) |
| GET    | `/api/books/:id` | Get book details                             |
| POST   | `/api/books`     | Add new book (seller only)                   |
| PUT    | `/api/books/:id` | Update book info or mark as sold             |
| DELETE | `/api/books/:id` | Delete book listing                          |

---

### 👥 Users (Seller/Buyer)

| Method | Endpoint         | Description              |
| ------ | ---------------- | ------------------------ |
| POST   | `/api/register`  | Register as seller/buyer |
| POST   | `/api/login`     | Login (JWT token)        |
| GET    | `/api/users/:id` | View profile             |

---

## 🔍 SEARCH & FILTER LOGIC

* Search bar: Title, Author
* Filters:

  * Subject (dropdown or tag-based)
  * Price range slider
  * Condition (checkboxes)

---

## 🎨 UI SCREENS

### 1. **Home / Book Listings**

* Grid or list view of available books
* Each card shows:

  * Title, Subject, Price, Seller Contact (WhatsApp link or Email button)

### 2. **Book Detail Page**

* Full info: Title, Author, Subject, Condition, Price
* Seller’s name + contact buttons (deep link to WhatsApp or mailto)
* "Mark as Sold" button (for seller)

### 3. **Add Book (Seller Dashboard)**

* Fields:

  * Title, Author, Subject, Condition, Price
  * Image upload
* Submit to POST `/api/books`

### 4. **My Listings (Seller)**

* Table view of all listed books
* Buttons: Edit / Delete / Mark as Sold

---

## 📱 WhatsApp Integration (Contact Seller)

Use deep links to initiate contact:

```txt
https://wa.me/<phone_number>?text=Hi! I'm interested in your book listing.
```

---

## 🧰 Optional Features

* Upload multiple images per book
* Report inappropriate listings
* Buyer reviews for sellers
* Saved/bookmarked books
* Share listing on social media

---

## ⚙️ TECH STACK

| Layer        | Stack Suggestion                            |
| ------------ | ------------------------------------------- |
| Frontend     | React (Next.js) + Tailwind CSS              |
| Backend      | Node.js + Express OR Django REST            |
| Database     | MySQL                       |
| Auth         | JWT-based Auth                  |

---

## 🔁 Listing Flow

1. Seller registers and logs in
2. Seller creates a book listing
3. Buyer searches/browses books
4. Buyer clicks WhatsApp link to message seller directly
5. Seller updates status to "Sold" once the deal is closed
