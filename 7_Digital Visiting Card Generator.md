## 📇 Digital Visiting Card Generator

**Goal:** Let individuals create and update online visiting cards with a personal link for easy sharing.

---

## 🔑 CORE FEATURES

1. **Create a digital card**

   * Add name, job title, phone, email, social links
2. **Generate a unique shareable link**

   * Example: `visit.me/username`
3. **Edit/update profile anytime**
4. **Mobile-friendly card view**
5. **Public/private mode (optional)**

---

## 🧱 DATABASE DESIGN

### 1. **Profiles**

```sql
Profiles (
  id UUID PRIMARY KEY,
  username VARCHAR(30) UNIQUE,
  full_name VARCHAR(100),
  job_title VARCHAR(100),
  phone VARCHAR(20),
  email VARCHAR(100),
  bio TEXT,
  profile_picture_url TEXT,
  is_public BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

### 2. **Links**

```sql
Links (
  id UUID PRIMARY KEY,
  profile_id UUID REFERENCES Profiles(id) ON DELETE CASCADE,
  platform ENUM('LinkedIn', 'Twitter', 'GitHub', 'Instagram', 'Website', 'Other'),
  label VARCHAR(50),
  url TEXT,
  icon TEXT, -- Optional: icon name/class
  order_index INT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

---

## 🌐 API ENDPOINTS

### 🔹 Profiles

| Method | Endpoint                  | Description              |
| ------ | ------------------------- | ------------------------ |
| GET    | `/api/profiles/:username` | View profile by username |
| POST   | `/api/profiles`           | Create new profile       |
| PUT    | `/api/profiles/:id`       | Update profile info      |
| DELETE | `/api/profiles/:id`       | Delete profile           |

---

### 🔹 Links

| Method | Endpoint                  | Description                     |
| ------ | ------------------------- | ------------------------------- |
| GET    | `/api/profiles/:id/links` | Get all links for profile       |
| POST   | `/api/profiles/:id/links` | Add new social or personal link |
| PUT    | `/api/links/:id`          | Update a specific link          |
| DELETE | `/api/links/:id`          | Delete a link                   |

---

## 🎨 UI SCREENS

### 1. **Landing Page**

* Welcome message: "Create your Digital Visiting Card"
* CTA: "Create My Card"
* Showcase sample cards (carousel or cards)

---

### 2. **Create/Edit Profile Page**

Form Fields:

* Full Name
* Job Title
* Phone Number
* Email
* Bio (short description)
* Profile Picture Upload
* Username (used for URL, like `visit.me/johndoe`)
* Toggle: Make Profile Public/Private

---

### 3. **Link Manager Page**

* List of social/media links
* Fields:

  * Platform (dropdown or custom)
  * Label (e.g., "My GitHub")
  * URL
  * Order (drag-and-drop or number)
* Button: "Add New Link"

---

### 4. **Profile Preview Page**

* URL: `/[username]`
* Shows:

  * Avatar
  * Name, Title
  * Contact details (click-to-call/email)
  * Social/media links (icons/buttons)
  * Shareable button ("Copy Link")

---

### 5. **Settings / Admin Panel**

* Change password (if account-based)
* Delete profile
* Export profile as QR or vCard (optional)

---

## 📦 OPTIONAL FEATURES

* **QR Code Generator** for each visiting card
* **vCard (.vcf) download**
* **Analytics** (views/clicks per link)
* **Dark/Light mode toggle**
* **Templates/themes** for profile layout
* **Password-protected card** (for private sharing)
* **Custom domain/subdomain support**

---

## ⚙️ TECH STACK SUGGESTION

| Layer          | Tech Stack                                              |
| -------------- | ------------------------------------------------------- |
| Frontend       | React + Tailwind CSS                                    |
| Backend        | Node.js + Express                             |
| Database       | MySQL            |
| Authentication | Firebase Auth / Clerk                                   |
| Optional       | QR Code: `qrcode` NPM package                           |

