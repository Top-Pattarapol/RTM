# Tech Stack

## Chosen Stack

| Layer | Technology | Version | Notes |
|-------|-----------|---------|-------|
| Frontend Language | TypeScript | 5.8.x | Strict typing, React 18 |
| Frontend Framework | React + Vite | 7.x (Vite) | SPA with client-side routing |
| Frontend UI | Radix UI + TailwindCSS | latest | Headless components, shadcn/ui patterns |
| Backend Language | Node.js (ES Modules) | LTS | Type: module, no build step |
| Backend Framework | Express | 4.18.x | REST API |
| Database | Google Sheets API | - | Sheets-as-database via googleapis |
| Auth | Firebase Admin + JWT | - | Firebase auth tokens, JWT sessions |
| Payments | PromptPay QR | - | Thai QR payment integration |
| Hosting | TBD | - | Frontend: static/CDN; Backend: Node.js host |
| CI/CD | TBD | - | GitHub Actions likely |

## Frontend (`meow-sheet-shop`)

**Runtime:**
- React 18.3 (SPA)
- React Router 6 (client routing)
- TanStack Query 5 (server state)
- Zod 3.25 (runtime validation)
- React Hook Form 7 + resolvers

**UI Layer:**
- Radix UI primitives (headless, accessible)
- TailwindCSS 3.4 + animate + typography
- Lucide React (icons)
- Recharts 2 (charts)
- Sonner (toasts)
- Embla Carousel

**Utilities:**
- firebase-admin 13 (auth verification)
- googleapis 160 (Sheets integration)
- html2canvas, pdf-lib, qrcode, browser-image-compression

**Dev:**
- Vite 7 + @vitejs/plugin-react-swc
- TypeScript 5.8 + eslint 9 (flat config)
- PostCSS + Autoprefixer

## Backend (`meow-sheet-shop-be`)

**Runtime:**
- Node.js (ES Modules, `"type": "module"`)
- Express 4.18
- No build step — direct `node src/index.js`

**Middleware:**
- helmet 7 (security headers)
- cors 2.8
- morgan 1.10 (logging)
- express-rate-limit 7
- multer 1.4.5-lts.1 (file uploads)
- formidable 3.5 (parse form data)

**Libraries:**
- googleapis 128 (Sheets API)
- firebase-admin 13 (auth)
- bcrypt 5.1 (password hashing)
- jsonwebtoken 9 (JWT)
- swagger-jsdoc 6 + swagger-ui-express 5 (API docs)
- zod 3.22 (validation)
- node-cron 3 (scheduler)
- promptpay-qr 0.5 + qrcode 1.5 (QR generation)

**Dev:**
- eslint 8 + nodemon 3

## Architecture Notes

- **Database**: Google Sheets used as a spreadsheet database (no traditional RDBMS)
- **Backend serves as API-only**: no SSR, no template rendering
- **Frontend is decoupled**: calls backend REST API, connects to Google Sheets via service account
- **No monorepo tool** (npm workspaces, pnpm, etc.) — two separate repos managed as git submodules
- **No TypeScript on backend** — plain JavaScript with ES Modules

## Rationale

| Choice | Why |
|--------|-----|
| React + TypeScript | Strong typing, large ecosystem, team familiarity |
| Vite | Fast dev server, optimized builds |
| Radix UI + Tailwind | Headless = fully customizable; shadcn pattern for rapid development |
| Express (backend) | Minimal, unopinionated, easy to deploy |
| Google Sheets as DB | Zero-cost, easy non-technical updates, familiar to client |
| Firebase Auth | Managed auth, supports various providers |
| PromptPay QR | Standard Thai payment method |
| ES Modules | Modern Node.js default, cleaner imports |

## Trade-offs

| Trade-off | Impact | Mitigated by |
|-----------|--------|--------------|
| Google Sheets as DB | Concurrency limits (~100 concurrent writes) | Rate limiting, careful query design |
| No TypeScript on backend | Less safety, more runtime errors | Zod validation at API boundary |
| Separate repos | More complex CI/deploy | Git submodules in monorepo wrapper |
| No ORM | Custom Sheets API logic | Wrapped in lib layer |
| SPA + separate backend | SEO complexity | Meta tags, potential SSR later |

## Key Dependencies

| Package | Purpose | License | Notes |
|---------|---------|---------|-------|
| react, react-dom | UI framework | MIT | |
| express | HTTP server | MIT | |
| googleapis | Sheets API | Apache 2 | |
| firebase-admin | Auth | Apache 2 | |
| @radix-ui/* | Headless UI primitives | MIT | |
| tailwindcss | Styling | MIT | |
| zod | Schema validation | MIT | |
| bcrypt | Password hashing | MIT | |
| jsonwebtoken | JWT handling | MIT | |
| swagger-ui-express | API docs | MIT | |

---
_Documented during RTM-11. Tech stack is finalized._