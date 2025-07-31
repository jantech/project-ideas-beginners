# **Bill Split App**

**Domain:** Social Finance
**Tech Stack:** HTML + CSS + Bootstrap OR React (Frontend), Node.js (Backend), MySQL (Database)

---

## **1. Project Scope**

**Goal:**
Create a web app that helps groups (friends, roommates, event teams) split bills fairly. Users can:

* Create **bill sessions** (e.g., “Dinner at XYZ”).
* Add participants and the amount they paid.
* Automatically calculate **who owes whom**.
* Store **history of past bills**.

**Target Users:**

* Small groups who share expenses (e.g., college friends, club members).
* Admin/Organizer (optional role for managing sessions).

---

## **2. Core Features**

### **Must-Have Features:**

1. **Add Bill Session:**

   * Title, description, date.
2. **Add Participants:**

   * Name, amount paid.
3. **Bill Calculation:**

   * System calculates total bill, average share, and how much each person owes or is owed.
4. **History of Past Bills:**

   * View all past sessions with participants and balances.
5. **CRUD Operations:**

   * **Bill Sessions:** Create, Read, Update, Delete.
   * **Participants:** Add/Remove participants for each session.

### **Nice-to-Have Features (Optional):**

* Export bill summary as PDF or CSV.
* Split based on percentages or custom shares.
* Authentication (Login for users).
* Mobile-friendly responsive design.

---

## **3. Database Schema (MySQL)**

```sql
-- Table for storing bill sessions
CREATE TABLE bill_sessions (
  id          BIGINT PRIMARY KEY AUTO_INCREMENT,
  title       VARCHAR(200) NOT NULL,
  description TEXT,
  created_at  DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- Table for participants in a session
CREATE TABLE participants (
  id          BIGINT PRIMARY KEY AUTO_INCREMENT,
  session_id  BIGINT NOT NULL,
  name        VARCHAR(100) NOT NULL,
  amount_paid DECIMAL(10,2) NOT NULL DEFAULT 0.00,
  created_at  DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (session_id) REFERENCES bill_sessions(id) ON DELETE CASCADE
);
```

---

## **4. Bill Calculation Logic**

1. **Total Bill:** `SUM(amount_paid)` of all participants in the session.
2. **Equal Share:** `total_bill / number_of_participants`.
3. **Balance for Each Participant:**
   `balance = amount_paid - equal_share`.

   * If `balance < 0`, participant **owes** money.
   * If `balance > 0`, participant **should receive** money.

---

## **5. REST API Design**

### **Bill Sessions**

* `POST /api/sessions` – Create new bill session.
* `GET /api/sessions` – List all sessions.
* `GET /api/sessions/:id` – Get a specific session with participants & balances.
* `PUT /api/sessions/:id` – Update session details.
* `DELETE /api/sessions/:id` – Delete a session and its participants.

### **Participants**

* `POST /api/sessions/:id/participants` – Add participant.
* `DELETE /api/sessions/:id/participants/:pid` – Remove participant.

---

## **6. Frontend Pages**

1. **Home Page:**

   * List of all bill sessions.
   * Button to create a new session.

2. **Session Detail Page:**

   * Show participants and their amounts.
   * Form to add participants.
   * Display calculation results: total, per person share, who owes how much.

3. **History Page:**

   * List of past sessions with summary.

4. **(Optional) Export/Print Page:**

   * Download bill summary.

---

## **7. Project Components (Team of 3)**

### **Week 1: Setup & Database Design**

* **Member A:** Setup Node.js + Express backend.
* **Member B:** Setup MySQL database, create migrations for `bill_sessions` & `participants`.
* **Member C:** Setup frontend (HTML, CSS/Bootstrap or React structure).

### **Week 2: CRUD for Sessions & Participants**

* **Member A:** Create REST APIs for `bill_sessions`.
* **Member B:** Create REST APIs for `participants`.
* **Member C:** Build pages to list and create sessions.

### **Week 3: Calculation & History**

* **Member A:** Add balance calculation logic in backend.
* **Member B:** Connect frontend to API (AJAX or Fetch API).
* **Member C:** Build history page & display balance summary.

### **Week 4: Testing & Polish**

* **All:** Test all CRUD and calculation logic, create demo data, finalize UI.

---

## **8. Example Workflow**

1. User creates a session: **"Weekend Trip"**.
2. Adds participants: Alice (₹1000), Bob (₹500), Carol (₹1500).
3. Total = ₹3000; Each share = ₹1000.

   * Alice paid ₹1000 → owes 0.
   * Bob paid ₹500 → owes ₹500.
   * Carol paid ₹1500 → should receive ₹500.
4. User can save this session to **History**.

---

## **9. Non-Functional Requirements**

* Backend validation for amounts (must be >= 0).
* Responsive frontend design.
* Error handling with proper messages.
* Use `.env` for database config.
* Git version control with proper commits.

---
