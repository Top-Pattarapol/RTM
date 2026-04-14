# RTM-61 — Order Confirmation Page After Successful Payment

**Issue:** RTM-61 Order confirmation page after successful payment
**Type:** UI Enhancement
**Priority:** High
**Revision:** 1.0 • **Date:** 2026-04-14 • **Owner:** UI Designer

---

## 1. Concept & Vision

The order confirmation page serves two distinct emotional moments:

1. **Pending/Verifying** — "Your order is created, awaiting payment" — tentative, uncertain
2. **Paid/Completed** — "Payment confirmed! Your order is officially in." — celebratory, final

This spec focuses on the **Paid/Completed state**, transforming the page into a confirmation celebration that makes customers feel confident their order is processed and on its way.

**Design philosophy:**
- Celebratory but tasteful — warm confetti, not fireworks
- Clear next steps — what happens after payment
- Confidence-building — reassure customer their order is real

---

## 2. Design Language

### Aesthetic Direction
Warm celebration with feline charm. Think: a happy cat receiving treats — confident, satisfied, inviting.

### Color Palette (Paid State Additions)

| Token | Hex | Usage |
|-------|-----|-------|
| `paid-celebration-bg` | `success-50` | Highlighted paid section background |
| `paid-glow` | `primary-200` | Subtle glow effect for celebration |
| `confetti-gold` | `primary-300` | Decorative confetti accent |
| `confetti-pink` | `secondary-400` | Decorative confetti accent |

### Typography
Same as design system. Use `display` for celebration headline.

### Motion & Animation
- **Confetti burst:** 8-12 small colored circles/squares animate falling from top on page load (paid state only)
- **Checkmark scale-in:** CheckCircle icon scales from 0.8 → 1.0 with bounce easing on mount
- **Card fade-slide:** Cards fade in with subtle upward motion, staggered 100ms
- **Duration:** 300-500ms, ease-out curves
- **Respect `prefers-reduced-motion`:** Skip confetti if user prefers reduced motion

---

## 3. Layout & Structure

### Paid State Layout

```
┌─────────────────────────────────────────────────────────────┐
│  [Confetti particles - animated, top area]                 │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  ✓ Paid Confirmation Header (larger, celebratory)    │   │
│  │    "ชำระเงินสำเร็จ!" (larger headline)                 │   │
│  │    "ReadyToMeow จะดูแลคำสั่งซื้อของคุณ"                  │   │
│  │    [Order ID badge - prominent]                      │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────┐  ┌─────────────────────┐          │
│  │  Order Details Card │  │  Payment Confirmed   │          │
│  │  (items, shipping)  │  │  Card (stamp-style)  │          │
│  └─────────────────────┘  └─────────────────────┘          │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Next Steps Card                                     │   │
│  │  "สิ่งที่จะเกิดขึ้นต่อไป"                               │   │
│  │  • Admin will verify payment slip                   │   │
│  │  • We'll ship when ready                             │   │
│  │  • Track status anytime with your Order ID          │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  [Action Buttons Row]                                       │
│  [Track Order] [Download Receipt] [Back to Home]          │
└─────────────────────────────────────────────────────────────┘
```

### Responsive Behavior
- Single column on mobile, two-column for detail cards on `md+`
- Confetti container is full-width, overflow hidden
- Action buttons stack vertically on mobile

---

## 4. Component Specifications

### 4.1 Celebration Header (Paid State)

**Visual Design:**

| Element | Specification |
|---------|---------------|
| Container | Card with `paid-celebration-bg` background, `success-500` left border accent (4px) |
| CheckCircle | 64px (larger than pending state 48px), `success-500`, scale-in animation |
| Headline | "ชำระเงินสำเร็จ!" in `h1` (36px), `success-700` |
| Subtext | "ReadyToMeow จะดูแลคำสั่งซื้อของคุณ" in `body-lg`, `neutral-600` |
| Order ID | Prominent badge with `primary-100` background, `primary-700` text, `font-mono` |

**States:**
- **Paid:** Celebration header with confetti
- **Verifying:** "กำลังตรวจสอบ" header with `info-500` accent, no confetti
- **Pending:** Original "สั่งซื้อสำเร็จ!" header, neutral styling

### 4.2 Payment Confirmed Stamp Card (New)

**Purpose:** Visual "stamp of approval" confirming payment is recorded.

**Visual Design:**

| Element | Specification |
|---------|---------------|
| Container | Card with `neutral-0` background, dashed `success-500` border (2px), `border-radius: 12px` |
| Icon | Lucide `stamp` or `badge-check`, 32px, `success-500` |
| Title | "ยืนยันการชำระ" in `h4`, `neutral-800` |
| Timestamp | "ชำระเมื่อ {datetime}" in `caption`, `neutral-500` |
| Label | "Payment ID: {transaction_ref}" in `font-mono`, `body-sm` |

**Animation:** Subtle pulse on mount (scale 1 → 1.02 → 1), 600ms

### 4.3 Next Steps Card (New)

**Purpose:** Set expectations for what happens after payment.

**Visual Design:**

