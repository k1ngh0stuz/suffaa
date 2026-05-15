# Suffa IT Academy — Video Course Sales Platform

**Academy:** Suffa IT Academy  
**Phone:** +998 50 155 67 00  
**Telegram:** [@suffaitacademy](https://t.me/suffaitacademy)  
**Instagram:** [@suffaitacademy](https://instagram.com/suffaitacademy)  
**Location:** Ibrат Yangikurgan, Buvayda District, Fergana Region, Uzbekistan

---

## Courses

| # | Name | Status | Sales |
|---|------|--------|-------|
| 1 | Network Engineer | ✅ Ready | Available, price TBD |
| 2 | Windows Server 2022 | 🔧 In Development | Button hidden — show "Coming Soon" |
| 3 | Ethical Hacker | 🔧 In Development | Button hidden — show "Coming Soon" |

---

## Tech Stack

Next.js 14 (App Router) · TypeScript · Supabase (Auth + DB + Storage) · Tailwind CSS · Mux / Cloudflare Stream · UzumPay · PayMe · Paynet · Click · Vercel

---

## Phase 0 — Architecture & Setup

- [x] Create repository
- [ ] Configure ESLint / Prettier / Husky
- [ ] Design DB schema (users, courses, modules, videos, enrollments, payments, manual_grants)
- [ ] Set up Supabase: project, migrations, RLS policies
- [ ] Configure environment variables (.env.local, Vercel secrets)
- [ ] Set up CI/CD pipeline (GitHub Actions → Vercel)

---

## Phase 1 — Public Website (Landing Page)

- [ ] Hero section — "Suffa IT Academy" name, tagline, CTA button
- [ ] "About Us" section — academy history, mission, team
- [ ] "Courses" section — course cards fetched from DB
  - [ ] Course 1: **Network Engineer** — active "Enroll" button, price shown as "on request" until confirmed
  - [ ] Course 2: **Windows Server 2022** — status "In Development", inactive "Coming Soon" button
  - [ ] Course 3: **Ethical Hacker** — status "In Development", inactive "Coming Soon" button
  - [ ] CourseCard component: support for `available` / `coming_soon` statuses
  - [ ] When `coming_soon`: hide price and replace button with placeholder
- [ ] "Why Us" section — academy advantages
- [ ] "Location" section — map with marker at academy address (Google Maps iframe or Yandex Maps)
- [ ] Contact block next to map:
  - Phone: +998 50 155 67 00
  - Telegram: @suffaitacademy → https://t.me/suffaitacademy
  - Instagram: @suffaitacademy → https://instagram.com/suffaitacademy
- [ ] Footer — phone, social links, copyright
- [ ] Responsive layout (mobile-first)
- [ ] SEO: meta tags, og:image, sitemap.xml, robots.txt

---

## Phase 2 — Authentication

- [ ] Sign up / sign in (email + password via Supabase Auth)
- [ ] OAuth: Google (optional — Telegram login)
- [ ] User profile page — /profile
- [ ] Middleware to protect /dashboard and /admin routes
- [ ] Password recovery via email
- [ ] User roles: `student`, `admin` (role field in profiles table)

---

## Phase 3 — Payment Integration

- [ ] Abstract PaymentProvider interface — unified API for all gateways
- [ ] **UzumPay** integration — order creation, webhook, confirmation
- [ ] **PayMe** integration — create / perform / cancel / check, webhook
- [ ] **Paynet** integration — payment initialization, status, callback
- [ ] **Click** integration — prepare / complete, webhook signature
- [ ] Payment gateway selection page `/checkout/[courseId]`
- [ ] `payments` table in DB: status, gateway, amount, user_id, course_id, transaction_id
- [ ] Auto-create `enrollment` after successful webhook
- [ ] `/payment/success` and `/payment/fail` pages
- [ ] Idempotency: duplicate webhooks do not create duplicate records
- [ ] Email notification to student after payment (Resend / Nodemailer)

---

## Phase 4 — Video Courses & Access Control

- [ ] Upload videos to Mux / Cloudflare Stream (never to a public bucket)
- [ ] Generate signed URLs for videos — token expires after N hours, only if enrollment exists
- [ ] Block direct access to video files without token (Supabase RLS)
- [ ] Course page `/courses/[slug]` — list of modules and lessons
- [ ] Lesson page `/courses/[slug]/lessons/[id]` — player + materials
- [ ] Guard: if no enrollment — redirect to `/checkout`
- [ ] Watch progress — save position in `lesson_progress` table
- [ ] Student dashboard `/dashboard` — my courses, progress

---

## Phase 5 — Admin Panel

- [ ] `/admin` route — accessible only with `admin` role
- [ ] Course management: create / edit / delete / hide / toggle status (`available` ↔ `coming_soon`)
- [ ] Module and lesson management, video upload
- [ ] Student list with search and filtering
- [ ] **Manual access grant (cash payment):**
  - Search student by email or phone number
  - Select course (e.g. Network Engineer)
  - Create `enrollment` + record in `manual_grants` with a comment
- [ ] Revoke access: deactivate enrollment from admin panel
- [ ] Payment log: all transactions, statuses, gateways (UzumPay / PayMe / Paynet / Click)
- [ ] Manual grant log: who, when, which course, comment
- [ ] Statistics: revenue by gateway, active students, video views

---

## Phase 6 — Testing & Launch

- [ ] Unit tests: webhook handler logic for all 4 gateways (Jest)
- [ ] E2E tests: sign up → payment → video access (Playwright)
- [ ] Test all 4 payment gateways in sandbox mode
- [ ] Security audit: RLS, signed URLs, CSRF, rate limiting
- [ ] Lighthouse: performance, accessibility, SEO ≥ 90
- [ ] Error monitoring setup (Sentry)
- [ ] Production deploy: domain, SSL, environment variables on Vercel

---

## Notes

- Courses 2 and 3 (Windows Server 2022, Ethical Hacker) should be activated automatically via status change in the admin panel — no redeployment needed
- Network Engineer course price is stored in DB and pulled dynamically on the landing page
- Academy address for map (Google Maps embed): **Ibrat Yangikurgan, Buvayda District, Fergana Region, Uzbekistan**
- Videos are never stored in public storage — only served via signed URLs generated server-side
