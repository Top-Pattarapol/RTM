# ReadyToMeow Design System

## Overview

ReadyToMeow is a warm, playful, and trustworthy e-commerce platform for the Thai market. The visual language balances feline charm with professional credibility — approachable without being childish, distinctive without being distracting. Every interaction should feel smooth, responsive, and delightful, like a cat's graceful movement.

## Brand Personality

- **Tone**: Warm, playful, confident, trustworthy
- **Visual metaphor**: A well-groomed cat: sleek, intentional, warm
- **Avoid**: Cartoonish excess, corporate coldness, clutter

---

## Color Palette

### Primary Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `primary-50` | `#FEF7F0` | Light tint — backgrounds, hover states |
| `primary-100` | `#FDECC8` | Cards, subtle highlights |
| `primary-200` | `#FBDB8E` | Borders, dividers |
| `primary-300` | `#F8C84E` | Active states, badges |
| `primary-400` | `#F5B81A` | Primary buttons, links |
| `primary-500` | `#E5A012` | Primary action — CTAs, key UI |
| `primary-600` | `#C7850A` | Button hover, emphasis |
| `primary-700` | `#9A6506` | Active/pressed states |
| `primary-800` | `#6E4604` | Dark text on light bg only |
| `primary-900` | `#4A2F02` | Rarely used — deep contrast text |

**Usage guideline**: Use `primary-500` for primary buttons and links. Use `primary-400` for hover states. Use `primary-50` and `primary-100` for subtle backgrounds and cards.

### Secondary Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `secondary-50` | `#F0F4FF` | Soft blue tint backgrounds |
| `secondary-400` | `#7AA2F7` | Secondary links, icons |
| `secondary-500` | `#5B8DEF` | Secondary buttons, highlights |
| `secondary-600` | `#3D6FD1` | Secondary hover |

**Usage guideline**: Secondary is for supporting actions, informational elements, and decorative accents. Never compete with primary color for attention.

### Neutral Palette

| Token | Hex | Usage |
|-------|-----|-------|
| `neutral-0` | `#FFFFFF` | Card backgrounds, input backgrounds |
| `neutral-50` | `#FAFAFA` | Page background |
| `neutral-100` | `#F5F5F5` | Subtle section backgrounds |
| `neutral-200` | `#EBEBEB` | Borders, dividers |
| `neutral-300` | `#D4D4D4` | Disabled borders, placeholder text |
| `neutral-400` | `#A3A3A3` | Placeholder text, helper text |
| `neutral-500` | `#737373` | Secondary text, labels |
| `neutral-600` | `#525252` | Body text |
| `neutral-700` | `#404040` | Headings on light bg |
| `neutral-800` | `#262626` | High-contrast headings |
| `neutral-900` | `#171717` | Maximum contrast text |

**Usage guideline**: Body text defaults to `neutral-600`. Headings use `neutral-700` to `neutral-800` depending on level. Never use pure black (`#000000`) — it creates harsh contrast.

### Semantic Colors

| Token | Hex | Usage |
|-------|-----|-------|
| `success-50` | `#F0FDF4` | Success background |
| `success-500` | `#22C55E` | Success icons, indicators |
| `success-600` | `#16A34A` | Success text/hover |
| `success-700` | `#15803D` | Success emphasis |

| Token | Hex | Usage |
|-------|-----|-------|
| `error-50` | `#FEF2F2` | Error background |
| `error-500` | `#EF4444` | Error icons, destructive buttons |
| `error-600` | `#DC2626` | Error text/hover |
| `error-700` | `#B91C1C` | Error emphasis |

| Token | Hex | Usage |
|-------|-----|-------|
| `warning-50` | `#FFFBEB` | Warning background |
| `warning-500` | `#F59E0B` | Warning icons |
| `warning-600` | `#D97706` | Warning text/hover |
| `warning-700` | `#B45309` | Warning emphasis |

| Token | Hex | Usage |
|-------|-----|-------|
| `info-50` | `#EFF6FF` | Info background |
| `info-500` | `#3B82F6` | Info icons |
| `info-600` | `#2563EB` | Info text/hover |

