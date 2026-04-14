# ReadyToMeow Admin UI Design Specification

**Revision:** 1.0 • **Date:** 2026-04-14 • **Owner:** UI Designer • **Scope:** UI design for Admin pages

---

## Overview

This document specifies the visual design and interaction patterns for the ReadyToMeow Admin panel. All designs follow the established [Design System](./DESIGN-SYSTEM.md) and [Admin PRD](../frontend/docs/ADMIN_PRD.md).

**Design Philosophy:**
- Clean, professional admin interface that feels warm and trustworthy
- Efficient workflows — admin actions should be quick and obvious
- Clear hierarchy — status, actions, and data are instantly readable
- Consistent with brand personality (warm, playful, confident, trustworthy)

---

## Design Tokens for Admin

Admin panel uses the same design system tokens with additional admin-specific considerations:

### Admin Color Additions

| Token | Hex | Usage |
|-------|-----|-------|
| `admin-sidebar-bg` | `neutral-50` | Sidebar background |
| `admin-sidebar-active` | `primary-50` | Active sidebar item background |
| `admin-sidebar-border` | `neutral-200` | Sidebar right border |
| `admin-topnav-bg` | `neutral-0` | Top navigation background |
| `admin-content-bg` | `neutral-50` | Main content area background |

### Admin Typography

Same as design system. Admin headings use `h3` (22px) for page titles, `h4` (18px) for section titles within pages.

### Admin Spacing

| Token | Value | Pixels | Usage |
|-------|-------|--------|-------|
| `admin-sidebar-width` | — | 240px | Fixed sidebar width |
| `admin-topnav-height` | — | 64px | Top navigation height |
| `admin-content-padding` | `space-6` | 24px | Page content padding |
| `admin-card-gap` | `space-5` | 20px | Gap between cards in grid |

---

## Layout Structure

### Admin Shell

```
┌─────────────────────────────────────────────────────────────┐
│  Top Nav (64px)  [Logo]              [Admin] [Logout]      │
├──────────────┬──────────────────────────────────────────────┤
│              │                                              │
│   Sidebar    │           Main Content Area                  │
│   (240px)    │           (scrollable)                      │
│              │                                              │
│  - Dashboard │                                              │
│  - Products   │                                              │
│  - Orders     │                                              │
│  - Settings   │                                              │
│              │                                              │
└──────────────┴──────────────────────────────────────────────┘
```

### Responsive Behavior

| Breakpoint | Sidebar | Content |
|------------|---------|---------|
| `lg+` (1024px+) | Fixed 240px | Fluid |
| `md` (768–1023px) | Collapsible overlay | Fluid |
| `sm` (< 768px) | Hidden, hamburger menu | Full width |

**Mobile sidebar:** Slides in from left as overlay with backdrop, 280px width, touch-friendly 48px menu items.

---

## Page Designs

### 1. `/admin/login` — Admin Login

**Purpose:** Secure entry point for admin access via `admin_access_code`.

**Layout:** Centered card on neutral-50 background.

**Visual Design:**

| Element | Specification |
|---------|---------------|
| Container | Centered, max-width 400px, card with `neutral-0` background, `border-radius: 16px`, `shadow: 0 4px 20px rgba(0,0,0,0.1)` |
| Logo/Title | "ReadyToMeow Admin" in `h2`, `neutral-800`, centered |
| Cat Icon | Lucide `cat` icon, 48px, `primary-500`, above title |
| Input Field | Full width, height 48px, `border-radius: 8px`, placeholder "Enter admin access code" |
| Submit Button | Full width, `primary` variant, `lg` size (48px height), text "เข้าสู่ระบบ" |
| Error Message | `error-50` background, `error-600` text, `border-radius: 8px`, appears below input |
| Footer Text | `caption`, `neutral-400`, "Protected by admin authentication" |

**States:**
- Default: Clean form
- Loading: Button shows spinner + "กำลังเข้าสู่ระบบ...", input disabled
- Error: Error message appears, input border changes to `error-500`

**Accessibility:**
- Input has `aria-label="Admin access code"`
- Button has `aria-busy` during loading
- Error message has `role="alert"`

---

### 2. `/admin` — Dashboard

**Purpose:** At-a-glance summary of store metrics.

**Layout:** Grid of summary cards + quick links.

**Visual Design:**

**Summary Cards Grid:** 2×2 on desktop, 1 column on mobile.

| Card | Content | Color Accent |
|------|---------|--------------|
| Products Active | Icon + count + label | `secondary-500` icon |
| Variants Active | Icon + count + label | `secondary-400` icon |
| Pending Orders | Icon + count + label | `warning-500` icon |
| Verifying Orders | Icon + count + label | `info-500` icon |
| Paid Orders | Icon + count + label | `success-500` icon |

