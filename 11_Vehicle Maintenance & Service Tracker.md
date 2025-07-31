
# 🚘 Vehicle Maintenance & Service Tracker

### 🔍 Summary

A full-stack web application to track maintenance, repairs, and reminders for one or more vehicles. Users can log services like oil changes, tire rotations, and view maintenance history. The app helps avoid missed service deadlines with a built-in reminder system.

---

## 🎯 Project Scope

### 👥 Target Users

* Vehicle owners tracking personal vehicle maintenance
* Small garages managing customer service history

### 🎯 Core Objectives

* Keep digital maintenance records per vehicle
* Remind users of due maintenance based on mileage or time
* Offer history view and downloadable service reports
* Be accessible across devices (mobile-responsive)

---

## 📦 Features & Modules

### 1. **Vehicle Management**

* Add/Edit/Delete vehicles
* Fields:

  * Make, Model, Year, Reg No, Odometer
  * Vehicle image (optional)
* Assign to user (if multi-user)

### 2. **Service Logs**

* Log maintenance events:

  * Oil Change, Brake Check, Filter Change, Tire Rotation, etc.
* Fields:

  * Type, Date, Odometer, Cost, Notes
* Custom service types

### 3. **Reminder System**

* Set by:

  * Mileage (e.g., every 5000 km)
  * Time (e.g., every 6 months)
* Dashboard “Upcoming Services” section
* Email/Notification (optional via Cron + Nodemailer)

### 4. **Service History**

* Timeline/list of past services
* Filter:

  * By date, type, vehicle, cost
* Download/Print as PDF

### 5. **User Authentication (Optional)**

* Register/Login (JWT-based)
* Users see only their own vehicles and logs


## 🗂 Tech Stack

| Layer       | Technology                |
| ----------- | ------------------------- |
| Frontend    | React.js + Tailwind CSS   |
| Backend     | Node.js + Express.js      |
| Database    | MySQL                     |
| Auth        | JWT + Bcrypt (optional)   |
| PDF Gen     | jsPDF or react-pdf        |

---

## 🧱 Database Schema (MySQL)

### 🔹 Users (optional)

```sql
id (PK), name, email (unique), password_hash, created_at
```

### 🔹 Vehicles

```sql
id (PK), owner_id (FK), make, model, year, reg_no, current_odometer, image_url
```

### 🔹 Service\_Records

```sql
id (PK), vehicle_id (FK), service_type, date, odometer, cost, notes
```

### 🔹 Reminders

```sql
id (PK), vehicle_id (FK), service_type, trigger_type (ENUM: 'km'|'date'), interval_km, interval_days, last_service_date, next_due_km, next_due_date
```

---

## 🧩 Backend (Node.js + Express.js)

### API Routes Overview

#### 🔐 Auth

```
POST   /auth/register
POST   /auth/login
```

#### 🚗 Vehicles

```
GET    /vehicles
POST   /vehicles
PUT    /vehicles/:id
DELETE /vehicles/:id
```

#### 🛠 Services

```
GET    /vehicles/:id/services
POST   /services
PUT    /services/:id
DELETE /services/:id
```

#### ⏰ Reminders

```
GET    /vehicles/:id/reminders
POST   /reminders
PUT    /reminders/:id
DELETE /reminders/:id
```

#### 📊 Dashboard

```
GET    /dashboard/upcoming
GET    /dashboard/summary
```

### 🔐 Authentication (Optional)

* JWT-based middleware
* Protected routes for vehicle/services/reminders
* Passwords hashed with bcrypt

---

## 💻 Frontend (React.js + Tailwind)

### Structure

```
src/
  components/
    VehicleForm.jsx
    VehicleList.jsx
    ServiceForm.jsx
    ReminderForm.jsx
    Dashboard.jsx
    PDFDownload.jsx
  pages/
    Login.jsx
    Register.jsx
    Vehicles.jsx
    Services.jsx
    Reminders.jsx
    Dashboard.jsx
  api/
    vehicleApi.js
    serviceApi.js
    reminderApi.js
  App.jsx
  main.jsx
```

### Key Frontend Features

* React Router for page navigation
* Fetch for API communication
* Form validation (basic)
* Tailwind for responsive design
* jsPDF/React-to-PDF for service report downloads
* Chart.js/Recharts for cost summaries

---

## 📅 Development Plan (Suggested Roadmap)

### **Week 1**

* Setup project structure (frontend/backend)
* Design DB schema & test with sample data
* Build user authentication

### **Week 2**

* Implement CRUD for vehicles
* Add service record module
* Setup image upload

### **Week 3**

* Add reminder system (create/view)
* Build dashboard for upcoming services
* Setup cron jobs for automatic checks

### **Week 4**

