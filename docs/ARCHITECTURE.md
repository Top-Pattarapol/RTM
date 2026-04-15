# System Architecture

## Overview

ReadyToMeow consists of two primary repositories:
- **meow-sheet-shop**: Frontend/client application (React + Vite + TypeScript)
- **meow-sheet-shop-be**: Backend/server application (Express + Node.js)

The system follows a standard client-server architecture with a Google Sheets backend.

## Components

| Component | Responsibility | Technology | Repository |
|-----------|---------------|------------|------------|
| Frontend | User interface, client-side logic, API integration | React 18, Vite 7, TanStack Query v5, React Router v6, Radix UI, TailwindCSS | `meow-sheet-shop` |
| Backend | REST API endpoints, business logic, Google Sheets integration, authentication | Express 4, Node.js ES Modules | `meow-sheet-shop-be` |

## Frontend Architecture (`meow-sheet-shop`)

### Directory Structure

```
frontend/src/
├── components/
│   ├── ui/           # 57 reusable Radix-based UI components (shadcn/ui pattern)
│   ├── admin/        # Admin-specific components
│   ├── catalog/      # Product catalog components
│   ├── layout/       # Header, Footer
│   └── ProtectedRoute.tsx, ApiTestComponent.tsx
├── contexts/
│   └── AuthContext.tsx   # Unified auth for users and admins
├── hooks/
│   ├── use-cart.ts, use-cart-v2.ts  # Dual cart implementations (tech debt)
│   ├── use-mobile.tsx
│   └── use-toast.ts
├── lib/
│   ├── api.ts           # Centralized API client
│   ├── admin-api.ts     # Admin-specific API calls
│   ├── sheets.ts        # Direct Google Sheets client (for frontend reads)
│   └── utils.ts, storage.ts, promptpay.ts, order-id.ts, save-qr-code.ts
├── pages/
│   ├── customer: Access, Index, ProductDetail, Cart, Checkout, OrderConfirmation, Track, Contact, NotFound
│   └── admin: AdminLogin, AdminDashboard, AdminProducts, AdminProductNew, AdminProductEdit, AdminOrders, AdminOrderDetail, AdminAccounts
├── types/
│   └── index.ts, variant.ts
├── App.tsx              # Route definitions with ProtectedRoute and AdminProtectedRoute wrappers
└── main.tsx
```

### Key Architectural Decisions

- **SPA with client-side routing**: No SSR, React Router v6 for navigation
- **Server state**: TanStack Query v5 manages API data fetching, caching (60s-300s), stale-while-revalidate
- **Auth context**: `AuthContext` manages both user and admin authentication via JWT stored in localStorage
- **API layer**: `lib/api.ts` centralizes all backend communication; admin uses separate `lib/admin-api.ts`
- **UI components**: 57 headless Radix UI components styled with TailwindCSS (shadcn/ui patterns)

### Frontend Code Metrics

| Metric | Value |
|--------|-------|
| UI Components | 57 |
| Customer Pages | 9 |
| Admin Pages | 7 |
| Total Page Lines | ~5,100 LOC |
| Largest Files | Access.tsx (415), Checkout.tsx (647), ProductDetail.tsx (485), AdminOrderDetail.tsx (530) |

## Backend Architecture (`meow-sheet-shop-be`)

### Directory Structure

```
backend/src/
├── config/
│   └── swagger.js        # Swagger/OpenAPI documentation configuration
├── lib/
│   ├── sheets.js         # Google Sheets API integration (~1100 lines - needs refactor)
│   ├── notification.js   # Firebase Cloud Messaging notifications
│   ├── storage.js        # File storage utilities
│   ├── promptpay.js      # PromptPay QR code generation
│   ├── shipping.js       # Shipping fee calculations
│   ├── order-id.js       # Order ID generation
│   ├── validation.js     # Zod schemas for request validation
│   ├── logger.js         # Morgan-based request logging
│   └── utils.js          # Shared utilities
├── middleware/
│   ├── admin.js          # Admin authentication middleware
│   └── access.js         # User authentication middleware
├── routes/
│   ├── access.js         # User login/register (~440 lines)
│   ├── admin-accounts.js # Admin account management (~540 lines)
│   ├── admin.js          # Admin CRUD operations (~1070 lines - needs refactor)
│   ├── cart.js           # Cart operations (~250 lines)
│   ├── order.js          # Order processing (~990 lines - needs refactor)
│   └── products.js       # Product catalog (~150 lines)
├── scheduler/
│   └── expire-orders.js  # Cron job for order expiration
├── tests/                # Integration tests (not runnable without node_modules)
│   ├── user-authentication.test.js
│   ├── admin-accounts.test.js
│   ├── registration.test.js
│   ├── unified-auth.test.js
│   ├── order-id.test.js
│   ├── promptpay.test.js
│   ├── notification.test.js
│   ├── scheduler.test.js
│   └── track-validation.test.js
└── index.js              # Express app entry with middleware, routes, graceful shutdown
```