**Card Design:**
- Background: `neutral-0`
- Border: `neutral-200` 1px
- Border-radius: 12px
- Padding: `space-5` (20px)
- Shadow: `0 1px 3px rgba(0,0,0,0.08)`
- Icon: 24px, positioned top-left
- Count: `display` typography (48px), `neutral-800`
- Label: `body-sm`, `neutral-500`

**Quick Links Section:**

| Link | Icon | Destination |
|------|------|-------------|
| "จัดการสินค้า" | Lucide `package` | `/admin/products` |
| "ดูออเดอร์" | Lucide `shopping-bag` | `/admin/orders` |

**Quick Link Design:**
- Button with `outline` variant
- Icon + text, `md` size
- Arranged in horizontal row on desktop, vertical stack on mobile

**Page Title:** "Dashboard" in `h2`, with greeting "ยินดีต้อนรับ" in `body`, `neutral-500`

---

### 3. `/admin/products` — Product Listing

**Purpose:** View, search, filter, and toggle products.

**Layout:** Table view with toolbar.

**Toolbar:**
| Element | Specification |
|---------|---------------|
| Search Input | `text` input, placeholder "ค้นหาสินค้า...", with Lucide `search` icon |
| Filter Toggle | `ghost` button with `filter` icon, toggles filter panel |
| Create Button | `primary` button, icon `plus`, text "สร้างสินค้าใหม่" |

**Filter Panel (collapsible):**
- Active filter: Toggle switch for "แสดงเฉพาะที่เปิด" (show active only)
- Clear filters link

**Table Design:**

| Column | Header | Content | Width |
|--------|--------|---------|-------|
| Product | "สินค้า" | Product name (`h4`, `neutral-800`) + description preview (`body-sm`, `neutral-500`, max 1 line) | Flex |
| Variants | "ตัวเลือก" | Count badge (`secondary-500` background) | 80px |
| Active | "สถานะ" | Toggle switch, green when active | 100px |
| Actions | "จัดการ" | `ghost` icon button, Lucide `pencil`, links to edit page | 80px |

**Row Design:**
- Background: `neutral-0`
- Border-bottom: `neutral-200` 1px
- Hover: `primary-50` background
- Cell padding: `space-3` vertical, `space-4` horizontal
- Row height: minimum 64px for touch

**Toggle Switch Design:**
- Width: 44px, Height: 24px
- Active: `success-500` background, white knob
- Inactive: `neutral-300` background, white knob
- Transition: 200ms ease

**Empty State:**
- Centered illustration (Lucide `package-x` icon, 64px, `neutral-300`)
- Text: "ยังไม่มีสินค้า" in `h3`, `neutral-500`
- Subtext: "เริ่มต้นโดยการสร้างสินค้าใหม่" in `body-sm`, `neutral-400`
- CTA Button: "สร้างสินค้าใหม่" `primary` button

**Loading State:**
- Skeleton rows (5 rows)
- Each row: gray animated rectangles matching content layout

---

### 4. `/admin/products/new` — Create Product + Variants

**Purpose:** Create a new product with at least one variant atomically.

**Layout:** Two-section form on single page.

**Page Header:**
- Title: "สร้างสินค้าใหม่" in `h2`
- Back link: "← กลับไปรายการสินค้า" in `body-sm`, `neutral-500`

**Section 1: Product Information (Card)**

| Field | Type | Validation | Notes |
|-------|------|------------|-------|
| ชื่อสินค้า (name) | text input | **required**, max 256 chars | |
| รายละเอียด (description) | textarea | optional | 3 rows min, max 500 chars |
| หมวดหมู่ (category) | text input | optional | |
| Image URLs | text input | optional, valid URL if provided | comma-separated for multiple |

**Field Design:**
- Label: `body-sm`, `neutral-700`, above input
- Input: Height 44px, `border-radius: 8px`
- Required indicator: Red asterisk `*` after label in `error-500`

**Section 2: Variants (Card)**

**Variant Table Header:** "ตัวเลือกสินค้า (Variants)" + Add button

| Column | Header | Width |
|--------|--------|-------|
| Label | "ชื่อตัวเลือก" | 150px |
| Price | "ราคา (บาท)" | 120px |
| Weight | "น้ำหนัก (กรัม)" | 120px |
| Stock | "สต็อก" | 100px |
| Active | "เปิดใช้งาน" | 80px |
| Actions | | 60px |

