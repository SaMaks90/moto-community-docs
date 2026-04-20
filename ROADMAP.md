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
- [x] My rides screen — rides created and joined by the current user
- [x] Join / leave ride — with capacity check and error handling
- [x] New shared components — RideStatusBadge, ChipSelector, FilterButton, EmptyStateView

---

## Phase 1 — v1 App Store Release
**Target: May – July 2026**

Everything needed for a complete, shippable v1. Phase 1 ends when the app is in the App Store.
The goal: a product people actually want to use — a closed core loop from finding a ride to building trust in the community.

### Core Loop
```
Find a ride → Join → Ride together → Rate participants → Add to Riding Circle → See their next ride first
```

### iOS Screens
- [ ] Ride details screen — info, participants list, creator profile with Trust Score
- [ ] Map screen — MapKit, nearby rides as pins, tap to open ride details
- [ ] News tab — "Coming Soon" placeholder (full feed planned for v2)

### Rider Languages
*For emigrants and travelers who want to ride but face a language barrier.*
- [ ] Backend — languages field on user profile, accepted_languages on rides
- [ ] iOS — language selector on profile (e.g. 🇺🇦 Ukrainian · 🇬🇧 English)
- [ ] iOS — language tags visible on ride card and ride details
- [ ] iOS — "Languages for this ride" field in create/edit ride (optional, empty = all welcome)
- [ ] iOS — filter rides by language in rides list

### Trust & Riding Circle
- [ ] Backend — ratings table, POST /rides/:id/rate, average rating on profile
- [ ] Backend — riding_circle table, add/remove riders, circle count on profile
- [ ] iOS — post-ride rating screen (appears after ride status → completed)
- [ ] iOS — "Add to Riding Circle" suggestion after giving 4–5 ⭐
- [ ] iOS — Trust Score on profile (avg rating + rides count + circle count)
- [ ] iOS — Riding Circle list on profile
- [ ] iOS — Feed priority — rides from Circle shown first

### Push Notifications
- [ ] **Purchase Apple Developer Program** — $99/year, required for APNs, TestFlight, App Store
- [ ] Backend — APNs integration, device token storage, notification service
- [ ] iOS — register device token on login, handle foreground/background/terminated state
- [ ] Notification types: someone joined your ride, ride status changed, new ride from Circle member
- [ ] Notification preferences — per-type on/off (UI already exists in Settings)

### Polish & Launch Prep
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
