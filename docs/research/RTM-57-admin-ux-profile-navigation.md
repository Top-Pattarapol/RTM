# Research: Admin Page UX - User Profile & Navigation

**Issue**: RTM-57 RTM-56a
**Date**: 2026-04-14
**Researcher**: UX Researcher Agent
**Status**: Complete

---

## Research Questions

1. How well does the Admin interface communicate the logged-in admin's identity?
2. How efficiently can admins navigate between different sections?
3. Are there any usability issues with the current navigation pattern?

---

## Methodology

Heuristic evaluation based on Nielsen's 10 usability heuristics, combined with cognitive walkthrough analysis of the admin interface pages:
- AdminDashboard.tsx
- AdminAccounts.tsx
- AdminOrders.tsx
- AdminOrderDetail.tsx
- AdminProducts.tsx
- AdminProductEdit.tsx
- AdminProductNew.tsx

Design system alignment check against DESIGN-SYSTEM.md

---

## Findings

### Finding 1: No User Profile Identity in Admin Header

**Severity**: High

**Evidence**:
The `AuthContext` stores the admin's `user_id`, `username`, and `role`, but this information is never displayed in the admin interface header.

In `AdminDashboard.tsx` (lines 54-68), the header only shows:
- A home link button
- Page title "แดชบอร์ดแอดมิน"
- A logout button

The `user` object from `useAuth()` is available but unused in the header.

**Impact**:
- Admins cannot verify which account they are logged in as
- In shared computer environments, users may accidentally perform actions on the wrong account
- No way to distinguish between multiple admin accounts without checking the backend logs
- Violates Nielsen's heuristic #3: "User control and freedom" — users can't easily confirm their identity

---

### Finding 2: No Confirmation on Logout Action

**Severity**: Medium

**Evidence**:
In `AdminDashboard.tsx` (lines 42-48), the logout handler immediately logs out without confirmation:

```tsx
const handleLogout = () => {
  logout();
  toast({ title: 'ออกจากระบบสำเร็จ' });
  navigate('/');
};
```

**Impact**:
- Accidental clicks on logout can cause loss of unsaved work
- No undo option — the action is immediate
- Violates Nielsen's heuristic #3: "User control and freedom" — support undo and exit

---

### Finding 3: No Persistent Navigation Structure

**Severity**: High

**Evidence**:
Each admin page implements its own independent header with a back button (`<ArrowLeft />`). There is no:
- Sidebar navigation
- Tab bar
- Persistent navigation bar at the top of every page

All navigation is card-based on the Dashboard (lines 129-172), requiring users to return to `/admin` to navigate to other sections.

**Impact**:
- Users must always return to Dashboard to change sections — linear, inefficient flow
- No sense of "where am I" within the system — no breadcrumbs, no active indicators
- High cognitive load for remembering the navigation path
- Violates Nielsen's heuristic #4: "Consistency and standards" — not consistent with typical admin interface patterns

---

### Finding 4: Inconsistent Header Layout Across Pages

**Severity**: Medium

**Evidence**:
- `AdminDashboard` header: Home icon + title + logout button (horizontal layout)
- `AdminOrders` header: Back icon + title on first row, then filters on second row (two-row layout)
- `AdminAccounts` header: Back icon + title on first row, then search + filters on second row (two-row layout)
- `AdminOrderDetail` header: Back icon + title + subtitle (order ID) on same row

**Impact**:
- Users cannot predict where navigation elements will appear
- Inconsistent visual weight and alignment
- Violates Nielsen's heuristic #4: "Consistency and standards"

---

### Finding 5: No Breadcrumb Navigation

**Severity**: Medium

**Evidence**:
Deep pages like `/admin/orders/:orderId` or `/admin/products/:productId` have no breadcrumb trail. A user at `AdminOrderDetail` cannot easily:
- See the path: Admin → Orders → Order Detail
- Click "Orders" to go back without using the back button
- Navigate to sibling pages

**Impact**:
- Users lose context when navigating deep into the system
- Back button is the only way to go up one level
- No way to jump to parent sections directly
- Violates Nielsen's heuristic #3: "User control and freedom"

---

### Finding 6: Navigation Cards Have No Hover Feedback

**Severity**: Low

**Evidence**:
In `AdminDashboard.tsx` (lines 131-171), the navigation cards use `hover:bg-muted/50 transition-colors` but:
- No scale or elevation change on hover
- No visual indicator that the entire card is clickable
- Cursor is pointer, but affordance is weak