**Variant Row Input Fields:**
- Text inputs: Height 40px, `border-radius: 6px`
- Number inputs: Same sizing, right-aligned text for numbers
- Active toggle: Same as product listing toggle
- Remove button: `ghost` icon button, Lucide `trash-2`, `error-500` on hover

**Add Variant Button:**
- `outline` button, full width at bottom of table
- Icon: `plus`
- Text: "เพิ่มตัวเลือก"

**Validation Messages:**
- Inline, below each field
- `caption`, `error-500`
- "ต้องมีอย่างน้อย 1 ตัวเลือก" for variant section

**Submit Area (sticky footer on mobile):**
- Submit button: `primary` variant, text "สร้างสินค้า"
- Cancel link: "ยกเลิก" returns to product list
- Gap: `space-4` between buttons

**Success State:**
- Redirect to `/admin/products/:productId`
- Toast: "สร้างสินค้าสำเร็จ" in `success-500`

**Error State:**
- Inline errors on invalid fields
- Error summary at top: "กรุณาแก้ไขข้อมูลที่ไม่ถูกต้อง" in `error-600`

---

### 5. `/admin/products/:productId` — Edit Product & Variants

**Purpose:** Edit product details and manage variants including stock adjustment.

**Layout:** Product form + variants table.

**Page Header:**
- Title: Product name in `h2` (truncated if > 40 chars)
- Back link: "← กลับไปรายการสินค้า"

**Section 1: Product Information Card**

Same fields as Create page:
- ชื่อสินค้า (name) — required
- รายละเอียด (description) — optional
- หมวดหมู่ (category) — optional
- Image URLs — optional
- Active toggle

**Section 2: Variants Table**

| Column | Content |
|--------|---------|
| Label | Text, editable inline or modal |
| Price | Number, editable |
| Weight | Number, editable |
| Stock | Current stock number + "ปรับสต็อก" button |
| Active | Toggle switch |
| Actions | Edit + Delete (with confirmation) |

**Stock Adjustment Modal:**

| Field | Type | Notes |
|-------|------|-------|
| Current Stock | Display only | Shows current value |
| Delta | number input | Positive to add, negative to reduce |
| New Stock | Display only | Updates as delta changes |
| Reason | textarea | **required**, max 200 chars |

**Modal Design:**
- Size: `md` (640px max-width)
- Header: "ปรับสต็อก" in `h3`
- Footer: Cancel (ghost) + Confirm (primary) buttons
- Guard: Confirm disabled if `current + delta < 0` or reason empty
- Error: "ไม่สามารถปรับสต็อกได้: จะติดลบ" in `error-500` below delta input

**Active Warning:**
- If product is inactive but variant is active:
- Badge: "สินค้าถูกปิด การแสดงผลลูกค้าจะถูกซ่อน" in `warning-50` background, `warning-700` text

**Submit Area:**
- Save button: `primary` variant, text "บันทึกการเปลี่ยนแปลง"
- Success toast: "บันทึกสำเร็จ"
- Error state: Inline field errors

---

### 6. `/admin/orders` — Order Listing

**Purpose:** Search, filter, and browse orders with pagination.

**Layout:** Filters + Table + Pagination.

**Filter Bar:**

| Element | Type | Notes |
|---------|------|-------|
| Search | text input | Placeholder "ค้นหาออเดอร์...", searches order_id/name/phone |
| Status | select dropdown | All / Pending / Verifying / Paid / Packing / Shipped / Canceled |
| Date From | date input | Optional |
| Date To | date input | Optional |
| Clear Filters | ghost link | Resets all filters |

**Filter Row:** Horizontal on desktop, stacked on mobile.

**Table Design:**

| Column | Header | Width |
|--------|--------|-------|
| Order ID | "รหัสออเดอร์" | 160px |
| Status | "สถานะ" | 120px |
| Total | "ยอดรวม" | 120px |
| Created | "วันที่สั่ง" | 140px |
| Expires | "หมดเข้า" | 140px |
| Actions | | 80px |

**Status Badge Design:**
- `pending`: `warning-50` bg, `warning-700` text
- `verifying`: `info-50` bg, `info-600` text
- `paid`: `success-50` bg, `success-700` text
- `packing`: `secondary-50` bg, `secondary-600` text
- `shipped`: `primary-50` bg, `primary-700` text
- `canceled_expired`: `neutral-100` bg, `neutral-500` text

**Expiration Warning:**
- If order is pending and `now > expires_at`:
- Badge: "หมดเวลา" in `error-500` text, pulsing dot indicator

**Row Hover:** `primary-50` background

**Actions Column:**
- View button: `ghost` icon button, Lucide `eye`, links to detail

