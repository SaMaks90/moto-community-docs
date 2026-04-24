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

### Rider Languages
*For emigrants and travelers who want to ride but face a language barrier.*
- [ ] Backend — languages field on user profile, accepted_languages on rides
- [ ] iOS — language selector on profile (e.g. 🇺🇦 Ukrainian · 🇬🇧 English)
- [ ] iOS — language tags visible on ride card and ride details
- [ ] iOS — "Languages for this ride" field in create/edit ride (optional, empty = all welcome)
- [ ] iOS — filter rides by language in the ride list

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

### Push Notifications
- [ ] **Purchase Apple Developer Program** — $99/year, required for APNs, TestFlight, App Store
- [ ] Backend — APNs integration, device token storage, notification service
- [ ] iOS — register device token on login, handle foreground/background/terminated state
- [ ] Notification types: someone joined your ride, ride status changed, new ride from Circle member
- [ ] Notification preferences — per-type on/off (UI already exists in Settings)

### Polish & Launch Prep
- [ ] iOS — password complexity validation on register/change-password screens (min eight chars, uppercase, number, special character — mirrors backend rules)
- [ ] Error states and empty states on all screens
- [ ] Loading skeletons / indicators where missing
- [ ] Offline handling — graceful errors on no connection
- [ ] Performance — image caching, pagination edge cases
- [ ] App icon, launch screen, onboarding flow
- [ ] QA all four languages (EN, UK, PL, DE) — layouts, text overflow
- [ ] Beta test with 10–20 real riders via TestFlight
- [ ] Bug fixing from beta feedback
- [ ] App Store Connect setup — screenshots, description, keywords, age rating
- [ ] Submit for App Store review
- [ ] Launch in Ukraine as first market

**v1 launch goal: August 2026**

---

## Phase 2 — v2 Business & Community
**Target: Q4 2026**

Only after v1 is live and stable with real user feedback.
Phase 2 is business-oriented — features that grow the community and generate revenue.

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
