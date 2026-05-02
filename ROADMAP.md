# Roadmap

> Solo development, part-time pace. Dates are targets, not deadlines.
> The goal is to ship something real, not something rushed.

**Phase = release version.** Phase 1 ends with v1 in the App Store. Phase 2 is a major update focused on business and community growth.

---

## Done

### Backend MVP — April 2026
- [x] Project setup (TypeScript, Express, Swagger, ESLint, Jest)
- [x] PostgreSQL schema — users, tokens, rides, ride_participants
- [x] Authentication — register, login, logout, refresh tokens, email verification
- [x] User profile — CRUD, avatar upload (Cloudflare R2), location, bio, Instagram
- [x] Rides — create, update, join, leave, status management
- [x] Cron job — auto-activate rides when `date_time` is reached (`planned` → `active`)
- [x] Ride filters — geo-radius (earthdistance), status, date range, moto type, engine size
- [x] Pagination — page / limit on rides list
- [x] Capacity check — enforce max_participants on join
- [x] Metrics endpoint (Prometheus-style)
- [x] Health check
- [x] Test coverage (Jest)
- [x] Swagger / OpenAPI documentation

### iOS Foundation — April 2026
- [x] App architecture (SwiftUI, AppState, navigation)
- [x] Auth flow — login, register, email verification
- [x] User profile — view, edit (username, location, Instagram), avatar upload
- [x] Settings — change password, resend verification, notification preferences, logout
- [x] Networking layer — JWT auth, auto token refresh, error handling
- [x] Secure token storage (Keychain)
- [x] Localization framework (EN, UK, PL, DE)
- [x] Location service

### iOS Core Screens — April 2026
- [x] Rides list screen — paginated feed with RideCardView, filterable by status / moto type / engine size
- [x] Create ride screen — full form (location, date, moto type, engine size, passenger seat)
- [x] Edit ride screen — edit fields and change ride status (with allowed transition logic)
- [x] My rides screen — rides created and joined by the current user; refreshes on navigation back
- [x] Join / leave ride — with capacity check and error handling
- [x] New shared components — RideStatusBadge, ChipSelector, FilterButton, EmptyStateView

### Backend + iOS — April 2026
- [x] `GET /rides/:ride_id/participants` — participants with user info (username, avatar, bio)
- [x] Status transition validation — blocked: cannot activate a ride with future `date_time`; cannot complete a ride that hasn't started
- [x] Ride detail screen — info, participants list with avatars, creator card (Trust Score placeholder), navigate to public profile
- [x] Public profile screen — read-only view with avatar, username, location, stats (Trust/Rides as placeholders), bio, Instagram link
- [x] Map screen — MapKit with iOS 17 API, nearby rides as color-coded pins, tap to select with preview card, filter sheet, locate button; shows nearest 100 rides sorted by geo distance with limit hint when at capacity
- [x] News tab — "Coming Soon" placeholder (full feed planned for v2)
- [x] Ride card status badge — "In progress" for active rides, countdown for planned/created, hidden for cancelled/completed
- [x] ~~Follow system~~ — removed; replaced by Trust & Riding Circle (earned trust through real rides)
- [x] Security audit — IDOR fix on rides, rate limiting (helmet, express-rate-limit), CORS locked, avatar upload size limit, refresh token hashing, password complexity rules, coordinate validation
- [x] Security audit for iOS — certificate pinning, tokens to Keychain, password complexity, memory hygiene, screenshot prevention, debug log sanitization

---

## Phase 1 — v1 App Store Release
**Target: May – August 2026**

Everything needed for a complete, shippable v1. Phase 1 ends when the app is in the App Store.
The goal: a product people actually want to use — a closed core loop from finding a ride to building trust in the community.

### Core Loop
```
Find a ride → Join → Ride together → Rate the ride → Add creator to Riding Circle → See their next ride first
```

### Logging
- [x] Backend — add `pino` structured JSON logging (request method/path/status/duration, errors with stack traces)
- [x] Backend — replace all `console.log` / `console.error` with pino logger instance

### Forgot Password
- [x] Backend — `POST /auth/forgot-password` — send a reset link via email (hashed token, 1h expiry)
- [x] Backend — `POST /auth/reset-password` — validate token, set new password, invalidate all refresh tokens
- [x] iOS — "Forgot password?" link on login screen → enter email screen
- [x] iOS — deep link handler for reset URL from email → new password screen