**Pagination:**
- Bottom of table
- Shows: "แสดง 1-20 จาก 45 ออเดอร์" in `body-sm`
- Page buttons: Previous / page numbers / Next
- Previous/Next: `ghost` buttons, disabled when at bounds

---

### 7. `/admin/orders/:orderId` — Order Detail

**Purpose:** View complete order details and change status with optional tracking.

**Layout:** Order summary cards + status control panel.

**Page Header:**
- Title: "ออเดอร์ #{order_id}" in `h2`
- Back link: "← กลับไปรายการออเดอร์"
- Current status badge (same design as listing)

**Section 1: Buyer Information Card**

| Field | Value Display |
|-------|---------------|
| ชื่อผู้สั่งซื้อ | Buyer name |
| เบอร์โทร | Phone number |
| ที่อยู่ | Full address, `body-sm` |

**Section 2: Order Items Card**

Table:

| Column | Content |
|--------|---------|
| สินค้า | Variant label |
| จำนวน | Quantity |
| ราคา/ชิ้น | Unit price |
| รวม | Line total |

**Totals Display:**
- Subtotal, Shipping Fee, **Total** (bold, larger)
- `price` typography for monetary values

**Section 3: Payment Slip**

| Element | Design |
|---------|--------|
| Slip preview | Image thumbnail, max 200px width, click to enlarge |
| Slip link | Opens in new tab |
| Uploaded at | Timestamp |

**If no slip:** "ยังไม่มีสลิป" in `neutral-400`

**Section 4: Timeline Card**

| Event | Timestamp |
|-------|-----------|
| สั่งซื้อเมื่อ | created_at |
| หมดเข้าเมื่อ | expires_at |
| จ่ายเมื่อ | paid_at (if applicable) |
| ส่งเมื่อ | shipped_at (if applicable) |

**Section 5: Tracking Card** (if tracking exists)

| Field | Value |
|-------|-------|
| ขนส่ง | tracking_vendor |
| เลขติดตาม | tracking_id (monospace) |
| Link | External tracking URL if available |

**Section 6: Status Control Panel**

**Current Status:** Large badge display

**Allowed Transitions:** Buttons for each valid next status

| Transition | Button Style |
|------------|--------------|
| pending → verifying | `primary` button |
| pending → canceled_expired | `destructive` outline button |
| verifying → paid | `primary` button |
| paid → packing | `primary` button |
| packing → shipped | `primary` button |

**Tracking Fields (shown when transitioning to "shipped"):**

| Field | Type | Required |
|-------|------|----------|
| ขนส่ง (tracking_vendor) | text input | optional |
| เลขติดตาม (tracking_id) | text input | optional |

**Transition Modal:**
- Header: "ยืนยันเปลี่ยนสถานะ"
- Body: "เปลี่ยนสถานะจาก {current} เป็น {new}?" + tracking fields if applicable
- Footer: Cancel (ghost) + Confirm (primary)
- Confirm button text: "ยืนยัน"

**Success:** Toast "เปลี่ยนสถานะสำเร็จ" + badge updates immediately

**Error:** Inline error "ไม่อนุญาตให้เปลี่ยนสถานะนี้" in `error-500`

---

### 8. `/admin/settings` — Admin Settings (Optional Minimal)

**Purpose:** View and optionally edit critical store settings.

**Layout:** Simple form with read-only display.

**Settings Display:**

| Setting | Display Format |
|---------|----------------|
| Payment Timeout | "{value} นาที" |
| Shipping Fee Ranges | JSON or formatted list |
| Admin Session TTL | "{value} ชั่วโมง" |
| Admin Access Code | Masked "••••••••" + Last changed date |

**Edit Mode (if permitted):**
- Each setting has "แก้ไข" link
- Opens inline edit or modal
- Confirm password before saving

**Design:** Same card-based layout as other admin pages. Read-only fields in `neutral-0` cards, edit buttons as `ghost` text buttons.

---

## Component Specifications

### Toast Notifications

| Variant | Background | Text | Icon |
|---------|-----------|------|------|
| Success | `success-50` | `success-700` | Lucide `check-circle` |
| Error | `error-50` | `error-700` | Lucide `alert-circle` |
| Warning | `warning-50` | `warning-700` | Lucide `alert-triangle` |
| Info | `info-50` | `info-700` | Lucide `info` |

**Position:** Top-right corner, stacked
**Duration:** 4 seconds, dismissible
**Animation:** Slide in from right, fade out

### Confirm Dialog

| Property | Value |
|-----------|-------|
| Backdrop | `rgba(0,0,0,0.5)` |
| Modal background | `neutral-0` |
| Border-radius | 16px |
| Max-width | 400px |
| Header | `h3`, close X button |
| Body | `body`, explanation text |
| Footer | Right-aligned, Cancel (ghost) + Confirm (variant per action) |

