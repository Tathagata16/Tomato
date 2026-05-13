# Tomato / FoodHub — Full-Stack Food Ordering Platform

Tomato (branded as **FoodHub** in the UI) is a full-stack food ordering system with:
- a **customer web app** for browsing menu items, searching, cart management, and checkout,
- an **admin dashboard** for catalog and order operations,
- a **Node.js/Express API** backed by MongoDB,
- **real-time order events** using Socket.IO,
- **Razorpay payment integration**,
- and **WhatsApp notifications** for order lifecycle updates.

---

## Project Structure

```text
.
├── frontend/   # Customer-facing React app
├── admin/      # Admin panel React app
└── backend/    # Express + MongoDB API
```

---

## Key Features

### Customer App (`frontend`)
- User registration and login with JWT-based backend auth.
- Browse food catalog and search by query (`/api/food/search`).
- Add/remove cart items with persistent server-side cart state.
- Place orders with address details and online payment via Razorpay.
- View personal orders and track status updates.

### Admin App (`admin`)
- Admin-only route protection (client-side guarded).
- Add, edit, delete, hide/unhide menu items.
- View paid orders and update status (Food Processing → Out for Delivery → Delivered).
- Real-time order feed updates via Socket.IO events (`newOrder`, `orderStatusUpdate`).

### Backend (`backend`)
- REST APIs for users, food, cart, and orders.
- MongoDB models for users, food items, and orders.
- Cloudinary-based image upload handling through Multer memory storage.
- Razorpay order creation + payment signature verification.
- WhatsApp automation integration for customer/admin notifications.

---

## Tech Stack

### Frontend / Admin
- React
- Vite
- React Router
- Axios
- React Toastify
- Socket.IO Client

### Backend
- Node.js + Express
- MongoDB + Mongoose
- JWT + bcrypt
- Multer + Cloudinary
- Razorpay
- Socket.IO
- whatsapp-web.js

---

## API Overview

Base URL (local): `http://localhost:4000`

### User
- `POST /api/user/register`
- `POST /api/user/login`

### Food
- `GET /api/food/list`
- `GET /api/food/search?q=<term>`
- `POST /api/food/add`
- `PUT /api/food/update/:id`
- `PUT /api/food/toggle/:id`
- `DELETE /api/food/remove/:id`

### Cart (auth required)
- `POST /api/cart/add`
- `POST /api/cart/remove`
- `POST /api/cart/get`

### Orders
- `POST /api/order/place` (auth required)
- `POST /api/order/verify`
- `POST /api/order/userorders` (auth required)
- `POST /api/order/status`
- `GET /api/order/list`

---

## Environment Variables

Create a `.env` inside `backend/` with at least:

```env
PORT=4000
JWT_SECRET=your_jwt_secret
FRONTEND_URL=http://localhost:5173
FRONTEND_ADMIN_URL=http://localhost:5174

RAZORPAY_KEY_ID=your_razorpay_key
RAZORPAY_KEY_SECRET=your_razorpay_secret

CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_key
CLOUDINARY_API_SECRET=your_cloudinary_secret

ADMIN_WHATSAPP=919XXXXXXXXX
NODE_ENV=development
```

> Note: Database connection is currently hardcoded in `backend/config/db.js`. For production readiness, move this URI to `MONGO_URL` in `.env`.

---

## Local Development

### 1) Backend
```bash
cd backend
npm install
npm run server
```

### 2) Admin
```bash
cd admin
npm install
npm run dev
```

### 3) Frontend
The `frontend/` app source exists and is complete, but this repo snapshot currently does **not** include a `frontend/package.json` file. Add/restore it before running dependency installation and Vite commands.

---

## Real-Time Events

Socket.IO is initialized in the backend and consumed by admin/client apps.

Emitted events:
- `newOrder` — emitted after successful payment verification.
- `orderStatusUpdate` — emitted when admin changes order status.

---

## Security & Production Notes

- Current admin login uses static client-side credentials for dashboard access (should be migrated to secure backend role-based auth).
- Cart `remove` handler currently reads `req.body.userId` while route applies auth middleware—this should be aligned to `req.userId`.
- CORS is currently permissive (`origin: true`) in backend middleware.
- Remove debug logs and hardcoded secrets/URIs before production deployment.

---

## Deployment

`frontend/` and `admin/` both include `vercel.json`, indicating Vercel-friendly static frontend deployments. Backend can be deployed to any Node host with environment variable support and persistent MongoDB access.

---

## Resume-Ready Highlights

See the 3 resume bullet points in the response accompanying this README update.