| Element | Specification |
|---------|---------------|
| Container | Card with `neutral-0` background |
| Header | "สิ่งที่จะเกิดขึ้นต่อไป" in `h4`, with Lucide `clipboard-list` icon |
| List | 3 items with checkmark icons, `body-sm`, `neutral-600` |
| List Item 1 | "Admin จะตรวจสอบสลิปภายใน 1-2 ชั่วโมง" — Lucide `clock` |
| List Item 2 | "เตรียมสินค้าและจัดส่งภายใน 1-2 วันทำการ" — Lucide `package` |
| List Item 3 | "ติดตามสถานะได้ตลอดเวลาด้วย Order ID" — Lucide `map-pin` |

### 4.4 Confetti System

**Implementation:**
- CSS-only or lightweight animation (no heavy library)
- 8-12 particles: circles (8px) and squares (6px)
- Colors: `primary-300`, `primary-400`, `secondary-400`, `success-500`
- Animation: `translateY(-100vh)` to `translateY(100vh)` over 3s, staggered start times
- Particle positions: Random horizontal distribution across top 20% of container
- `prefers-reduced-motion`: Hide confetti, keep static decorative dots

---

## 5. Status-Based Rendering

### Status → Visual Treatment Mapping

| Status | Header Style | Confetti | Payment Stamp | Next Steps Card |
|--------|-------------|----------|--------------|----------------|
| `pending` | Neutral, "สั่งซื้อสำเร็จ!" | No | No | No |
| `verifying` | Info tint, "กำลังตรวจสอบ" | No | No | No (show "รอตรวจสอบ" message) |
| `paid` | Success celebration, "ชำระเงินสำเร็จ!" | **Yes** | **Yes** | **Yes** |
| `shipped` | Primary tint, "จัดส่งแล้ว!" | No | Yes (paid timestamp) | Yes (tracking info) |
| `completed` | Success celebration, "ส่งถึงแล้ว!" | **Yes** | Yes | Yes (review request) |

### Enhanced Cards per Status

**Verifying State:**
- Replace "Next Steps" with "รอตรวจสอบสลิป" card showing:
  - "Admin กำลังตรวจสอบสลิปของคุณ"
  - "โปรดรอสักครู่ — มักใช้เวลาไม่เกิน 2 ชั่วโมง"
  - Lucide `hourglass` icon, `info-500`

**Shipped State:**
- Add tracking card before Next Steps:
  - "ข้อมูลการจัดส่ง" header
  - "ขนส่ง: {vendor}" and "เลขพัสดุ: {tracking_id}" (monospace)
  - Link button to tracking URL

**Completed State:**
- Add "รีวิว" card:
  - "ช่วยแบ่งปันประสบการณ์" with star rating prompt
  - (Defer to future iteration — show placeholder)

---

## 6. Interaction Details

### Download Receipt Button
- **Trigger:** Click on "ดาวน์โหลดใบสรุป" button
- **Behavior:** Opens print dialog with print-optimized CSS
- **Print CSS considerations:**
  - Hide Header, Footer, confetti, action buttons
  - Show business info (store name, contact)
  - Include: Order ID, date, items, buyer info, totals, payment confirmed timestamp
  - Monochrome-friendly (not required to be color)

### Track Order Button
- Links to `/track?order={order_id}`
- Pre-fills order ID in tracking search

### Back to Home
- Links to `/`
- Clears cart after confirmation (good UX: prevents accidental re-order)

---

## 7. Technical Approach

### File: `frontend/src/pages/OrderConfirmation.tsx`

**Changes needed:**

1. **Extend status handling:**
   ```tsx
   const isPaidOrBeyond = ['paid', 'shipped', 'completed'].includes(order.status);
   const isVerifying = order.status === 'verifying';
   const isPending = order.status === 'pending';
   ```

2. **Add confetti component:**
   - Conditional render based on `isPaidOrBeyond`
   - CSS animation with `prefers-reduced-motion` check
   - Small SVG/CSS particles, positioned absolutely

3. **Add PaymentStampCard component:**
   - New sub-component or inline
   - Shows `paid_at` timestamp
   - Stamp-style dashed border

4. **Add NextStepsCard component:**
   - Different content per status
   - Icon + text list format

5. **Conditional card rendering based on status**

6. **Update print styles:**
   - Add `@media print` CSS
   - Hide non-essential elements

### Key Implementation Notes
- Keep existing functionality intact
- Add new components without breaking existing flow
- Ensure all animations are CSS-based (no JS animation libraries)
- Test with `prefers-reduced-motion: reduce`

---

## 8. Accessibility

- Confetti must be `aria-hidden="true"` (decorative)
- All interactive elements keyboard accessible
- Status badges have `role="status"` or `aria-label`
- Focus visible on all buttons
- Print version must be readable (contrast, font size)

---

## 9. Files to Modify

| File | Change |
|------|--------|
| `frontend/src/pages/OrderConfirmation.tsx` | Add status-based rendering, confetti, stamp card, next steps card |
| (Optional) `frontend/src/components/ui/confetti.tsx` | Extract confetti as standalone component if reusable |

---

## 10. Acceptance Criteria

1. [ ] `paid`, `shipped`, `completed` statuses show celebratory header with confetti
2. [ ] Confetti respects `prefers-reduced-motion`
3. [ ] Payment stamp card shows for `paid`+ statuses
4. [ ] Next steps card adapts content per status
5. [ ] Shipped status shows tracking information prominently
6. [ ] Print styles hide decorative elements, show receipt details
7. [ ] All animations are smooth (60fps target)
8. [ ] No breaking changes to existing pending/verifying states