**Usage guideline**: Semantic colors appear on their respective `-50` background with `-500` to `-600` text/icon. Never use bare semantic colors on white — always provide background contrast.

---

## Typography

### Font Stack

- **Display/Headings**: `"Noto Sans Thai", "Noto Sans", system-ui, sans-serif`
  - Use for all headings H1–H4
- **Body**: `"Inter", "Noto Sans Thai", "Noto Sans", system-ui, sans-serif`
  - Use for body text, labels, UI copy
- **Monospace**: `"JetBrains Mono", "Fira Code", monospace`
  - Use for code, prices, SKUs, technical data

### Type Scale

| Token | Font | Size | Weight | Line Height | Usage |
|-------|------|------|--------|-------------|-------|
| `display` | Noto Sans Thai | 48px | 700 (Bold) | 1.1 | Hero headlines |
| `h1` | Noto Sans Thai | 36px | 700 | 1.2 | Page titles |
| `h2` | Noto Sans Thai | 28px | 600 | 1.25 | Section titles |
| `h3` | Noto Sans Thai | 22px | 600 | 1.3 | Card headings, subsections |
| `h4` | Noto Sans Thai | 18px | 600 | 1.35 | Small headings, group labels |
| `body-lg` | Inter | 18px | 400 | 1.6 | Lead text, descriptions |
| `body` | Inter | 16px | 400 | 1.6 | Default body text |
| `body-sm` | Inter | 14px | 400 | 1.5 | Secondary text, captions |
| `caption` | Inter | 12px | 500 | 1.4 | Labels, badges, timestamps |
| `price` | JetBrains Mono | 18px | 600 | 1.2 | Product prices |
| `price-lg` | JetBrains Mono | 24px | 700 | 1.1 | Hero prices, sale prices |

### Text Hierarchy Guidelines

- **Page titles** (H1): `neutral-700`, left-aligned, no truncation
- **Section titles** (H2): `neutral-700`, with visible spacing (24px margin-top)
- **Card titles** (H3): `neutral-800`, 2-line max with ellipsis
- **Body text**: `neutral-600`, max 80 characters per line
- **Helper/caption text**: `neutral-500`, never use for critical info

---

## Spacing Scale

Base unit: **4px**

| Token | Value | Pixels | Usage |
|-------|-------|--------|-------|
| `space-0` | 0 | 0px | Reset |
| `space-1` | 1 | 4px | Icon gaps, tight spacing |
| `space-2` | 2 | 8px | Between related elements |
| `space-3` | 3 | 12px | Input padding, compact cards |
| `space-4` | 4 | 16px | Default padding, gutters |
| `space-5` | 5 | 20px | Card padding, section gaps |
| `space-6` | 6 | 24px | Section padding |
| `space-8` | 8 | 32px | Large section gaps |
| `space-10` | 10 | 40px | Major section breaks |
| `space-12` | 12 | 48px | Page-level spacing |
| `space-16` | 16 | 64px | Hero spacing |

**Container widths**:
- Max content width: `1200px`
- Card max width: `400px`
- Input max width: `480px`

---

## Component Patterns

### Buttons

#### Variants

| Variant | Background | Text | Border | Usage |
|---------|-----------|------|--------|-------|
| `primary` | `primary-500` | white | none | Main CTA, submit |
| `secondary` | `secondary-500` | white | none | Secondary actions |
| `outline` | transparent | `primary-500` | `primary-500` 2px | Alternative CTAs |
| `ghost` | transparent | `neutral-600` | none | Tertiary, subtle actions |
| `destructive` | `error-500` | white | none | Delete, remove |

#### Sizes

| Size | Height | Padding | Font | Border Radius |
|------|--------|---------|------|--------------|
| `sm` | 32px | 12px 16px | `caption` | 6px |
| `md` | 40px | 12px 20px | `body-sm` | 8px |
| `lg` | 48px | 14px 28px | `body` | 10px |

#### States

