# 💸 SplitSmart — Expense Splitter

A production-ready web application for splitting shared expenses across groups. Built with Node.js, Express, MongoDB, and Vanilla JavaScript.

---

## 🚀 Quick Start

### Prerequisites
- [Node.js](https://nodejs.org) v18+
- [MongoDB](https://www.mongodb.com) ([MongoDB Atlas](https://www.mongodb.com/atlas))

### 1. Install Dependencies

```bash
cd backend
npm install
```

### 2. Configure Environment

```bash
cp .env.example .env
```

`.env`:

```env
PORT=5000
NODE_ENV=development
MONGODB_URI=mongodb://localhost:27017/expense_splitter
JWT_SECRET=your_super_secret_key_at_least_32_characters_long
JWT_EXPIRE=7d
CORS_ORIGIN=http://localhost:5000
```

### 3. Seed Sample Data (Optional)

```bash
npm run seed
```

This creates 4 sample users, 2 groups, and 5 expenses for testing.

**Demo Credentials:**
| Email | Password |
|-------|----------|
| alice@example.com | password123 |
| bob@example.com | password123 |
| carol@example.com | password123 |

### 4. Start the Server

```bash
# Development (with auto-reload)
npm run dev

# Production
npm start
```

Open **http://localhost:5000** in your browser.

---

## 📁 Project Structure

```
splitwise-clone/
├── backend/
│   ├── config/
│   │   └── database.js          # MongoDB connection
│   ├── controllers/
│   │   ├── authController.js    # Signup, login, user search
│   │   ├── groupController.js   # Group CRUD, members, dashboard
│   │   └── expenseController.js # Expense add/list/delete
│   ├── middleware/
│   │   ├── auth.js              # JWT protect middleware
│   │   ├── errorHandler.js      # Central error handling
│   │   └── validation.js        # Joi schemas
│   ├── models/
│   │   ├── User.js              # User model + bcrypt + JWT
│   │   ├── Group.js             # Group model
│   │   └── Expense.js           # Expense model with shares
│   ├── routes/
│   │   ├── auth.js              # /api/auth/*
│   │   ├── groups.js            # /api/groups/*
│   │   └── expenses.js          # /api/expenses/*
│   ├── utils/
│   │   ├── balanceCalculator.js # Core balance math
│   │   └── seeder.js            # Sample data seeder
│   ├── server.js                # Express entry point
│   ├── package.json
│   └── .env.example
└── frontend/
    ├── css/
    │   └── style.css            # Complete design system
    ├── js/
    │   ├── api.js               # Fetch wrapper + token mgmt
    │   ├── utils.js             # Toast, Format, Auth helpers
    │   ├── modal.js             # Modal system
    │   └── layout.js            # Shared app layout
    ├── index.html               # Login / Signup
    ├── dashboard.html           # User dashboard
    ├── groups.html              # Groups list
    ├── group.html               # Group detail (expenses, balances, members)
    └── profile.html             # User profile
```

---

## 🔌 API Reference

### Auth
| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | `/api/auth/signup` | Register new user | No |
| POST | `/api/auth/login` | Login | No |
| GET | `/api/auth/me` | Get current user | Yes |
| GET | `/api/auth/search?email=` | Search user by email | Yes |

### Groups
| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/api/groups/dashboard` | Dashboard summary | Yes |
| GET | `/api/groups` | List my groups | Yes |
| POST | `/api/groups` | Create group | Yes |
| GET | `/api/groups/:id` | Get group + balances | Yes |
| PUT | `/api/groups/:id` | Update group | Yes (admin) |
| DELETE | `/api/groups/:id` | Delete group | Yes (admin) |
| POST | `/api/groups/:id/members` | Add member | Yes (admin) |
| DELETE | `/api/groups/:id/members/:userId` | Remove member | Yes (admin) |

### Expenses
| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/api/groups/:groupId/expenses` | List group expenses | Yes |
| POST | `/api/groups/:groupId/expenses` | Add expense | Yes |
| GET | `/api/expenses/:id` | Get single expense | Yes |
| DELETE | `/api/expenses/:id` | Delete expense | Yes |

**Expense Filters** (query params):
- `startDate`, `endDate` — date range
- `userId` — filter by member
- `category` — filter by category
- `page`, `limit` — pagination

---

## 🧮 Balance Algorithm

The app uses a **greedy minimization algorithm** to reduce the number of settlements:

1. Calculate each user's net balance from all expenses
2. Separate into creditors (owed) and debtors (owe)
3. Greedily pair the largest debtor with largest creditor
4. Settle as much as possible in each pairing
5. Continue until all balances are settled

This reduces N*(N-1)/2 possible transactions to at most N-1.

---

## 🔒 Security

- Passwords hashed with bcrypt (12 salt rounds)
- JWT tokens (7-day expiry, configurable)
- Protected routes — all non-auth endpoints require valid JWT
- Input validation with Joi on all endpoints
- MongoDB injection prevention via Mongoose
- CORS configured for specific origins

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|------------|
| Runtime | Node.js |
| Framework | Express.js |
| Database | MongoDB |
| ODM | Mongoose |
| Auth | JWT + bcryptjs |
| Validation | Joi |
| Frontend | Vanilla HTML/CSS/JS |
| Fonts | DM Sans + DM Mono (Google Fonts) |