**Impact**:
- Users may not realize the entire card is a navigation target
- Affordance is not clearly communicated
- Violates Nielsen's heuristic #1: "Visibility of system status"

---

### Finding 7: Missing "My Profile" or Account Settings

**Severity**: Medium

**Evidence**:
The `AdminAccounts` page (`/admin/accounts`) is for managing ALL user accounts (CRUD). However, there is no dedicated "My Profile" section where an admin can:
- View their own account details
- See their role and permissions
- Update their own contact information (TikTok ID, Line ID, phone)

The admin's own profile data is visible in AuthContext but not accessible through any UI.

**Impact**:
- Admins cannot update their own contact details without going through the full accounts list
- No separation between "my profile" and "user management"
- Violates Nielsen's heuristic #5: "Error prevention" — should provide restricted views for different roles

---

### Finding 8: No Loading State Indicator in Headers

**Severity**: Low

**Evidence**:
Pages like `AdminAccounts` and `AdminOrders` show `LoadingOverlay` but the header area (with filters) remains interactive during loading. The header doesn't indicate "loading" state.

**Impact**:
- Minor confusion about system responsiveness during data fetches
- Violates Nielsen's heuristic #1: "Visibility of system status"

---

## Recommendations

### Priority 1 (Critical)

**1. Add User Profile Identity to Admin Header**
- Show logged-in admin's username and role badge in all admin page headers
- Place it consistently in the top-left area (next to or replacing the home icon)
- Add avatar placeholder with first letter of username

**Estimated Impact**: High — addresses core trust and identity issue

---

### Priority 2 (High)

**2. Add Confirmation Dialog on Logout**
- Show confirmation dialog: "ออกจากระบบ?" with Cancel/Logout options
- Prevents accidental logout and data loss

**Estimated Impact**: Medium — prevents frustrating accidental actions

**3. Implement Persistent Sidebar Navigation**
- Create a collapsible sidebar (240px width per design system specs) with:
  - Admin profile section at top
  - Navigation links: Dashboard, Products, Orders, Accounts
  - Active state indicator (primary-50 background, primary-500 text, left border)
  - Logout at bottom
- Make sidebar collapsible on mobile (hamburger menu)

**Estimated Impact**: High — standardizes navigation, reduces cognitive load

---

### Priority 3 (Medium)

**4. Standardize Header Layout Across Pages**
- Use consistent single-row header: [Back/Profile] [Title] [Actions]
- Move filters/search to a secondary toolbar row only when needed
- Document header pattern in design system

**Estimated Impact**: Medium — improves predictability

**5. Add Breadcrumbs to Deep Pages**
- Show path: Admin > [Section] > [Item]
- Make parent sections clickable
- Use separator ">" with muted color

**Estimated Impact**: Medium — improves orientation in deep hierarchies

---

### Priority 4 (Low)

**6. Add "My Profile" Quick Access**
- In sidebar or header, show logged-in admin's name with dropdown to:
  - View own profile (read-only)
  - Sign out
- Consider adding admin's own contact info edit capability

**Estimated Impact**: Low-Medium — improves self-service

---

## Design Direction Reference

Based on `DESIGN-SYSTEM.md` Side Navigation specs (lines 262-273):

```
| Property | Value |
|-----------|-------|
| Width | 240px |
| Background | neutral-50 |
| Border-right | neutral-200 1px |
| Item height | 40px |
| Item padding | 12px 16px |
| Active item | primary-50 background, primary-500 text, left border 3px primary-500 |
| Hover item | neutral-100 background |
```

---

## Handover

@UI-Designer — Recommendations #2 (sidebar), #3 (header), #4 (breadcrumbs) require design decisions.

@Engineer — Recommendations #1 (profile display), #2 (logout confirmation), #3 (sidebar) require implementation.

---

## Appendix: Pages Analyzed

| Page | Route | User Profile Issue | Navigation Issue |
|------|-------|-------------------|------------------|
| AdminDashboard | /admin | No profile shown | No sidebar |
| AdminAccounts | /admin/accounts | No own profile shown | No breadcrumb |
| AdminOrders | /admin/orders | No profile shown | No breadcrumb |
| AdminOrderDetail | /admin/orders/:id | No profile shown | No breadcrumb |
| AdminProducts | /admin/products | No profile shown | No breadcrumb |
| AdminProductEdit | /admin/products/:id | No profile shown | No breadcrumb |
| AdminProductNew | /admin/products/new | No profile shown | No breadcrumb |