* Add PDF download and filtering
* Integrate Chart.js for service summary
* Responsive UI polish

---


## 🧩 Feature-Based Team Split (All Do Full Stack)

| Student       | Assigned Module                 | Scope (Frontend + Backend + DB)                                      |
| ------------- | ------------------------------- | -------------------------------------------------------------------- |
| **Student A** | **Vehicle Management**          | Add/Edit/Delete vehicles, upload image, display vehicle list/details |
| **Student B** | **Service Log Management**      | CRUD for service records, filtering, show history for each vehicle   |
| **Student C** | **Reminder System + Dashboard** | Reminder logic (km/date), show upcoming services, dashboard summary  |

Each student will:

* Build frontend React components
* Create API endpoints using Express
* Design the DB tables and write queries

---

## 🔁 How the Modules Connect

```
Vehicle (Student A)
   └── Services (Student B)
        └── Reminders (Student C)
             └── Dashboard (Student C)
```

To keep boundaries clear:

* Students **share the base Express server and DB**
* Students define their own routes, models, and frontend pages/components

---

## 🎓 Individual Responsibilities Breakdown

### 👩‍🎓 Student A – Vehicle Module (Full Stack)

#### Frontend

* VehicleForm.jsx: Add/edit vehicle
* VehicleList.jsx: Show vehicle cards
* VehicleDetails.jsx: Odometer, image, etc.

#### Backend

* `GET /vehicles`
* `POST /vehicles`
* `PUT /vehicles/:id`
* `DELETE /vehicles/:id`
* Upload image (Multer)

#### DB

* `vehicles` table:

  ```sql
  id, make, model, year, reg_no, current_odometer, image_url
  ```

---

### 👨‍🎓 Student B – Service Log Module (Full Stack)

#### Frontend

* ServiceForm.jsx: Add/Edit service
* ServiceHistory.jsx: List all logs
* FilterComponent.jsx: Filter by date/type/cost

#### Backend

* `GET /vehicles/:id/services`
* `POST /services`
* `PUT /services/:id`
* `DELETE /services/:id`

#### DB

* `service_records` table:

  ```sql
  id, vehicle_id (FK), service_type, date, odometer, cost, notes
  ```

---

### 👩‍🎓 Student C – Reminder + Dashboard Module (Full Stack)

#### Frontend

* ReminderForm.jsx: Set reminders
* UpcomingReminders.jsx: List upcoming ones
* Dashboard.jsx: Monthly summary chart (Chart.js)
* PDFDownload.jsx: Download report

#### Backend

* `GET /vehicles/:id/reminders`
* `POST /reminders`
* `PUT /reminders/:id`
* Cron job to calculate next\_due
* `GET /dashboard/summary`

#### DB

* `reminders` table:

  ```sql
  id, vehicle_id (FK), service_type, trigger_type, interval_km, interval_days,
  last_service_date, next_due_km, next_due_date
  ```

---

## 📦 Shared Development Tasks

| Task                        | Assigned To (Pair or All)         |
| --------------------------- | --------------------------------- |
| Set up GitHub Monorepo      | All (collaborative setup)         |
| Setup MySQL                 | All (one DB, shared schema)       |
| Create ER Diagram           | All (on draw\.io or dbdiagram.io) |
| Initial Express boilerplate | All (team config + route base)    |
| Common styles & layout      | Student A + others reuse          |
| API documentation (Postman) | Each owns their module docs       |

---

## 🧰 Dev Environment Setup

```
/vehicle-service-tracker/
├── /client/        → React frontend
├── /server/        → Express backend
├── /uploads/       → Vehicle images
├── /docs/          → ERD, project plan, Postman collection
└── README.md
```

Use:

* GitHub with separate branches: `feature/vehicles`, `feature/services`, etc.
* Shared `.env` for DB config
* Git pull/merge weekly to main

---

## 🗓 Project Timeline & Milestones

| Week | Tasks                                                           |
| ---- | --------------------------------------------------------------- |
| 1    | Team setup, DB schema, project scaffold (frontend + backend)    |
| 2    | Each student builds their assigned model (DB + Express)         |
| 3    | Frontend: pages for vehicle, services, reminders                |
| 4    | Integrate backend APIs with frontend, test modules              |
| 5    | Dashboard, PDF report, chart, final testing                     |
| 6    | Deployment, prepare demo, write documentation |

---

## ✅ Final Deliverables Checklist

| Item                         | Owner         |
| ---------------------------- | ------------- |
| Vehicle management module    | Student A     |
| Service log module           | Student B     |
| Reminder & dashboard module  | Student C     |
| ER diagram (complete schema) | All           |
| API documentation (Postman)  | All (modular) |
| Final Report + README        | All           |
| Presentation/demo video      | All           |