### Trust & Riding Circle
- [x] Backend — ride_reviews table, POST /rides/:id/rate, avg_rating on profile (one rating per ride → trust score of the creator)
- [x] Backend — riding_circle table, add/remove riders, circle_count on profile
- [x] Backend — GET /rides/pending-reviews, POST /rides/:id/dismiss-review (one-time prompt, server-side)
- [x] Backend — rides_count on profile (completed rides only)
- [x] Backend — leave blocked for completed rides
- [x] Backend — feed priority: Riding Circle creators shown first in GET /rides
- [x] Backend — creator blocked from rating their own ride (400)
- [x] Backend — pending-reviews excludes rides the user created
- [x] iOS — post-ride rating screen (one rating for the ride, not per participant; one-time prompt on app open)
- [x] iOS — pre-fills existing rating when re-opening the review screen
- [x] iOS — "Add creator to Riding Circle" suggestion after giving 4–5 ⭐
- [x] iOS — creator cannot see or submit the rate button for their own ride
- [x] iOS — completed and cancelled rides open in detail view, not edit view
- [x] iOS — Trust Score on profile (avg_rating + rides_count + circle_count)
- [x] iOS — Riding Circle list on profile — trash button with confirmation alert; swipe-to-remove also requires confirmation
- [x] iOS — Add / Remove from Riding Circle on any public profile (button hidden on own profile)
- [x] iOS — circle_count updates live in ProfileView immediately after add/remove without a reload
- [x] iOS — bug fix: creator no longer sees "Join" button on cancelled or completed rides
- [x] iOS — Riding Circle navigation fix: NavigationLink now navigates on first tap (switched from value-based to destination-based)
- [x] iOS — My Rides sort: active/planned rides first (ascending by date), cancelled/completed last (descending)
- [x] iOS — MapSnapshotView: draws orange pin at start and white pin at end coordinate over the static map image (MKMapSnapshotter + UIGraphicsImageRenderer)
- [x] iOS — Ride detail start location: tappable label opens Apple Maps with driving directions

