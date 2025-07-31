## 💡 Daily Habit & Mood Tracker

**Goal:** Help users build consistent habits, reflect on their mood daily, and identify wellness patterns over time.

---

## 🔑 CORE FEATURES

1. ✅ Add and manage custom daily habits
2. 📆 Mark habits complete for each day
3. 🔥 View current streaks
4. 😀 Choose a daily mood (emoji-based)
5. 📝 Add optional personal notes
6. 📊 View mood and habit trends over time

---

## 🧱 DATABASE DESIGN

### 1. **Habits**

```sql
Habits (
  id UUID PRIMARY KEY,
  user_id UUID,
  name VARCHAR(100),
  description TEXT,
  is_active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

### 2. **DailyHabitStatus**

```sql
DailyHabitStatus (
  id UUID PRIMARY KEY,
  habit_id UUID REFERENCES Habits(id) ON DELETE CASCADE,
  date DATE,
  is_completed BOOLEAN DEFAULT FALSE,
  UNIQUE (habit_id, date)
)
```

---

### 3. **MoodEntries**

```sql
MoodEntries (
  id UUID PRIMARY KEY,
  user_id UUID,
  date DATE UNIQUE,
  mood ENUM('😃', '🙂', '😐', '☹️', '😢'),
  note TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

## 🌐 API ENDPOINTS

### 🔹 Habits

| Method | Endpoint          | Description             |
| ------ | ----------------- | ----------------------- |
| GET    | `/api/habits`     | Get all habits for user |
| POST   | `/api/habits`     | Create a new habit      |
| PUT    | `/api/habits/:id` | Update habit            |
| DELETE | `/api/habits/:id` | Delete habit            |

---

### 🔹 Daily Habit Status

| Method | Endpoint                             | Description                         |
| ------ | ------------------------------------ | ----------------------------------- |
| GET    | `/api/habits/status?date=YYYY-MM-DD` | Get daily completion for all habits |
| POST   | `/api/habits/:id/mark`               | Mark habit complete/incomplete      |
| PUT    | `/api/habits/:id/mark`               | Update daily status                 |

---

### 🔹 Mood Entries

| Method | Endpoint                    | Description                       |
| ------ | --------------------------- | --------------------------------- |
| GET    | `/api/mood?date=YYYY-MM-DD` | Get mood for the day              |
| POST   | `/api/mood`                 | Create or update daily mood entry |

---

## 🔥 HABIT STREAK LOGIC

### Habit Streak Calculation (Frontend or backend job)

* For each habit:

  * Count consecutive `is_completed = TRUE` rows by date descending
  * Reset streak if a date is missing or `FALSE`

---

## 🎨 UI SCREENS

### 1. **Daily Dashboard**

* 📅 Today's date
* ✅ Habit checklist
* 😀 Mood picker (emoji buttons)
* 📝 Notes input (textarea)
* "Save Mood & Notes" button

---

### 2. **Habit Manager**

* Add/Edit/Delete habits
* Toggle habit active/inactive

---

### 3. **Progress Overview**

* 🔥 Habit streak tracker (per habit)
* 📊 Mood calendar (color-coded by emoji)
* Line/bar chart for habit completion %

---

### 4. **History Page**

* View past moods and notes
* Filter by date range or habit

---

## 🧰 OPTIONAL FEATURES

* Notifications (daily reminder to track)
* Weekly or monthly reports (email or downloadable)
* Tag moods with custom labels (e.g., "stress", "energized")
* Gamification: badges, streak awards
* Journal view (mood + notes per day)

---

## ⚙️ TECH STACK SUGGESTION

| Layer    |Technology                                   |
| -------- | -------------------------------------------- |
| Frontend | React + TailwindCSS |
| Backend  | Node.js + Express                |
| Database | MySQL                 |
| Auth     | JWT                       |
| Charts   | Recharts / Chart.js                          |