- **Default**: As specified above
- **Hover**: Darken background by 10% (use next token step, e.g., `primary-500` → `primary-600`)
- **Active/Pressed**: Darken by additional 5%, scale `0.98`
- **Disabled**: `neutral-300` background, `neutral-400` text, cursor `not-allowed`
- **Loading**: Show spinner, text "Loading...", button disabled

#### Usage Rules

- Primary button appears **once per page section** — never multiple primary buttons competing
- Always pair ghost or outline buttons as secondary actions alongside primary
- Icon-only buttons must have `aria-label`
- Minimum touch target: 44px × 44px

---

### Inputs

#### Text Input

| State | Background | Border | Text | Placeholder |
|-------|-----------|--------|------|-------------|
| Default | `neutral-0` | `neutral-300` 1px | `neutral-700` | `neutral-400` |
| Hover | `neutral-0` | `neutral-400` 1px | `neutral-700` | `neutral-400` |
| Focus | `neutral-0` | `primary-500` 2px | `neutral-700` | — |
| Error | `error-50` | `error-500` 2px | `neutral-700` | — |
| Disabled | `neutral-100` | `neutral-200` 1px | `neutral-400` | — |

**Dimensions**: Height 44px, padding `12px 16px`, border-radius `8px`

#### Label & Helper Text

- Labels: `body-sm`, `neutral-700`, positioned above input, required asterisk in `error-500`
- Helper text: `caption`, `neutral-500`, positioned below input
- Error message: `caption`, `error-500`, replaces helper text on error

#### Input Variants

- **Text input**: Single line, max 256 characters
- **Textarea**: Multi-line, min 3 rows, max height 200px, resize vertical only
- **Select**: Native or custom dropdown, same sizing as text input
- **Checkbox**: 18×18px, border-radius 4px, checkmark in `primary-500`
- **Radio**: 18×18px, border-radius 50%, selected dot in `primary-500`

---

### Cards

| Property | Value |
|-----------|-------|
| Background | `neutral-0` |
| Border | `neutral-200` 1px |
| Border-radius | 12px |
| Padding | `space-5` (20px) |
| Shadow (default) | `0 1px 3px rgba(0,0,0,0.08)` |
| Shadow (hover) | `0 4px 12px rgba(0,0,0,0.12)` |
| Hover transition | 200ms ease-out |

**Card content structure**:
- Image area: Top, 16:9 aspect ratio, `border-radius: 12px 12px 0 0`
- Content area: `space-4` padding
- Title: H3, max 2 lines with ellipsis
- Description: `body-sm`, `neutral-500`, max 3 lines
- Footer: action area, aligned bottom

---

### Navigation

#### Top Navigation Bar

| Property | Value |
|-----------|-------|
| Height | 64px |
| Background | `neutral-0` |
| Border-bottom | `neutral-200` 1px |
| Shadow | `0 1px 2px rgba(0,0,0,0.05)` |
| Logo | Left-aligned, max-height 36px |
| Nav links | `body-sm`, `neutral-600`, hover → `primary-500` |
| CTA button | Right-aligned, `primary` variant |

#### Side Navigation (Dashboard)

| Property | Value |
|-----------|-------|
| Width | 240px |
| Background | `neutral-50` |
| Border-right | `neutral-200` 1px |
| Item height | 40px |
| Item padding | `12px 16px` |
| Active item | `primary-50` background, `primary-500` text, left border 3px `primary-500` |
| Hover item | `neutral-100` background |

---

### Modals & Overlays

| Property | Value |
|-----------|-------|
| Backdrop | `rgba(0,0,0,0.5)` |
| Modal background | `neutral-0` |
| Modal border-radius | 16px |
| Modal padding | `space-6` |
| Modal max-width | 480px (sm), 640px (md), 800px (lg) |
| Box-shadow | `0 20px 40px rgba(0,0,0,0.2)` |
| Enter animation | Fade in + scale from 0.95, 200ms ease-out |
| Exit animation | Fade out + scale to 0.95, 150ms ease-in |