### Rider Languages
*For emigrants and travelers who want to ride but face a language barrier.*
- [x] Backend — `languages` field on user profile; rides return `creator_languages` via JOIN (no separate field on rides — language comes from the creator's profile)
- [x] iOS — `LanguagePickerField` — full-screen sheet with search, checkmarks, localized names sorted A→Z, collapsed pill row when closed
- [x] iOS — language selector on profile edit (e.g. 🇺🇦 Ukrainian · 🇬🇧 English)
- [x] iOS — language flags visible on ride card, map preview card, ride detail organizer card, and public profile
- [x] iOS — filter rides by language in the ride list
- [x] iOS — languages section on Public Profile view (flag + name chips)
- [x] Backend — `GET /rides` language filter: `u.languages && $n::text[]`

### Auth Enhancements
- [x] iOS — Face ID / Touch ID login (LocalAuthentication + Keychain, biometric token stored in separate Keychain key, persists across logout)
- [x] Backend — Google OAuth endpoint (`POST /auth/google`, google-auth-library, find-or-create user)
- [x] iOS — Sign in with Google (GIDSignIn SDK, custom button, Google Cloud Console configured)
- [x] Backend — Sign in with Apple endpoint (`POST /auth/apple`, apple-signin-auth, find-or-create user)
- [x] iOS — Sign in with Apple (code complete — capability requires Apple Developer account to activate in Xcode)

### Localization Expansion (RU/BE)
*Russian and Belarusian interface — emigrants and cross-border riders.*
- [x] iOS — перевірити що RU і BE присутні в `Language.all` (`Language.swift`) для фічі "Languages I speak" — RU вже був, BE (🇧🇾) додано
- [x] iOS — додати `ru.lproj/Localizable.strings` — переклад інтерфейсу на Russian
- [x] iOS — додати `be.lproj/Localizable.strings` — переклад інтерфейсу на Belarusian
- [x] iOS — оновити `LanguageSelector` — додати RU і BE як варіанти вибору мови інтерфейсу

### Website
*Next.js лендінг — `moto-community-web/`, окремий GitHub репо, деплой на Railway. Потрібен до App Store submission (Privacy Policy URL).*
- [x] Setup — Next.js 16 проєкт в `moto-community-web/`, готовий до пушу в GitHub
- [x] Односторінковий лендінг — мультимовний (EN, UK, PL, DE, RU, BE)
- [x] Privacy Policy сторінка — публічний URL для App Store Connect
- [x] i18n роутинг — URL-підпапки (`/en/`, `/uk/`, `/ru/` тощо), locale detection через Accept-Language
- [x] `hreflang` теги + `x-default` — вказує Google яка мовна версія для якого регіону
- [x] `generateMetadata` — унікальні `<title>` і `<meta description>` на кожній мові
- [x] Open Graph теги + OG Image (динамічна `opengraph-image.tsx`, 1200×630) — превʼю в соцмережах і месенджерах
- [x] Twitter Card (`summary_large_image`)
- [x] JSON-LD structured data — `MobileApplication` schema → rich results в Google
- [x] `sitemap.xml` — усі мовні версії всіх сторінок, автогенерація через `next-sitemap`
- [x] `robots.txt` — дозволити індексацію всіх публічних сторінок
- [x] Семантична розмітка — `<header>`, `<main>`, `<nav>`, `<section>`, `<footer>`, `<h1>`, `<h2>`
- [x] Cloudflare Web Analytics — beacon token отримано, `NEXT_PUBLIC_CF_BEACON_TOKEN` додано в Railway env
- [x] Deploy — GitHub репо створено, підключено до Railway, `NEXT_PUBLIC_SITE_URL=https://motocommunity.app`, домен `motocommunity.app` придбано і підключено
- [x] iOS — після публікації сайту: додати Privacy Policy посилання в `AboutAppView` (нова секція "Legal", відкриває URL в Safari); додати `privacy_policy` ключ у всі 6 файлів локалізації
- [ ] Website — секція App Screenshots між Features і DownloadCTA
  - **Блокер:** емодзі (прапорці мов) не відображаються в симуляторі — знімати тільки на реальному пристрої або після фіксу
  - Потрібні 4 скріншоти (iPhone 16 Pro Max, 6.9", мова EN, темний режим):
    - [ ] **Rides List** — декілька поїздок з різними статусами і мовними прапорцями
    - [ ] **Map** — декілька пінів на карті, один вибраний з превью
    - [ ] **Ride Detail** — учасники, маршрут, кнопка Join
    - [ ] **Profile** — Trust Score, Riding Circle, мови
  - [ ] Додати компонент `Screenshots.tsx` на лендінг (горизонтальний скрол або grid)
  - [ ] Оновити всі 6 мовних `json` — новий ключ для заголовку секції

### Push Notifications
- [ ] **Purchase Apple Developer Program** — $99/year, required for APNs, TestFlight, App Store
- [ ] Backend — APNs integration, device token storage, notification service
- [ ] iOS — register device token on login, handle foreground/background/terminated state
- [ ] Notification types: someone joined your ride, ride status changed, new ride from Circle member
- [ ] Notification preferences — per-type on/off (UI already exists in Settings)

### Infrastructure & DevOps
- [x] CI/CD pre-production — GitHub Actions (lint → test → docker build → Railway deploy → Telegram notify)
- [ ] Production environment on Railway (окремий environment, окремий Neon DB)
- [ ] GitHub Environments: production secrets (RAILWAY_SERVICE_ID, DATABASE_URL, SECRET_KEY, REFRESH_KEY тощо)
- [ ] CI/CD production — workflow тригер на git tag `v*.*.*` → docker tag `production` → Railway deploy
- [ ] GitHub Releases — вручну після кожного production deploy (тег → release notes → publish)
- [ ] Certificate Pinning — після налаштування production домену: `openssl s_client -connect <production-domain>:443 2>/dev/null | openssl x509 -pubkey -noout | openssl pkey -pubin -outform der | openssl dgst -sha256 -binary | openssl enc -base64` → додати хеш у `PinningURLSessionDelegate.pinnedHashes` поруч з pre-production хешем

### Launch Prep
- [x] iOS — password complexity hints on register/change-password screens — real-time `PasswordHintsView` (4 pill indicators: 8+, A–Z, 0–9, !@#)
- [x] Error states and empty states on all screens — RideListView, RidingCircleView (wifi.slash + retry), MapView (error/locationDenied/empty/limit banners), RideDetail (no participants yet)
- [x] Loading skeletons / indicators — ProfileView location skeleton, avatar upload overlay with ProgressView
- [x] Offline handling — `NetworkClient` catches `.notConnectedToInternet` / `.networkConnectionLost`, surfaces readable error to user
- [x] Performance — image caching (`ImageCache` NSCache singleton, `AvatarView` with `.task(id:)`), pagination edge case fix (`hasMorePages` uses `total_pages` from response)
- [x] App icon, launch screen, onboarding flow — launch screen (black bg + logo), `OnboardingView` (3 slides, shown once via `@AppStorage`), `AboutAppView` з "View App Tour" кнопкою
- [x] QA all four languages (EN, UK, PL, DE) — fixed missing `status_open` in UK/PL/DE; fixed typo "Not verifed" in EN
- [x] Telegram Bot — @motocommunity_bot (support); Telegram Channel — @motocommunityapp (announcements)
- [x] Support & Donate page in Settings — `SupportView` with: Telegram Channel (community news), Telegram Bot (support), Buy Me a Coffee (donation); opens Telegram app or fallback Safari; placeholder links until bot/channel are created
- [ ] Beta test with 10–20 real riders via TestFlight
- [ ] Bug fixing from beta feedback
- [ ] App Store Connect setup:
  - [ ] Screenshots — iPhone 6.9" (1320×2868 px), мінімум 3-5 екранів (зробити в симуляторі або на пристрої)
  - [ ] App description, subtitle, keywords — у 4 мовах: EN, UK, PL, DE
  - [ ] Privacy Policy — URL з `moto-community-web/` (без нього Apple не прийме)
  - [ ] Category: Social Networking (або Sports)
  - [ ] Age rating: заповнити анкету Apple (зазвичай 12+)
  - [ ] Developer info — береться з Apple Developer акаунту автоматично
- [ ] Submit for App Store review
- [ ] Launch in Ukraine as first market

**v1 launch goal: August 2026**

---

## Phase 2 — v2 Business & Community
**Target: Q4 2026**

Only after v1 is live and stable with real user feedback.
Phase 2 is business-oriented — features that grow the community and generate revenue.

### Ride Tracking & Ride Summary Card
- [ ] Backend — `route_points` (JSONB array `{lat, lng, timestamp}`), `distance_km`, `avg_speed_kmh` on rides table
- [ ] iOS — CLLocationManager background tracking during active ride; GPS noise filtering; real-time distance + avg speed calculation
- [ ] iOS — Ride Summary Card: branded visual card after ride completion (route map snapshot, distance, avg speed, duration, date, participants)
- [ ] iOS — Share button → `UIActivityViewController` → save to Photos or share directly to Instagram Stories (via `instagram-stories` URL scheme)

### Local Feed
- [ ] Backend — feed algorithm: nearby rides + Riding Circle activity + community events
- [ ] iOS — Local feed screen replacing the "Coming Soon" placeholder
- [ ] iOS — activity cards: new rides nearby, rides from Circle, community events

### SOS Help
- [ ] Backend — sos_requests table, POST /sos, GET /sos/nearby, resolve endpoint
- [ ] iOS — SOS button (persistent, accessible from map screen)
- [ ] iOS — nearby SOS alerts with push notification
- [ ] iOS — SOS history on profile
- [ ] Push notification type: SOS alert from nearby rider

### Passenger Mode
- [ ] Backend — passenger role in ride_participants, seat availability logic
- [ ] iOS — "I have a free seat" toggle when creating a ride
- [ ] iOS — "Join as passenger" option on ride details

### Community & Monetization
- [ ] Clubs — create/join private groups, member roles, club rides
- [ ] STO Catalog — service station directory with ratings and reviews
- [ ] Local business announcements — geo-targeted posts from shops, events, brands
- [ ] Sponsored rides — brand/shop sponsorship visible on ride card
- [ ] Android app — find a contributor or co-developer

---

## Phase 3 — v3 Platform
**Target: 2027**

- [ ] Bike History — service and ownership records by plate number
- [ ] Marketplace — buy/sell gear and motorcycles
- [ ] Web dashboard — for club organizers and STO owners
- [ ] Chat — direct messages between riders
- [ ] Open API — public read-only API for event aggregators and navigation apps

---

## Notes

- Phase 1 = v1 App Store release. Nothing ships until the core loop is complete.
- Push notifications are blocked on Apple Developer account ($99) — buy before starting APNs work
- App Store review can take 1–14 days — submit early
- SOS moved to Phase 2 — it requires push notifications and is too complex for v1
- Local Feed moved to Phase 2 — News tab shows "Coming Soon" in v1
- Android is not planned for solo development — looking for a contributor or co-founder
