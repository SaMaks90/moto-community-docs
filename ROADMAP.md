# Roadmap

> Solo development, part-time pace. Dates are targets, not deadlines.
> The goal is to ship something real, not something rushed.

---

## Done

### Backend MVP — April 2026
- [x] Project setup (TypeScript, Express, Swagger, ESLint, Jest)
- [x] PostgreSQL schema — users, tokens, rides, ride_participants
- [x] Authentication — register, login, logout, refresh tokens, email verification
- [x] User profile — CRUD, avatar upload (Cloudflare R2), location, bio, Instagram
- [x] Rides — create, update, join, leave, status management
- [x] Ride filters — geo-radius (earthdistance), status, date range, moto type, engine size
- [x] Pagination — page / limit on rides list
- [x] Capacity check — enforce max_participants on join
- [x] Metrics endpoint (Prometheus-style)
- [x] Health check
- [x] Test coverage (Jest)
- [x] Swagger / OpenAPI documentation

### iOS Foundation — April 2026
- [x] App architecture (SwiftUI, AppState, navigation)
- [x] Auth flow — login, register
- [x] User profile — view and edit
- [x] Settings — change password, resend verification, notification preferences
- [x] Networking layer — JWT auth, auto token refresh, error handling
- [x] Secure token storage (Keychain)
- [x] Localization framework

---

## Phase 1 — iOS Core Screens
**Target: April 14 – May 25, 2026 (6 weeks)**

The backend is ready. This phase is purely iOS — bringing the existing API to life on screen.

- [ ] Rides list screen — paginated, filterable by status / moto type / engine size
- [ ] Ride details screen — info, participants list, creator profile
- [ ] Create ride screen — form with all fields (location, date, filters, passenger seat option)
- [ ] Join / leave ride — with full / already joined error handling
- [ ] Map screen — MapKit, nearby rides as pins, tap to open details
- [ ] Local feed screen — nearby rides, activity, community events filtered by region
- [ ] Avatar upload — complete the existing UI integration
- [ ] My rides screen — rides created and joined by current user

---

## Phase 2 — v1 New Features
**Target: May 26 – June 29, 2026 (5 weeks)**

Features that make MotoCommunity different from a simple ride planner.

### Passenger Mode
- [ ] Backend — add `passenger` role to ride_participants, seat availability logic
- [ ] iOS — toggle "I have a free seat" when creating a ride
- [ ] iOS — "Join as passenger" option on ride details

### Rider Ratings
- [ ] Backend — ratings table, POST /rides/:id/rate endpoint, average rating on user profile
- [ ] iOS — post-ride rating prompt (shown after ride status → completed)
- [ ] iOS — rating display on user profile (stars + count)

### SOS Help
- [ ] Backend — sos_requests table, POST /sos, GET /sos/nearby, resolve endpoint
- [ ] iOS — SOS button (persistent, accessible from map screen)
- [ ] iOS — nearby SOS alerts for riders within radius
- [ ] iOS — SOS history on profile

---

## Phase 3 — Push Notifications + Polish
**Target: June 30 – July 20, 2026 (3 weeks)**

Push notifications are required for SOS to work — without them, a rider with the app closed never receives an emergency alert. This phase completes the v1 feature set and prepares for launch.

### Prerequisites
- [ ] **Purchase Apple Developer Program** — $99/year, required for APNs, TestFlight, and App Store

### Push Notifications
- [ ] Backend — APNs integration, device token storage, notification service
- [ ] iOS — register device token on login, handle foreground/background/terminated state
- [ ] Notification types: SOS alert nearby, someone joined your ride, ride status changed, new ride nearby
- [ ] Notification preferences — per-type on/off (UI already exists in Settings)

### Localization
- [ ] Polish translation — coordinate with native speaker from the community
- [ ] German translation — coordinate with native speaker
- [ ] QA all 4 languages (EN, UK, PL, DE) — layouts, text overflow, RTL check

### Polish
- [ ] Error states and empty states on all screens
- [ ] Loading skeletons / indicators where missing
- [ ] Offline handling — graceful errors on no connection
- [ ] Performance — image caching, pagination edge cases
- [ ] App icon, launch screen, onboarding flow

---

## Phase 4 — Beta & App Store Launch
**Target: July 21 – August 4, 2026 (2 weeks)**

- [ ] Beta test with 10–20 real riders via TestFlight
- [ ] Bug fixing from beta feedback
- [ ] App Store Connect setup — screenshots, description, keywords, age rating
- [ ] Submit for App Store review (allow ~7 days for Apple review)
- [ ] Launch in Ukraine as first market
- [ ] Basic analytics setup (track: registrations, rides created, SOS activations)

**Soft launch goal: August 2026**

---

## Phase 5 — v2 Community Features
**Target: Q4 2026**

Only after v1 is live and stable with real user feedback.

- [ ] Clubs — create/join private groups, member roles, club rides
- [ ] STO Catalog — service station directory with ratings and reviews
- [ ] Local business announcements — geo-targeted posts from shops, events, brands
- [ ] Chat — direct messages between riders
- [ ] Android app — find a contributor or co-developer

---

## Phase 7 — v3 Platform
**Target: 2027**

- [ ] Bike History — service and ownership records by plate number
- [ ] Marketplace — buy/sell gear and motorcycles
- [ ] Web dashboard — for club organizers and STO owners

---

## Notes

- Phases 1 and 2 can overlap if backend changes for Phase 2 are small
- SOS is the most complex feature — ship a basic version first, iterate later
- Push notifications are blocked on Apple Developer account — plan the $99 purchase before Phase 4
- Android is not planned for solo development — looking for a contributor or co-founder
- App Store review can take 1–14 days — submit early, don't wait until the last moment
