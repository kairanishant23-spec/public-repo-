# 🏔 HIMSARU — Pure Taste of the Himalayas
### *A Full-Stack E-Commerce Platform for Authentic Pahadi Products*

---

> **Live Website:** [himsaru.vercel.app](https://h-imsaru.vercel.app///.app)  
> **Backend API:** [himsaru-at0n.onrender.com](https://himsaru.onrender.com.com/api/health)  
---

## 📖 Project Overview

**HIMSARU** is a production-ready, full-stack e-commerce platform built to sell authentic organic products sourced directly from the Himalayan mountains of Uttarakhand, India — including A2 Ghee, Wild Honey, Pahadi Salts, Mountain Pulses, Rice, and Spices.

The platform was designed with three core goals:
1. **Empower Himalayan women** — bringing their handcrafted products to a national audience
2. **Production-grade reliability** — capable of handling real orders, payments, and customer data
3. **Zero-dependency frontend** — fully functional using vanilla HTML, CSS, and JavaScript without any framework

---

## ✨ Key Features

### 🛍️ Customer-Facing Store
| Feature | Details |
|---|---|
| **Product Catalogue** | Dynamic product listings fetched from MongoDB, filterable by category (Ghee, Honey, Salt, Pulses, Rice, Spices) |
| **Product Detail Pages** | Full-page product view with image gallery, variant selector (size), benefits, ingredients, how-to-use, and tags |
| **Smart Search** | Real-time search overlay with instant results across product name, category and tags |
| **Shopping Cart** | Persistent cart using localStorage, supports multiple products, sizes, and quantities |
| **Checkout Flow** | 3-step checkout: Delivery Address → Payment Method → Order Confirmation |
| **Multiple Payment Methods** | UPI (with QR code + deep links for GPay/PhonePe/Paytm/BHIM), Razorpay (cards, net banking, EMI), Cash on Delivery |
| **Order History & Tracking** | Authenticated users can see all past orders with a live 6-step visual progress tracker |
| **User Accounts** | Register/Login with email+phone+password, saved delivery addresses, profile management |
| **Responsive Design** | Fully mobile-optimized, works across all screen sizes from 320px to 4K |
| **The Soul Page** | Dedicated standalone page (`/our-soul`) telling the story of the Himalayan women behind HIMSARU |

---

### 🔐 Admin Dashboard (`/admin`)
| Feature | Details |
|---|---|
| **Secure Login** | Dedicated admin-only authentication endpoint, separate from customer login |
| **Auto Session Restore** | JWT token persisted in `localStorage`, auto-verified on page load |
| **Dashboard Stats** | Live metrics: Total Revenue, Total Orders, Active Products, Registered Users, Pending Orders, Unread Messages |
| **Order Management** | View all orders with status/date filter, paginated table, order detail modal with full item breakdown and shipping address |
| **Status Updates** | Update any order status (Placed → Confirmed → Processing → Shipped → Out for Delivery → Delivered → Cancelled) with optional courier notes — triggers customer email/SMS automatically |
| **Product CRUD** | Full Create/Edit/Delete interface for products: name, Hindi name, category, pricing, stock, images, tags, benefits, ingredients, how-to-use, badge |
| **Customer Messages** | View all contact form and distributor inquiry submissions, mark as read |

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND (Vercel)                     │
│                                                              │
│   index.html          — Single Page Application (SPA)        │
│   our-soul.html       — Standalone page (The Soul)           │
│   admin.html          — Admin Dashboard (protected)          │
│   vercel.json         — URL rewrites (/admin, /our-soul)     │
└──────────────────────────┬──────────────────────────────────┘
                           │ HTTPS REST API
┌──────────────────────────▼──────────────────────────────────┐
│                        BACKEND (Render)                      │
│                                                              │
│   Express.js server   — Node.js REST API                     │
│   ├── /api/auth       — Register, Login, Admin Login, Me     │
│   ├── /api/products   — CRUD (Public GET, Admin POST/PUT/DEL)│
│   ├── /api/orders     — Place order, My Orders, Cancel       │
│   ├── /api/payment    — Razorpay create/verify               │
│   ├── /api/contact    — Contact form, Distributor enquiry    │
│   └── /api/admin      — Dashboard, Orders, Contacts (Admin)  │
│                                                              │
│   Middleware          — JWT Auth, Admin-only Guard           │
│   Utils               — Notifications (Email + SMS), Seeder  │
└──────────────────────────┬──────────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────────┐
│                     DATABASE (MongoDB Atlas)                │
│                                                             │
│   Collections:                                              │
│   ├── users           — Accounts, roles, saved addresses    │
│   ├── products        — Full catalogue with variants        │
│   ├── orders          — Orders, status history, payments    │
│   ├── contacts        — Customer messages & distributor leads│      │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

### Frontend
- **HTML5** — Semantic SPA structure
- **Vanilla CSS** — Custom design system with CSS variables, glassmorphism, animations
- **Vanilla JavaScript** — Full SPA router, API client, cart, checkout, auth, SEO injector
- **Google Fonts** — Playfair Display + Inter typography
- **Font Awesome 6** — Icons throughout

### Backend
- **Node.js + Express.js** — REST API server
- **MongoDB + Mongoose** — Database and ODM
- **JWT (jsonwebtoken)** — Stateless authentication
- **bcryptjs** — Password hashing
- **Nodemailer** — Transactional email (SMTP)
- **Razorpay SDK** — Payment gateway integration
- **Helmet + CORS + Rate Limiting** — Security hardening

### Infrastructure
- **Vercel** — Frontend hosting with automatic HTTPS and global CDN
- **Render** — Backend Node.js hosting (auto-sleep free tier)
- **MongoDB Atlas** — Managed cloud database

---

## 📦 Database Schema Summary

### User
```
firstName, lastName, email (unique), phone (unique),
password (bcrypt), role (user|admin), addresses[], createdAtIST
```

### Product
```
name, hindiName, category, description, shortDesc,
ingredients, benefits[], howToUse, price, mrp, unit,
images[], thumbnail, tags[], variants[], badge,
stock, rating, reviewCount, isActive
```

### Order
```
user (ref), orderNumber (unique), items[], address,
paymentMethod (upi|razorpay|cod), status, statusHistory[],
subtotal, shipping, discount, total,
utr, razorpayOrderId, trackingNumber, courier,
estimatedDelivery, createdAtIST
```

### Contact
```
name, email, phone, subject, message,
type (contact|distributor|support),
businessName, city, state, isRead, createdAtIST
```

---

## 🔔 Notification System

The platform has a fully integrated notification pipeline:

- **Order Placed** → Email confirmation + SMS to customer
- **Order Confirmed** → Email + SMS
- **Order Processing** → Email + SMS  
- **Order Shipped** → Email + SMS
- **Out for Delivery** → Email + SMS
- **Order Delivered** → Email + SMS
- **Order Cancelled** → Email + SMS

All notifications are triggered automatically when the admin updates an order status through the dashboard. Falls back to console simulation if SMTP/SMS credentials are not configured.

---

## 🔍 SEO Features

Since HIMSARU is a Single Page Application (SPA), special care was taken for SEO:

- **Static OG Tags** — Open Graph, Twitter Card, canonical, robots meta in `<head>`
- **Dynamic Meta Injection** — `injectProductSEO()` updates `<title>`, description, OG image, OG URL, and Twitter Card every time a product page opens
- **JSON-LD Product Schema** — Full Google-readable Product schema injected dynamically for each product (name, price, availability, seller)
- **SEO Reset** — `resetSEO()` restores site-wide defaults when navigating away from product pages
- **The Soul Page** — Separate HTML page with its own unique title and meta description

---

## 💳 Payment Flow

### UPI
1. Customer selects UPI → QR code + UPI ID displayed
2. Deep-link buttons open GPay / PhonePe / Paytm / BHIM directly
3. Customer completes payment and enters UTR number
4. Order is placed with `paymentMethod: "upi"` and UTR stored

### Razorpay (Cards / Net Banking / EMI)
1. Backend creates a Razorpay order → returns `razorpayOrderId`
2. Razorpay checkout modal opens on frontend
3. On success → `/api/orders/verify-razorpay` validates HMAC signature
4. Order status set to `confirmed` automatically

### Cash on Delivery
1. Order placed immediately with `paymentMethod: "cod"`
2. No payment required upfront

---

## 🌐 URL Structure

| URL | Page |
|---|---|
| `/` | Home (SPA) |
| `/#products` | Product catalogue |
| `/#about` | About HIMSARU |
| `/#dist` | Distributor enquiry |
| `/#contact` | Contact form |
| `/#orders` | My Orders (auth required) |
| `/#product/0` | Product detail (by index) |
| `/our-soul` | The Soul page |
| `/admin` | Admin Dashboard |

---

## 🚀 Deployment

### Frontend (Vercel)
- Root directory: `himsaru-vercel-deploy/`
- Output directory: `public/`
- No build step required
- URL rewrites in `vercel.json` handle `/admin` → `/admin.html` and `/our-soul` → `/our-soul.html`

### Backend (Render)
- Root directory: `backend/src/`
- Start command: `node server.js`
- Node version: ≥18.0.0
- Auto-seeds database on first startup if empty

### Environment Variables (Backend)
```
MONGODB_URI         — MongoDB Atlas connection string
JWT_SECRET          — Secret for signing JWT tokens
RAZORPAY_KEY_ID     — Razorpay public key
RAZORPAY_KEY_SECRET — Razorpay secret key
FRONTEND_URL        — Vercel deployment URL
NODE_ENV            — production
PORT                — 10000
```

---

## 🔑 Admin Access

The seeder automatically creates a default admin account:

| Field | Value |
|---|---|
| Email | `admin@himsaru.com` |
| Password | `admin123` |

To promote any existing user to admin:
```bash
npm run make-admin <user-email>
```

---

## 📸 Website Pages

### 🏠 Home Page
- Full-screen hero carousel (11 slides) with product showcases and the HIMSARU story
- Category quick-nav grid (Ghee, Honey, Salt, Pulses, Rice, Spices)
- Bestseller product strip
- "Why HIMSARU" trust section
- Customer testimonials carousel
- Social proof trust strip
- WhatsApp order banner
- Newsletter signup
- Featured products (horizontal scroll)

### 🌾 Products Page
- Category filter tabs
- Search bar
- Sort options (Newest, Price Low→High, Price High→Low, Rating)
- Product cards with badge, price, MRP, quick-add to cart
- Product detail modal/page with image gallery, variant selection, add to cart

### ❤️ The Soul Page (`/our-soul`)
- Dedicated standalone page
- Hero image of Himalayan women
- Story narrative sections
- Photo grids of artisans
- CTA to shop

### 🛠️ Admin Dashboard (`/admin`)
- Secure login (admin accounts only)
- Live dashboard stats
- Orders table with filtering and pagination
- Order detail modal with status update
- Products table with full CRUD modal
- Customer messages inbox

---

## 🏆 Project Highlights

- **Zero Framework Frontend** — Entire SPA built in pure HTML/CSS/JS (1,285 KB single file) — demonstrates deep JavaScript mastery without relying on React/Vue
- **Production Security** — Helmet CSP headers, JWT auth, bcrypt passwords, rate limiting, input validation with express-validator
- **Indian Market Optimized** — UPI payment deep-links, IST timestamps, Indian number validation, Fast2SMS integration, INR pricing
- **Bilingual Product Names** — All products have English and Hindi names (e.g., "बद्री गाय का शुद्ध देसी घी")
- **Smart Cart Persistence** — Cart survives page refreshes via localStorage and migrates product IDs when backend data changes
- **Auto DB Seeding** — Backend seeds 15 curated products automatically on first launch from the frontend catalogue

---

## 🌿 About the Brand

HIMSARU stands for **HIM**alayan + **SARU** (pure/clean in Pahadi dialect). The brand was created to:

- Connect urban consumers with authentic, unadulterated Himalayan products
- Provide fair income to local farming cooperatives and women artisans
- Preserve traditional processing methods (Bilona ghee, Silbatta salt-grinding, wild honey harvesting)
- Build a sustainable supply chain from mountain farms directly to homes across India

---

*Built for the Himalayas and the people who call them home.*