### Backend Code Metrics

| Metric | Value |
|--------|-------|
| Route Handlers | 6 routes |
| Lib Modules | 9 modules |
| Middleware | 2 |
| Test Files | 9 |
| Total Route/Lib Lines | ~5,800 LOC |
| Largest Files | sheets.js (1103), admin.js (1070), order.js (987) |

### API Endpoints

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/api/products` | GET | Public | List all active products |
| `/api/product/:id` | GET | Public | Get product details |
| `/api/cart/price` | POST | User | Calculate cart total |
| `/api/order` | POST | User | Create order with PromptPay QR |
| `/api/order/:id/slip` | POST | User | Upload payment receipt |
| `/api/access/login` | POST | Public | User login |
| `/api/access/register` | POST | Public | User registration |
| `/api/admin/products` | CRUD | Admin | Manage products |
| `/api/admin/orders` | CRUD | Admin | Manage orders |
| `/api/admin/accounts` | CRUD | Admin | Manage admin accounts |

### Security Features

- **Helmet.js**: Security headers middleware
- **Rate limiting**: Auth (20/15min), Admin (300/15min), General (1000/15min)
- **CORS**: Configurable with wildcard support for preview deployments
- **JWT**: Firebase Admin-verified tokens for authentication
- **Input validation**: Zod schemas at API boundary

## Data Flow

```
┌─────────────┐     HTTPS      ┌─────────────┐     Google Sheets API    ┌────────────┐
│   Browser   │ ◄─────────────►│   Backend   │ ◄──────────────────────►│  Sheets DB │
│  (React SPA)│               │ (Express)   │                          │            │
└─────────────┘                └─────────────┘                          └────────────┘
     │                              │
     │ JWT Auth                     │ Firebase Admin
     ▼                              ▼
┌─────────────┐                ┌─────────────┐
│  Firebase   │                │   Admin    │
│  Auth       │                │  Dashboard │
└─────────────┘                └─────────────┘
```

## Deployment Model

- **Frontend**: Static hosting (Vercel/Netlify), Docker-compatible via Dockerfile
- **Backend**: Node.js host (Koyeb configured in index.js), Docker with multi-stage build
- **Database**: Google Sheets via googleapis (service account auth)
- **Auth**: Firebase Admin SDK for token verification

## Key Decisions

| Decision | Context | Rationale |
|----------|---------|-----------|
| Two-repo structure | Separate frontend and backend | Separation of concerns, independent deployment |
| Google Sheets as DB | Budget e-commerce for non-tech client | Zero-cost, easy non-technical updates |
| SPA + separate backend | E-commerce with minimal SEO needs | Simple architecture, fast iteration |
| ES Modules on backend | Node.js 18+ production | Modern Node.js default, cleaner imports |

## Tech Debt & Recommendations

### High Priority

| Issue | Location | Recommendation |
|-------|----------|----------------|
| Large route files | `admin.js` (1070 LOC), `order.js` (987 LOC) | Split into multiple route files by resource |
| Large lib file | `sheets.js` (1103 LOC) | Extract product-related functions from sheet operations |
| Missing frontend tests | No test framework | Add Vitest for component testing |
| Dual cart implementations | `use-cart.ts` and `use-cart-v2.ts` | Consolidate into single implementation |

### Medium Priority

| Issue | Location | Recommendation |
|-------|----------|----------------|
| Large page files | `Access.tsx` (415 LOC), `Checkout.tsx` (647 LOC) | Extract form logic into custom hooks |
| Backend tests non-runnable | `bcrypt` not installed in test environment | Add test dependencies to package.json |
| Admin login page | `AdminLogin.tsx` (14 LOC) | Verify this is intentional minimal design |

### Low Priority

| Issue | Location | Recommendation |
|-------|----------|----------------|
| Inconsistent error handling | Some routes throw raw errors | Standardize error wrapper usage |
| No API versioning | All endpoints under `/api/` | Consider `/api/v1/` prefix for future compatibility |

## Test Coverage

### Frontend (`meow-sheet-shop`)

| Category | Status | Notes |
|----------|--------|-------|
| Unit tests | **Not configured** | No Vitest/Jest setup |
| Component tests | **Not configured** | - |
| E2E tests | **Not configured** | - |
| Integration | Partial | TanStack Query provides caching, but no formal tests |

### Backend (`meow-sheet-shop-be`)

| Category | Status | Notes |
|----------|--------|-------|
| Unit tests | **Present but not runnable** | 9 test files, missing `bcrypt` native module |
| Integration tests | **Unknown** | Cannot verify without dependencies |
| Test framework | Manual | No formal test runner configured |

**Action item**: Install backend test dependencies (`npm install` in backend) and configure a test runner (Jest or Node's built-in test runner).

---
_Documented during RTM-19 (audit). Update as tech debt is addressed._