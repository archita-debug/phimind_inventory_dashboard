# PhiMind Inventory Management Dashboard

A full-stack web application built for the Inventory Management System. It includes user authentication (Login & Register) and a fully functional Inventory Management Dashboard with role-based access control.

---

## 🔗 Links

- **Frontend (HTML):** `phimind_final.html` — open directly in any browser
- **Backend API:** https://phimind-api.vercel.app
- **Screenshots & Demo Video:** https://drive.google.com/drive/folders/1Vwwi22_mLN2XUjYp47nHtnNmhWcf2rYT?usp=drive_link

---

## ⚙️ Setup Instructions

### Prerequisites

- Node.js v18+
- MongoDB Atlas account
- Vercel account (for deployment)

---

### Backend Setup

1. **Clone / download** the `phimind-api` project and navigate into it:

```bash
cd phimind-api
```

2. **Install dependencies:**

```bash
npm install
```

3. **Create a `.env` file** in the root directory:

```env
MONGODB_URI=mongodb+srv://<username>:<password>@cluster0.xxxxx.mongodb.net/phimind?retryWrites=true&w=majority
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRES_IN=7d
PORT=5000
```

4. **Run locally:**

```bash
npm run dev
```

The API will start at `http://localhost:5000`

5. **Deploy to Vercel:**

```bash
vercel --prod
```

Make sure to add all `.env` variables in **Vercel → Project → Settings → Environment Variables**, then redeploy.

---

### Frontend Setup

No build step required. The frontend is a single HTML file.

1. Open `phimind_final.html` in any modern browser
2. The `API_BASE` is already set to `https://phimind-api.vercel.app`
3. Register an account → Login → use the dashboard

> To point to a local backend, edit line in the `<script>` tag:
> ```js
> const API_BASE = 'http://localhost:5000';
> ```

---

## ✅ Features Implemented

### Authentication

| Feature | Details |
|---|---|
| Register | Email, password (strength rules), role selection (ADMIN / STAFF) |
| Login | JWT token stored in sessionStorage, redirects to dashboard |
| Logout | Clears session, redirects to login |
| Session Persistence | Stays logged in on page refresh |
| Form Validation | Inline errors for all fields |
| Password Strength Meter | Visual 4-bar indicator |
| Loading States | Spinner on buttons during API calls |
| Error Messages | Specific messages for wrong password, server errors, network failures |

### Inventory Dashboard

| Feature | Details |
|---|---|
| Fetch Items | Loads all inventory on page load |
| Stats Bar | Total, In Stock (>10), Low Stock (1–10), Out of Stock (0) |
| Status Badges | Color-coded: green / amber / red |
| Search | Live filter by item name |
| Create Item | ADMIN only — modal with name + quantity |
| Add Stock | ADMIN only — `+ Add Stock` button per item, modal with quantity input |
| Use Stock | ADMIN + STAFF — `− Use Stock` button per item |
| Optimistic UI | Table and stats update instantly without full reload |
| Role Banner | Shows logged-in role and permissions |
| Empty State | Friendly UI when no items exist |
| Error State | Retry button on API failure |

### Role-Based Access Control

| Action | ADMIN | STAFF |
|---|---|---|
| View inventory | ✅ | ✅ |
| Use stock | ✅ | ✅ |
| Add stock | ✅ | ❌ |
| Create items | ✅ | ❌ |

### UX / Edge Cases

- Responsive design (mobile + desktop)
- Toast notifications (success / error / info)
- Modal close on backdrop click or Escape key
- Enter key submits login / register forms
- Buttons disabled during API calls
- 401 response → auto logout
- Insufficient stock → validation error before API call
- Negative quantity → prevented client-side

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Backend | Node.js, Express.js |
| Database | MongoDB Atlas (Mongoose ODM) |
| Auth | JWT (jsonwebtoken + bcryptjs) |
| Validation | express-validator |
| Deployment | Vercel (serverless) |

---

## 📁 Project Structure

```
phimind-api/
├── src/
│   ├── config/
│   │   └── db.js              # MongoDB connection with caching
│   ├── middleware/
│   │   └── auth.js            # JWT protect + role restrict
│   ├── models/
│   │   ├── User.js            # User schema (email, password, role)
│   │   └── Item.js            # Item schema (name, quantity, status virtual)
│   ├── routes/
│   │   ├── auth.js            # POST /auth/register, POST /auth/login
│   │   └── inventory.js       # GET/POST /inventory/items, stock-in, stock-out
│   └── index.js               # Express app entry point
├── vercel.json                # Vercel deployment config
├── package.json
└── .env.example
```

---

## 🔌 API Endpoints

| Method | Endpoint | Auth | Role | Description |
|---|---|---|---|---|
| POST | `/auth/register` | No | — | Register new user |
| POST | `/auth/login` | No | — | Login, returns JWT |
| GET | `/inventory/items` | Yes | Any | Get all items |
| POST | `/inventory/items` | Yes | ADMIN | Create item |
| POST | `/inventory/items/:id/stock-in` | Yes | ADMIN | Add stock |
| POST | `/inventory/items/:id/stock-out` | Yes | Any | Use stock |
| GET | `/` | No | — | Health check |
