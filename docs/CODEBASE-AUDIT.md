# RTM-19: Codebase Audit

**Date:** 2026-04-14
**Status:** Completed
**Auditor:** Engineer Agent

---

## Executive Summary

The RTM project consists of two git submodules:
- **frontend/** (`meow-sheet-shop`) - React + TypeScript SPA
- **backend/** (`meow-sheet-shop-be`) - Express.js REST API

Both are on `ai/main` branches. The codebase is functional but has significant tech debt in code organization, type safety, and test coverage. Major route/lib files exceed 1000 LOC and need refactoring.

---

## Frontend Audit (`meow-sheet-shop`)

### Technology Stack
- React 18.3.1, TypeScript 5.8.3
- Vite 7.1.12, TailwindCSS 3.4.19
- TanStack Query v5, React Router v6
- Radix UI (57 headless components), shadcn/ui patterns
- Zod 3.25.76, React Hook Form 7.61.1
- Firebase Admin 13.5, Google APIs 160.0

### Directory Structure
```
frontend/src/
├── components/        # 57 UI components + admin/catalog/layout
├── contexts/          # AuthContext.tsx (unified user/admin auth)
├── hooks/             # use-cart.ts, use-cart-v2.ts (DUAL!), use-mobile.tsx
├── lib/               # api.ts, admin-api.ts, sheets.ts, utils.ts, etc.
├── pages/             # 9 customer pages, 7 admin pages
├── types/             # index.ts, variant.ts
├── App.tsx            # Route definitions
└── main.tsx           # Entry point
```

### Code Metrics
| Metric | Value |
|--------|-------|
| Total source files | ~90 |
| UI components | 57 |
| Page files | 16 |
| LOC (pages only) | ~5,100 |
| Largest pages | Checkout.tsx (647), AdminOrderDetail.tsx (530), ProductDetail.tsx (485), Access.tsx (415) |

### Tech Debt Hotspots

#### 1. Duplicate Type Definitions (HIGH)
**Files:** `src/types/index.ts` AND `src/lib/api.ts`

Both files define `Product`, `CartItem`, `Order`, `BuyerInfo`. The `lib/api.ts` definitions are used by the API client while `types/index.ts` is used elsewhere. This creates maintenance nightmares when types diverge.

```typescript
// types/index.ts defines:
export interface Product { id, name, price, stock, category, image_url, is_active }
export interface CartItem { variant_id, product_id, product_name, variant_name, quantity, unit_price, ... }

// lib/api.ts defines its own (different):
export interface Product { id, name, price, stock, category, image_url, is_active }
export interface CartItem { productId, name, price, quantity, image_url }  // different shape!
```

**Recommendation:** Consolidate to single type definition in `types/index.ts`, update `lib/api.ts` to import from there.

#### 2. Dual Cart Implementations (HIGH)
**Files:** `src/hooks/use-cart.ts` AND `src/hooks/use-cart-v2.ts`

Both implement shopping cart functionality. `use-cart-v2.ts` appears to be a newer version but the old one is still present and imported by some components.

**Recommendation:** Remove `use-cart.ts`, consolidate to `use-cart-v2.ts`, audit all imports.

#### 3. Large Page Components (MEDIUM)
**Files:** Checkout.tsx (647 LOC), AdminOrderDetail.tsx (530 LOC), ProductDetail.tsx (485 LOC)

These pages have grown organically and mix:
- State management
- API calls
- Form validation
- Business logic
- UI rendering

**Recommendation:** Extract form logic into custom hooks, API calls into dedicated functions, split by feature if >300 LOC.

#### 4. Missing Type Safety in API Client (MEDIUM)
**File:** `src/lib/api.ts`

The `getOrder` method uses `any` type and manual response transformation:
```typescript
// Line 78: uses any
const response = await this.request<any>(`/order/${orderId}`);

// Lines 81-107: manual transformation instead of proper typing
```

**Recommendation:** Define proper response types, use a schema validator (Zod) for runtime type safety.

#### 5. No Test Framework (CRITICAL)
**Status:** No Vitest, Jest, or any testing library configured.

**Recommendation:** Add Vitest (aligns with Vite), configure component tests for critical flows (cart, checkout).

---

## Backend Audit (`meow-sheet-shop-be`)

### Technology Stack
- Node.js (ES Modules, `type: "module"`)
- Express 4.21.2, no build step
- Google Sheets API (sheets-as-database)
- Firebase Admin 13.5, JWT, bcrypt
- Zod 3.25.76 for validation
- Swagger-jsdoc + swagger-ui-express
- Helmet 7.2.0, cors, express-rate-limit

### Directory Structure
```
backend/src/
├── config/           # swagger.js
├── lib/              # sheets.js (1103 LOC), validation.js, promptpay.js, etc.
├── middleware/        # access.js, admin.js
├── routes/           # access.js (440), admin.js (1070), order.js (987), ...
├── scheduler/        # expire-orders.js (cron job)
├── tests/            # 9 manual integration test files
└── index.js          # Express app entry
```

### Code Metrics
| Metric | Value |
|--------|-------|
| Route handlers | 6 routes |
| Lib modules | 9 |
| Middleware | 2 |
| Largest files | sheets.js (1103 LOC), admin.js (1070 LOC), order.js (987 LOC) |
| Total route/lib LOC | ~5,800 |

### Tech Debt Hotspots

#### 1. Massive Files Exceeding 1000 LOC (CRITICAL)
**Files:**
- `src/lib/sheets.js` (1103 LOC) - Google Sheets API wrapper
- `src/routes/admin.js` (1070 LOC) - Admin CRUD operations
- `src/routes/order.js` (987 LOC) - Order processing

These files are unmaintainable. They mix concerns (e.g., sheets.js handles products, orders, AND admin operations).

**Recommendation:** Split by resource:
- `lib/sheets/` → `products.js`, `orders.js`, `accounts.js`
- `routes/admin.js` → `admin-products.js`, `admin-orders.js`, `admin-accounts.js`

#### 2. Manual Integration Tests Not Runnable (HIGH)
**Files:** `tests/*.test.js` (9 files)

Tests use `console.log` for output and require running API server. They are not:
- Automated
- Part of CI/CD
- Runnable with `npm test`

```javascript
// From tests/user-authentication.test.js
async function testValidUserAuthentication() {
  const response = await fetch(`${API_BASE_URL}/api/access/verify`, {...});
  // console.log results - no assertions
}
```

**Recommendation:** Replace with Jest + supertest for automated unit/integration tests.

#### 3. No TypeScript (MEDIUM)
Backend uses plain JavaScript with ES Modules. While Zod provides runtime validation, there's no compile-time type checking.

**Recommendation:** Consider migrating to TypeScript or at minimum JSDoc type annotations.

#### 4. Inconsistent Error Handling (MEDIUM)
Some routes throw raw errors; others use custom error wrappers. No consistent error response format.

**Recommendation:** Create `AppError` class, enforce consistent error handling middleware.

#### 5. No API Versioning (LOW)
All endpoints under `/api/`. Future versions would require breaking existing clients.

**Recommendation:** Consider `/api/v1/` prefix for new endpoints.

---

## Security Analysis

### Frontend
| Issue | Severity | Notes |
|-------|----------|-------|
| JWT stored in localStorage | MEDIUM | Vulnerable to XSS. Consider httpOnly cookies. |
| No CSRF protection | MEDIUM | For state-changing operations |
| Client-side env vars exposed | LOW | VITE_* vars visible in browser |

### Backend
| Issue | Severity | Notes |
|-------|----------|-------|
| Rate limiting enabled (prod) | GOOD | Auth: 20/15min, Admin: 300/15min |
| Helmet security headers | GOOD | Enabled |
| CORS configured | GOOD | With wildcard support for preview |
| Input validation (Zod) | GOOD | At API boundary |
| No security audit in CI | HIGH | Recommend adding npm audit to CI |

---

## Test Coverage Summary

| Component | Unit | Integration | E2E | Notes |
|-----------|------|-------------|-----|-------|
| Frontend | NONE | NONE | NONE | No test framework |
| Backend | MANUAL | MANUAL | N/A | Console-based, not automated |

### Frontend Test Gaps
- Cart operations (add, update, remove, calculate)
- Checkout flow (form validation, order creation)
- Authentication context (login, logout, token refresh)
- API client error handling

### Backend Test Gaps
- All route handlers
- Zod validation schemas
- Sheets API integration
- Order ID generation
- PromptPay QR generation

---

## Follow-up Issues to Create

Based on this audit, the following cleanup tasks should be created:

1. **[HIGH] Refactor backend route files** - Split admin.js (1070 LOC), order.js (987 LOC) into resource-specific files
2. **[HIGH] Consolidate frontend types** - Remove duplicate Product/CartItem/Order definitions
3. **[HIGH] Remove dual cart hooks** - Delete use-cart.ts, keep use-cart-v2.ts
4. **[MEDIUM] Add Vitest to frontend** - Configure unit/component testing
5. **[MEDIUM] Migrate backend tests to Jest** - Convert manual tests to automated
6. **[MEDIUM] Split sheets.js lib** - Extract product/order/account operations
7. **[LOW] Add security audit to CI** - Run npm audit in GitHub Actions
8. **[LOW] Extract page logic into hooks** - Checkout.tsx (647 LOC), etc.

---

## Recommendations Priority Matrix

| Priority | Frontend | Backend |
|----------|----------|---------|
| CRITICAL | Add test framework | Make tests automated |
| CRITICAL | Consolidate types | Split large files |
| HIGH | Remove dual cart | Migrate tests to Jest |
| MEDIUM | Extract page logic | Split sheets.js |
| LOW | Add E2E tests | Add API versioning |

---

## Branch Constraint Reminder

All development must occur on **`ai/` prefixed branches only**. Current work is on `ai/main`. Submodules (frontend, backend) are also on their respective `ai/main` branches.

---

_Document compiled from codebase analysis. Update as tech debt is addressed._