**Modal header**: H3, close button (X) top-right, border-bottom `neutral-200`
**Modal footer**: Right-aligned buttons, `space-4` gap between buttons, border-top `neutral-200`

---

### Badges & Tags

| Variant | Background | Text | Border-radius |
|---------|-----------|------|--------------|
| `default` | `neutral-100` | `neutral-700` | 6px |
| `primary` | `primary-100` | `primary-700` | 6px |
| `success` | `success-50` | `success-700` | 6px |
| `warning` | `warning-50` | `warning-700` | 6px |
| `error` | `error-50` | `error-700` | 6px |

**Sizes**: Height 22px (sm), 26px (md). Padding 4px 8px (sm), 6px 10px (md).

---

### Tables

| Property | Value |
|-----------|-------|
| Header background | `neutral-50` |
| Header text | `body-sm`, `neutral-700`, weight 600 |
| Row border-bottom | `neutral-200` 1px |
| Row hover | `primary-50` |
| Cell padding | `space-3` `space-4` |
| Border-radius | 8px (container), 0 (cells) |

---

## Brand Guidelines

### Logo Usage

- **Primary logo**: Text "ReadyToMeow" in Noto Sans Thai Bold + subtle cat ear motif (optional icon form)
- **Icon form**: Cat silhouette or paw print for favicon, app icon, minimal contexts
- **Minimum clear space**: 8px on all sides
- **Minimum size**: 24px height for icon, 80px width for wordmark

### Iconography

- **Style**: Outlined icons, 1.5px stroke, rounded caps and joins
- **Size**: 20px default for UI, 16px for inline, 24px for feature icons
- **Library**: Use [Lucide Icons](https://lucide.dev) (open source, consistent stroke weight)
- **Color**: `neutral-600` default, `neutral-400` for decorative, `primary-500` for active state icons

### Imagery

- **Product photos**: Clean white or light gray background, consistent aspect ratio (4:3 for grid, 1:1 for thumbnails)
- **Hero images**: Warm, natural lighting, slight lifestyle context
- **Avoid**: Stock photo clichés, overly edited images, inconsistent image treatments

### Visual Tone

- Warm neutrals with amber/golden accents
- Generous whitespace — let content breathe
- Subtle shadows for depth (never harsh drop shadows)
- Rounded corners (8–16px) for approachability
- Micro-copy is friendly but not silly (never "Meow!" in UI text)

---

## Responsive Breakpoints

| Name | Min Width | Max Container | Notes |
|------|-----------|---------------|-------|
| `xs` | 0px | 100% | Extra small — fallback for very old devices |
| `sm` | 640px | 640px | Large phones, small tablets |
| `md` | 768px | 768px | Tablets |
| `lg` | 1024px | 1024px | Small laptops |
| `xl` | 1280px | 1200px | Desktops |

**Mobile-first approach**: Styles are written for `xs` first, then enhanced at each breakpoint with `min-width`.

**Column grid**:
- Mobile: 1 column
- Tablet (md): 2 columns
- Desktop (lg): 3–4 columns
- Wide (xl): Max 4 columns with 1200px container

---

## Accessibility

- All interactive elements have visible focus states (`outline: 2px solid primary-400, outline-offset: 2px`)
- Color contrast: Minimum 4.5:1 for body text, 3:1 for large text and UI components
- Touch targets: Minimum 44×44px
- Semantic HTML: Use `<button>`, `<a>`, `<input>` for their intended purposes
- ARIA: Labels on icons, roles on interactive composites, live regions for dynamic content
- Reduced motion: Respect `prefers-reduced-motion` — disable animations when set

---

## Implementation Notes for Engineer

1. **CSS Custom Properties**: All tokens should be implemented as CSS custom properties (variables) for easy theming and dark mode future-proofing.
2. **Spacing utility classes**: Generate `space-{n}` utilities matching the spacing scale.
3. **Component isolation**: Each component should be self-contained with scoped styles.
4. **Font loading**: Use `<link>` for Google Fonts with `display=swap` for performance.
5. **Icon build step**: Set up a Lucide icon import strategy (tree-shakeable imports).
