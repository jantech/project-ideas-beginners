## 🏥 Clinic Appointment Scheduler

**Goal:** Enable clinics to efficiently schedule and manage doctor appointments while tracking patient and doctor data.

---

## 🎯 CORE FEATURES

### ✅ Must-Haves

* Book and view doctor appointment slots
* See real-time availability
* Create and manage patient records
* Create and manage doctor profiles

### 🧱 CRUD Entities

* **Doctors**
* **Patients**
* **Appointments**

---

## 📊 DATABASE DESIGN (Schema)

### 1. **Doctors**

```sql
Doctors (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  specialization VARCHAR(100),
  email VARCHAR(100),
  phone VARCHAR(20),
  working_hours JSONB, -- e.g., { "Mon": ["09:00-12:00", "14:00-17:00"] }
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

### 2. **Patients**

```sql
Patients (
  id UUID PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100),
  phone VARCHAR(20),
  dob DATE,
  gender VARCHAR(10),
  notes TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

### 3. **Appointments**

```sql
Appointments (
  id UUID PRIMARY KEY,
  doctor_id UUID REFERENCES Doctors(id) ON DELETE CASCADE,
  patient_id UUID REFERENCES Patients(id) ON DELETE CASCADE,
  appointment_date DATE,
  start_time TIME,
  end_time TIME,
  reason TEXT,
  status ENUM('Scheduled', 'Completed', 'Canceled') DEFAULT 'Scheduled',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

## 🌐 API ENDPOINTS

### 🔹 Doctors

| Method | Endpoint           | Description        |
| ------ | ------------------ | ------------------ |
| GET    | `/api/doctors`     | List all doctors   |
| GET    | `/api/doctors/:id` | Get doctor profile |
| POST   | `/api/doctors`     | Add new doctor     |
| PUT    | `/api/doctors/:id` | Update doctor      |
| DELETE | `/api/doctors/:id` | Delete doctor      |

---

### 🔹 Patients

| Method | Endpoint            | Description         |
| ------ | ------------------- | ------------------- |
| GET    | `/api/patients`     | List all patients   |
| GET    | `/api/patients/:id` | Get patient profile |
| POST   | `/api/patients`     | Add new patient     |
| PUT    | `/api/patients/:id` | Update patient      |
| DELETE | `/api/patients/:id` | Delete patient      |

---

### 🔹 Appointments

| Method | Endpoint                | Description               |
| ------ | ----------------------- | ------------------------- |
| GET    | `/api/appointments`     | List all appointments     |
| GET    | `/api/appointments/:id` | Get appointment details   |
| POST   | `/api/appointments`     | Schedule new appointment  |
| PUT    | `/api/appointments/:id` | Update appointment        |
| DELETE | `/api/appointments/:id` | Cancel/delete appointment |

### 🔹 Availability

| Method | Endpoint                                        | Description                            |
| ------ | ----------------------------------------------- | -------------------------------------- |
| GET    | `/api/doctors/:id/availability?date=YYYY-MM-DD` | View available slots on a specific day |

---

## 🎨 UI SCREENS

### 1. **Login / Register (for Staff)**

* Clinic staff login to manage appointments
* Optional: Admin role to manage doctors

---

### 2. **Dashboard**

* Summary cards:

  * 📅 Today's Appointments
  * 👨‍⚕️ Active Doctors
  * 👥 Registered Patients
* Quick Links: Add Doctor, Add Patient, Schedule Appointment

---

### 3. **Doctors Page**

* Table/List view with:

  * Name, Specialization
  * Weekly Schedule (clickable)
  * Actions: View, Edit, Delete
* “Add Doctor” button

---

### 4. **Patients Page**

* Searchable list of patients
* Columns: Name, Phone, Email, DOB
* Actions: View, Edit, Delete
* “Add Patient” button

---

### 5. **Appointments Page**

* Calendar View (Week/Day toggle)
* Filters: Doctor, Status, Date Range
* “+ Schedule Appointment” button

---

### 6. **Book Appointment Form**

* Select Doctor (dropdown)
* Select Patient (autocomplete or new)
* Pick Date
* View Available Slots (real-time)
* Enter Reason
* Confirm button

---

### 7. **Doctor Availability View**

* Week view of working hours and booked slots
* Greyed-out unavailable slots
* Green for available, red for booked

---

### 8. **Appointment Detail View**

* Show:

  * Patient info
  * Doctor info
  * Date & Time
  * Status (with update option)
* Actions: Mark as Completed, Cancel, Reschedule

---

## ⚙️ TECH STACK RECOMMENDATION

| Layer         | Technology                       |
| ------------- | -------------------------------- |
| Frontend      | React + TailwindCSS              |
| Backend       | Node.js + Express / Fastify      |
| Database      | MySQL                            |
| Auth          | JWT             |
| Calendar      | FullCalendar.js                  |
| Notifications | Nodemailer (Email)               |

---