**Destructive confirm:** Confirm button in `destructive` variant.

### Loading States

**Button loading:**
- Spinner icon (Lucide `loader-2`, animated rotation)
- Text: "กำลังโหลด..." or original text
- Disabled state

**Page loading:**
- Full-page skeleton
- Sidebar skeleton
- Content area: animated gray rectangles

**Table loading:**
- 5 skeleton rows
- Pulsing animation (opacity 0.5 → 1)

### Empty States

**Design:**
- Centered vertically
- Icon: 64px, `neutral-300`
- Title: `h3`, `neutral-500`
- Description: `body-sm`, `neutral-400`
- CTA button when applicable

---

## Navigation

### Sidebar Navigation

| Item | Icon | Route |
|------|------|-------|
| Dashboard | Lucide `layout-dashboard` | `/admin` |
| สินค้า (Products) | Lucide `package` | `/admin/products` |
| ออเดอร์ (Orders) | Lucide `shopping-bag` | `/admin/orders` |
| ตั้งค่า (Settings) | Lucide `settings` | `/admin/settings` |

**Active State:**
- Background: `primary-50`
- Text: `primary-600`, weight 600
- Left border: 3px solid `primary-500`
- Icon: `primary-500`

**Hover State:**
- Background: `neutral-100`

**Sidebar Footer:**
- Admin info: "Admin" label
- Logout button: `ghost` icon button, Lucide `log-out`

---

## Accessibility Requirements

- All form inputs have associated labels (`<label>` or `aria-label`)
- Toggle switches have `aria-checked`
- Status badges have `role="status"` or `aria-label`
- Modals trap focus and close on Escape
- Error messages have `role="alert"`
- Loading states announced via `aria-busy`
- Color contrast: minimum 4.5:1 for text
- Touch targets: minimum 44×44px
- Focus visible: 2px solid `primary-400` outline

---

## Responsive Breakpoints for Admin

| Name | Width | Admin Layout |
|------|-------|--------------|
| `sm` | < 640px | Single column, hidden sidebar (hamburger), stacked filters |
| `md` | 640–767px | Single column, collapsible sidebar |
| `lg` | 768–1023px | Sidebar + content, 2-column dashboard |
| `xl` | 1024px+ | Full layout, 2×2 dashboard grid |

---

## Implementation Notes

1. **CSS Architecture:** Use design tokens as CSS custom properties. Admin-specific tokens should be prefixed with `--admin-`.

2. **Component Library:** Build these reusable components:
   - `AdminShell` — Layout wrapper (sidebar + topnav + content)
   - `AdminCard` — Card container for sections
   - `AdminTable` — Sortable table with hover states
   - `AdminBadge` — Status badges
   - `AdminToggle` — Toggle switch
   - `AdminModal` — Confirm dialog
   - `AdminToast` — Toast notification
   - `AdminPagination` — Pagination controls

3. **Form Handling:** Use controlled components with validation on blur and submit.

4. **API Integration:** All admin endpoints require `X-Admin-Token` header. Handle 401 redirects to login.

5. **State Management:** Local state for forms, server state for lists. Use optimistic updates for toggles with rollback on error.

---

## Files to Create

| File | Description |
|------|-------------|
| `components/admin/AdminShell.tsx` | Layout wrapper |
| `components/admin/AdminSidebar.tsx` | Sidebar navigation |
| `components/admin/AdminTopNav.tsx` | Top navigation |
| `components/admin/AdminCard.tsx` | Card container |
| `components/admin/AdminTable.tsx` | Table component |
| `components/admin/AdminBadge.tsx` | Status badge |
| `components/admin/AdminToggle.tsx` | Toggle switch |
| `components/admin/AdminModal.tsx` | Modal dialog |
| `components/admin/AdminToast.tsx` | Toast system |
| `components/admin/AdminPagination.tsx` | Pagination |
| `pages/admin/Login.tsx` | Login page |
| `pages/admin/Dashboard.tsx` | Dashboard page |
| `pages/admin/Products.tsx` | Product listing |
| `pages/admin/ProductNew.tsx` | Create product |
| `pages/admin/ProductEdit.tsx` | Edit product |
| `pages/admin/Orders.tsx` | Order listing |
| `pages/admin/OrderDetail.tsx` | Order detail |
| `pages/admin/Settings.tsx` | Settings page |
| `styles/admin.css` | Admin-specific styles |

---

**Owner:** UI Designer
**Status:** Design Specification Complete
**Next:** Hand off to Engineer for implementation
