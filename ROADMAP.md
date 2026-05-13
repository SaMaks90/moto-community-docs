# Roadmap

> Solo development, part-time pace. Dates are targets, not deadlines.
> The goal is to ship something real, not something rushed.

**Phase = release version.** Phase 1 ends with v1 in the App Store. Phase 2 is a major update focused on business and community growth.

---

## Phase 1 — v1 App Store Release
**Target: April – June 2026**

Everything needed for a complete, shippable v1. Phase 1 ends when the app is in the App Store.
The goal: a product people actually want to use — a closed core loop from finding a ride to building trust in the community.

### Core Loop
```
Find a ride → Join → Ride together → Rate the ride → Add creator to Riding Circle → See their next ride first
```

### Backend Foundation
- [x] Project setup (TypeScript, Express, Swagger, ESLint, Jest)
- [x] PostgreSQL schema — users, tokens, rides, ride_participants
- [x] Authentication — register, login, logout, refresh tokens, email verification
- [x] User profile — CRUD, avatar upload (Cloudflare R2), location, bio, Instagram
- [x] Bug fix: avatar now deleted from Cloudflare R2 on user account deletion
- [x] Rides — create, update, join, leave, status management
- [x] Cron job — auto-activate rides when `date_time` is reached (`planned` → `active`)
- [x] Ride filters — geo-radius (earth distance), status, date range, moto type, engine size
- [x] Pagination — page / limit on a ride list
- [x] Capacity check — enforce max_participants on join
- [x] Metrics endpoint (Prometheus-style)
- [x] Health check
- [x] Test coverage (Jest)
- [x] Swagger / OpenAPI documentation
- [x] `pino` structured JSON logging (request method/path/status/duration, errors with stack traces)
- [x] Replace all `console.log` / `console.error` with pino logger instance

### iOS Foundation
- [x] App architecture (SwiftUI, AppState, navigation)
- [x] Auth flow — login, register, email verification
- [x] User profile — view, edit (username, location, Instagram), avatar upload
- [x] Settings — change password, resend verification, notification preferences, logout
- [x] Networking layer — JWT auth, auto token refresh, error handling
- [x] Secure token storage (Keychain)
- [x] Localization framework (EN, UK, PL, DE)
- [x] Location service
- [x] Bug fix: `NSLocationWhenInUseUsageDescription` missing from Info.plist — "My location" button now works in EditProfile
- [x] Location search switched to `MKLocalSearchCompleter` — shows up to 4 autocomplete suggestions instead of 1 (EditProfile, CreateRide, EditRide)
- [x] Removed "My Location" button from CreateRide and EditRide location fields

### iOS Core Screens
**Backend**
- [x] `GET /rides/:ride_id/participants` — participants with user info (username, avatar, bio)
- [x] Status transition validation — blocked: cannot activate a ride with future `date_time`; cannot complete a ride that hasn't started

**iOS**
- [x] Rides list screen — paginated feed with RideCardView, filterable by status / moto type / engine size
- [x] Create ride screen — full form (location, date, moto type, engine size, passenger seat)
- [x] Edit ride screen — edit fields and change ride status (with allowed transition logic)
- [x] My rides screen — rides created and joined by the current user; refreshes on navigation back
- [x] Join / leave ride — with capacity check and error handling
- [x] Ride detail screen — info, participants list with avatars, creator card (Trust Score placeholder), navigate to public profile
- [x] Public profile screen — read-only view with avatar, username, location, stats (Trust/Rides as placeholders), bio, Instagram link
- [x] Map screen — MapKit with iOS 17 API, nearby rides as color-coded pins, tap to select with preview card, filter sheet, locate button; shows the nearest 100 rides sorted by geo distance with a limit hint when at capacity
- [x] News tab — "Coming Soon" placeholder (full feed planned for v2)
- [x] Ride card status badge — "In progress" for active rides, countdown for planned/created, hidden for canceled/completed
- [x] ~~Follow system~~ — removed; replaced by Trust & Riding Circle (earned trust through real rides)
- [x] New shared components — RideStatusBadge, ChipSelector, FilterButton, EmptyStateView

### Security
**Backend**
- [x] IDOR fix on rides, rate limiting (helmet, express-rate-limit), CORS locked, avatar upload size limit, refresh token hashing, password complexity rules, coordinate validation

**iOS**
- [x] Certificate pinning, tokens to Keychain, password complexity, memory hygiene, screenshot prevention, debug log sanitization

### Forgot Password
**Backend**
- [x] `POST /auth/forgot-password` — send a reset link via email (hashed token, 1h expiry)
- [x] `POST /auth/reset-password` — validate token, set new password, invalidate all refresh tokens

**iOS**
- [x] "Forgot password?" link on login screen → enter email screen
- [x] Deep link handler for reset URL from email → new password screen

### Trust & Riding Circle
**Backend**
- [x] ride_reviews table, POST /rides/:id/rate, avg_rating on profile (one rating per ride → trust score of the creator)
- [x] riding_circle table, add/remove riders, circle_count on profile
- [x] GET /rides/pending-reviews, POST /rides/:id/dismiss-review (one-time prompt, server-side)
- [x] rides_count on profile (completed rides only)
- [x] Leave blocked for completed rides
- [x] Feed priority: Riding Circle creators are shown first in GET /rides
- [x] "From my Riding Circle" filter — `from_circle` query param on `GET /rides` (EXISTS subquery on `riding_circle`); iOS filter sheet toggle (`AppToggleRow`); Swagger docs; integration tests
- [x] Creator blocked from rating their own ride (400)
- [x] pending-reviews exclude rides the user created

**iOS**
- [x] Post-ride rating screen (one rating for the ride, not per participant; one-time prompt on app open)
- [x] Pre-fills existing rating when re-opening the review screen
- [x] "Add creator to Riding Circle" suggestion after giving 4–5 ⭐
- [x] Creator cannot see or submit the rate button for their own ride
- [x] Completed and canceled rides open in detail view, not edit view
- [x] Trust Score on profile (avg_rating + rides_count + circle_count); star icon always visible next to rating; defaults to `★ 0.0` when no ratings yet
- [x] Riding Circle list on the profile — trash button with a confirmation alert; swipe-to-remove also requires confirmation
- [x] Add / Remove from Riding Circle on any public profile (button hidden on own profile)
- [x] circle_count updates live in the ProfileView immediately after add/remove without a reload
- [x] ProfileView auto-refresh on appear — `UserManager.refresh()` fetches fresh profile from backend each time the Profile tab opens; ensures avg_rating, rides_count, circle_count are always up to date without restarting the app
- [x] Bug fix: the creator no longer sees the "Join" button on canceled or completed rides
- [x] Riding Circle navigation fix: NavigationLink now navigates on the first tap (switched from value-based to destination-based)
- [x] My Rides sort: active/planned rides first (ascending by date), canceled/completed last (descending)
- [x] MapSnapshotView: two-phase loading — pins appear immediately, then real driving route via `MKDirections` draws as blue polyline; large-distance region fitting fixed (1.7× span); landscape snapshot size (800×440) prevents pin clipping; pins have white halo for visibility on any map background
- [x] Ride detail start location: tappable label opens Apple Maps with driving directions
- [x] Ride detail location picker: tapping Start Location or End Location (if set) shows a custom centered popup (replaces `confirmationDialog` that anchored to toolbar) — Apple Maps / Google Maps / Waze; Google Maps falls back to web, Waze falls back to App Store; `comgooglemaps` and `waze` schemes added to `LSApplicationQueriesSchemes` in Info.plist

### Rider Languages
*For emigrants and travelers who want to ride but face a language barrier.*

**Backend**
- [x] `languages` field on user profile; rides return `creator_languages` via JOIN (no separate field on rides — language comes from the creator's profile)
- [x] `GET /rides` language filter: `u.languages && $n::text[]`

**iOS**
- [x] `LanguagePickerField` — full-screen sheet with search, checkmarks, localized names sorted A→Z, collapsed pill row when closed
- [x] Language selector on the profile edit (e.g., 🇺🇦 Ukrainian · 🇬🇧 English)
- [x] Language flags visible on ride card, map preview card, ride detail organizer card, and public profile
- [x] Filter rides by language in the ride list
- [x] Languages section in the Public Profile view (flag and name chips)

### Auth Enhancements
**Backend**
- [x] Google OAuth endpoint (`POST /auth/google`, google-auth-library, find-or-create user)
- [x] Sign in with Apple endpoint (`POST /auth/apple`, apple-signin-auth, find-or-create user)
- [x] Biometric token architecture — `is_biometric` column on `tokens` table, `POST /auth/biometric-token` endpoint, `createBiometricToken` (365-day expiry), `deleteAllSessionTokens` (preserves biometric token on login/logout)
- [x] `login()` returns full user via `getUserById` (includes trust score, avg_rating, rides_count, circle_count)
- [x] Test suite updated — auth service/controller tests cover biometric token flow; token repository integration tests cover `createBiometricToken` and `deleteAllSessionTokens`

**iOS**
- [x] Face ID / Touch ID login (LocalAuthentication + Keychain, biometric token stored in a separate Keychain key, persists across logout)
- [x] Sign in with Google (GIDSignIn SDK, custom button, Google Cloud Console configured)
- [x] Sign in with Apple (code completes — capability requires an Apple Developer account to activate in Xcode)

### Localization Expansion (RU/BE)
*Russian and Belarusian interface — emigrants and cross-border riders.*

**iOS**
- [x] Verify that RU and BE are present in `Language.all` (`Language.swift`) — RU was already there, BE (🇧🇾) added
- [x] Add `ru.lproj/Localizable.strings` — Russian interface translation
- [x] Add `be.lproj/Localizable.strings` — Belarusian interface translation
- [x] Update `LanguageSelector` — add RU and BE as interface language options

### Ride Chat
*Chat opens with the ride, closes when the ride is completed or canceled.*

**Backend**
- [x] `ride_messages` table (ride_id, user_id, text, created_at); only participants and organizer can write
- [x] WebSocket gateway (Socket.IO) — `join_room`, `send_message`, `new_message` broadcast; sending only for `planned` / `active` status
- [x] `GET /rides/:id/messages` — message list with pagination (participants only)
- [x] `GET /rides/:id/messages/last` — last message for unread indicator on iOS
- [x] Bug fix: `rideChatRoute` mounted on `/api/rides/:ride_id/messages` (previously 404 on all chat endpoints)

**iOS**
- [x] `RideChatView` — real-time chat via Socket.IO (socket.io-client-swift); message bubbles, auto-scroll, input field with Send button
- [x] `RideChatViewModel` — Socket.IO connection, `join_room` on open, `send_message`, `new_message` listener
- [x] Chat button hidden when status is `completed` or `cancelled`; button in toolbar (next to Edit); orange dot if there are unread messages
- [x] `ChatReadTracker` — stores `lastReadAt` in UserDefaults per rideId; dot disappears after opening chat
- [x] `AppEnvironment` — centralized environment config (`.local` / `.preProduction` / `.production`); single place for API URL, Socket URL, SSL pinning hashes
- [x] Unread badge (`bubble.left.fill` orange) on `RideCardView` — visible in My Rides and Rides List without entering the ride; `MyRidesViewModel` and `RideListViewModel` fetch `getLastMessage` in parallel for `planned`/`active` rides
- [x] Map: creator pin — gray with "My" capsule; joined pin — green with "Joined" capsule; others — as before by status
- [x] Map preview card: "Created by me" label (gray) or "Joined" (green) + unread bubble indicator
- [x] Bug fix: tapping on empty map area now dismisses the ride preview card
- [x] Localization: new key `"my"` in all 6 languages; fixed `"created_by_you"` → "Створена мною" (the UK)
- [x] Push notification type: new message in chat
- [x] **Chat Inbox** — global chat icon button (top-right overlay in `MainTabView`) with orange unread badge dot; opens `ChatInboxView` — list of all active chats (planned/active rides where user is participant or creator), sorted unread-first; each row shows ride title, last message preview (`username: text`), unread dot; tap → `RideChatView`; reloads on app foreground and on chat close; `ChatInboxViewModel.shared` singleton; keys `"messages"` / `"no_active_chats"` in all 6 languages

### Push Notifications
- [x] **Purchase Apple Developer Program** — $99/year, required for APNs, TestFlight, App Store

**Backend**
- [x] APNs integration (.p8 Auth Key via `APNS_PRIVATE_KEY` env var), device token storage, notification service
- [x] Structured logging for APNs: logs `environment` on init, `device_token` (last 8 chars) on success and failure
- [x] Global `notifications_enabled` toggle — added to `INotificationPreferences`, Zod schema, and `initDb` default JSONB; backend SQL uses `IS NOT FALSE` so NULL = enabled
- [x] Notification type `ride_started` — cron job sends to all participants when planned ride auto-activates; bypasses per-type toggle (only respects global)
- [x] Cron job `remindActiveRides` — runs hourly; sends `organizer_ride_reminder` to ride creator if ride is still `active` 4h+ after last status change
- [x] Notification type `rate_organizer` — sent to participants on `completed` status change instead of `ride_status_changed`; translations in all 6 languages

**iOS**
- [x] `AppDelegate` via `@UIApplicationDelegateAdaptor`, register device token on login, send to backend
- [x] Notification types: someone joined your ride, ride status changed, new chat message, new ride from Circle member (backend sends `ride_id` in payload)
- [x] Notification preferences — per-type on/off (UI + backend)
- [x] `UNUserNotificationCenterDelegate` — foreground banner display + navigate to ride screen on tap (by `ride_id` from payload); chat notifications open `RideChatView` directly, others fetch and open `RideDetailView`
- [x] Tap on avatar in chat → opens `PublicProfileView`
- [x] Tested and working on TestFlight (pre-production backend, sandbox APNs)
- [x] Global notifications toggle wired to backend — `NotificationSettingsViewModel` sends `notifications_enabled: true/false` directly; `isEnabled` reads from backend response (removed UserDefaults + disable-all-prefs workaround)
- [x] New notification types `ride_started` and `organizer_ride_reminder` route to ride detail on tap (handled automatically by existing `NotificationRouter` / `MainTabView` logic)
- [x] Notification type `rate_organizer` — sent to all participants when organizer sets status to `completed`; tap opens `PostRideReviewView` directly via `reviewVM.loadForRide`; `ride_status_changed` no longer sent for `completed` (covered by this) or `active` (covered by `ride_started` cron)
- [x] Bug fix: cold start deep link — `new_ride_from_circle` (and all notification types) now correctly navigate on tap when app was killed; added `.task` in `MainTabView` to handle `router.pending` already set before the view appeared; extracted `handle(_:)` method to eliminate duplicate logic

### Website
*Next.js landing — `moto-community-web/`, separate GitHub repo, deployed on Railway. Required before App Store submission (Privacy Policy URL).*

- [x] Setup — Next.js 16 project in `moto-community-web/`, ready to push to GitHub
- [x] Single-page landing — multilingual (EN, UK, PL, DE, RU, BE)
- [x] Privacy Policy page — public URL for App Store Connect
- [x] i18n routing — URL subfolders (`/en/`, `/uk/`, `/ru/` etc.), locale detection via Accept-Language
- [x] `hreflang` tags + `x-default` — tells Google which language version for which region
- [x] `generateMetadata` — unique `<title>` and `<meta description>` per language
- [x] Open Graph tags and OG Image (dynamic `opengraph-image.tsx`, 1200×630) — previews in social media and messengers
- [x] Twitter Card (`summary_large_image`)
- [x] JSON-LD structured data — `MobileApplication` schema → rich results in Google
- [x] `sitemap.xml` — all language versions of all pages, auto-generated via `next-sitemap`
- [x] `robots.txt` — allow indexing of all public pages
- [x] Semantic markup — `<header>`, `<main>`, `<nav>`, `<section>`, `<footer>`, `<h1>`, `<h2>`
- [x] Cloudflare Web Analytics — beacon token obtained, `NEXT_PUBLIC_CF_BEACON_TOKEN` added to Railway env
- [x] Deploy — GitHub repo created, connected to Railway, `NEXT_PUBLIC_SITE_URL=https://motocommunity.app`, domain `motocommunity.app` purchased and connected
- [x] App Screenshots section between Features and DownloadCTA
  - Screenshots taken on real device (iPhone 14 Pro): Rides List, Map, Ride Detail, My Rides, Profile, Chat
  - [x] `Screenshots.tsx` component — horizontal scroll with 6 screenshots
  - [x] All 6 language `json` files updated with `screenshots` key

**iOS**
- [x] After site publication: add Privacy Policy link in `AboutAppView` (new "Legal" section, opens URL in Safari); add `privacy_policy` key to all six localization files

### Telegram Bot
- [x] `@motocommunity_bot` — support bot deployed (Telegraf.js, Railway service)
- [x] `@motocommunityapp` — announcements channel

### Infrastructure & DevOps
- [x] CI/CD pre-production — GitHub Actions (lint → test → docker build → Railway deploy → Telegram notify)
- [x] Production environment on Railway (separate environment, separate Neon DB)
- [x] GitHub Environments: production environment created, secrets added (DOCKERHUB_USERNAME, DOCKERHUB_TOKEN, TELEGRAM_BOT_TOKEN, TELEGRAM_CHAT_ID)
- [x] CI/CD production — workflow triggered on git tag `v*.*.*` → docker build → push `:production` + `:vX.X.X` → Railway auto-deploy → Telegram notify
- [x] Certificate Pinning — Railway wildcard hash (`*.up.railway.app`) covers both environments; `motocommunity.app` — landing, iOS doesn't connect to it
- [ ] GitHub Releases — manually after each deployment: GitHub → Releases → Draft → select tag → write notes → Publish

### Additional Features

**Organizer UX**
- [x] Active Ride Banner — floating top card in `MainTabView` reminding the organizer to update status when ride is active; appears 4h after ride goes active; swipe-up to dismiss (session-only); reappears on every app open; hides when app is backgrounded and re-appears with animation on return; "Complete" button with confirmation dialog; works across all tabs
- [x] My Rides draft banner — rides with `status = created` show an inline "Not visible to others" strip under the card with a "Publish" button; tapping immediately changes status to `planned`; success strip ("Ride published!" ✓) appears with fade animation for 2 seconds, then disappears; all 6 languages
- [x] Edit Ride status actions — replaced badge-style transition buttons with explicit action buttons: "Publish Ride" (orange, `created`), "Cancel Ride" (red, `planned`), "Complete Ride" (green, `active`); current status badge stays in the header; `created` shows `eye.slash` hint "People will see this ride once you publish it"; `planned` shows `clock` hint "Will activate automatically at the scheduled time"; confirmation alert preserved for all actions; all 6 languages
- [x] Draft expired-date guard — if a `created` ride's start date has passed: "Publish Ride" button turns dimmed, inline `exclamationmark.triangle` red hint appears, tapping shows an alert "Date has passed / Update the start date before publishing"; countdown badge removed from draft ride cards entirely (`RideCardView`); all 6 languages
- [x] Ride time conflict error — localized error message (`error_ride_time_conflict`) when joining or creating a ride that conflicts with an existing one within ±2 hours; all 6 languages
- [x] 15-minute lead time guard — client-side check in `CreateRideViewModel`, `EditRideViewModel.save()`, and `EditRideViewModel.changeStatus()` before network call; server-side 422 also mapped to localized `error_ride_too_soon`; all 6 languages

**Backend**
- [x] `APNS_PRODUCTION` env var — controls APNs sandbox vs production endpoint independently of `NODE_ENV`; pre-production → sandbox, production → production
- [x] Rate limit: `100 → 300` req / 15 min per IP; added `warn` log when limit is exceeded (IP + path)
- [x] Ride time conflict guard — `join` and `create` reject with 409 if user already has a `planned` or `active` ride within ±2 hours of the target `date_time`
- [x] 15-minute lead time guard — publishing a ride (`status → planned`) rejected with 422 if `date_time` is less than 15 minutes from now; applies to `createRide` (when `status = planned`), `changeRideStatus` (`created → planned`), and `updateRide` (when changing `date_time` on a `planned` ride)

### Launch Prep
- [x] Password complexity hints on register/change-password screens — real-time `PasswordHintsView` (four pill indicators: 8+, A – Z, 0–9,@#)
- [x] Error states and empty states on all screens — RideListView, RidingCircleView (Wi-Fi.slash + retry), MapView (error/locationDenied/empty/limit banners), RideDetail (no participants yet)
- [x] Loading skeletons / indicators — ProfileView location skeleton, avatar upload overlay with ProgressView
- [x] Offline handling — `NetworkClient` catches `.notConnectedToInternet` / `.networkConnectionLost`, surfaces readable error to user
- [x] Performance — image caching (`ImageCache` NSCache singleton, `AvatarView` with `.task(id:)`), pagination edge case fix (`hasMorePages` uses `total_pages` from response)
- [x] App icon, launch screen, onboarding flow — launch screen (black bg + logo), `OnboardingView` (three slides, shown once via `@AppStorage`), `AboutAppView` with "View App Tour" button
- [x] QA all four languages (EN, UK, PL, DE) — fixed missing `status_open` in UK/PL/DE; fixed typo "Not verifed" in EN
- [x] Support & Donate page in Settings — `SupportView` with: Telegram Channel (community news), Telegram Bot (support), Ko-fi / Patreon / PayPal (donation); opens Telegram app or fallback Safari; donation links localized in all 6 languages
- [x] Removed "For Developers" section from `SupportView` (GitHub Docs link — not relevant for end users)
- [x] Publish sheet after ride creation — after successful save, custom popup asks "Publish ride?" with "Publish" (→ PATCH status to `planned`) and "Save as Draft" buttons; hint at the bottom: "You can change the status in My Rides at any time."; all 6 languages
- [x] Ride reminder notification — push sent X minutes before ride starts to all participants + organizer; configurable in Notification Settings (Off / 15 min / 30 min / 1h / 2h, default 30 min); per-user preference stored as `ride_reminder_minutes` in JSONB
- [x] Bug fix: `getRidesForUpcomingReminder` — replaced broken `UNION` (referenced non-existent `status` column on `ride_participants`) with direct `JOIN ride_participants`; reminders were silently never sent
- [x] Reminder deduplication — `reminder_sent_at` column on `ride_participants`; reminder fires once per participant per ride regardless of cron timing; resets automatically when ride `date_time` is changed by organizer
- [x] Job optimization — merged three cron jobs (`activateRides` every 1 min, `remindUpcomingRides` every 5 min, `remindActiveRides` every hour) into one `combinedRides.job.ts` running every 10 min; Neon DB can now sleep between ticks reducing compute hours ~10× during low-traffic periods
- [x] Bug fix: `organizer_ride_reminder` was re-sent every hour indefinitely after 4h mark; fixed by using `UPDATE ... RETURNING` which resets `updated_at` on each send, enforcing a real 4h cooldown
- [x] Bug fix: My Rides not refreshing after ride creation — `CreateView` now posts `rideCreated` notification on success; `MyRidesView` observes it, switches to "Created" tab and reloads immediately
- [x] UI fix: filter sheet color consistency — `AppToggleStyle` and `ChipSelector` now use `Color.brandOrange` instead of system `Color.orange` (which rendered as a darker brownish shade in dark mode); toggle OFF-state track opacity increased for visibility
- [ ] Beta test with 10–20 real riders via TestFlight
- [ ] Bug fixing from beta feedback
- [ ] App Store Connect setup:
  - [ ] Screenshots — iPhone 6.9" (1320×2868 px), minimum 3–5 screens (taken in simulator or on device)
  - [x] App description, subtitle, keywords — in four languages: EN, UK, PL, DE → `moto-community-docs/APP_STORE.md`
  - [x] Privacy Policy — `https://motocommunity.app/en/privacy`
  - [ ] Category: Social Networking (or Sports)
  - [ ] Age rating: fill out Apple questionnaire (usually 12+)
  - [ ] Developer info — taken from an Apple Developer account automatically
- [ ] Submit for App Store review
- [ ] Launch in Ukraine as first market

**v1 launch goal: August 2026**

---

## Phase 2 — v2 New Event Types
**Target: July – August 2026**

Only after v1 is live and stable with real user feedback.
Phase 2 introduces two new event types and a local feed — all building on the existing rides' infrastructure.

### In-App Text Translation
*For emigrants and travelers — select any text in the app (ride description, chat message, bio) and translate it on the spot.*

**iOS**
- [ ] Long-press context menu on ride description, chat messages, and user bio — "Translate" action via iOS `UIReferenceLibraryViewController` or `NLLanguageRecognizer` + Apple Translate framework
- [ ] Auto-detect source language; translate to the device's current interface language

---

### Moto Gymkhana
*A new event type (`ride_type = gymkhana`) — precision riding competition with cones. Organizer sets up the course, riders register and compete.*

**Backend**
- [ ] `ride_type = 'gymkhana'` — extended event fields: course description, skill level, equipment requirements
- [ ] Result endpoint: POST /rides/:id/results — organizer submits times/scores per participant after the event
- [ ] Leaderboard: GET /rides/:id/leaderboard — ranked results visible to all participants
- [ ] Push notification type: new gymkhana event nearby

**iOS**
- [ ] Gymkhana event creation — extended form (course info, skill level, requirements)
- [ ] Distinct card style in rides list and map pins
- [ ] Result screen — organizer inputs times after the event; participants see a ranked leaderboard

### SOS Help
*A new event type (`ride_type = 'sos'`) — roadside help request visible to nearby riders. Not a panic button — a structured call for help.*

**Backend**
- [ ] `ride_type = 'sos'` — location, description, help type (flat tire, out of fuel, mechanical, other); visible only within geo-radius
- [ ] Auto-resolve after 2h or manual resolve by creator
- [ ] Push notification type: SOS request from nearby rider

**iOS**
- [ ] Quick SOS creation from the map screen (location pre-filled, minimal form)
- [ ] Nearby SOS visible on the map — distinct pin with help type icon
- [ ] "I'm on my way" response (notifies the creator); resolve button for creator
- [ ] SOS history on profile

### Local Feed
*Replaces the "Coming Soon" News tab placeholder.*

**Backend**
- [ ] Feed algorithm: nearby rides + Riding Circle activity + gymkhana events + community highlights
- [ ] Feed endpoint with geo-filter and pagination

**iOS**
- [ ] Local feed screen replacing the "Coming Soon" placeholder
- [ ] Activity cards: new rides nearby, rides from Circle, gymkhana events, SOS resolved nearby

---

## Phase 3 — v3 Business & Monetization
**Target: August – December 2026**

B2B layer on top of the rider community. Businesses pay to be visible to nearby riders.

### Business Account
- [ ] Business account type — verified badge, separate onboarding flow
- [ ] Business profile page — photos, description, contact info, working hours, location
- [ ] Account types: STO (service station), Moto salon (dealership), Events organizer, Cross Tour operator
- [ ] Monthly / annual subscription for businesses

### Map Placements
*Paid pins are visible to all nearby riders on the map.*
- [ ] STO pin — workshop/service station with rating and contact
- [ ] Moto salon pin — dealership with brand and inventory info
- [ ] Event pin — one-off or recurring moto events
- [ ] Cross Tour pin — organized multi-day tours with a booking link

### Sponsored Content
- [ ] Sponsored rides — business logo visible on the ride card in the feed
- [ ] Geo-targeted announcements — posts visible to riders within a set radius
- [ ] Pay-per-placement for one-off events and announcements

### Analytics Dashboard
- [ ] Web Dashboard for business accounts — pin views, profile clicks, announcement reach
- [ ] Basic weekly / monthly stats

---

## Phase 4 — v4 Ride Experience
**Target: Q1 2027**

Deeper ride experience. Requires real user feedback from Phase 3 before committing to GPS infra.

### Ride Tracking
*Background GPS recording during an active ride.*

**iOS limitation:** force-quitting the app stops tracking — data for that session is lost. No workaround exists on iOS.

**Open design question — who gets tracked:**
- **Option A — organizer only:** one GPS stream per ride; Summary Card generated from organizer's data and shared with all participants. Risk: organizer force-quits → the entire ride has no data.
- **Option B — all participants (opt-in):** each rider tracks independently, each gets a personal Summary Card. More resilient, but no single "official" route for the ride.
- **Option C — organizer + opt-in for participants:** organizer provides the official route; participants who opt in get personal stats on top. Most flexible, most complex.

**Backend**
- [ ] `route_points` (JSONB array `{lat, lng, timestamp}`), `distance_km`, `avg_speed_kmh` columns on ride table
- [ ] Distance calculation (haversine between consecutive points) and avg speed on ride completion

**iOS**
- [ ] `CLLocationManager` background mode — `allowsBackgroundLocationUpdates`, background location entitlement
- [ ] `distanceFilter = 10m` + `kCLLocationAccuracyNearestTenMeters` — battery-optimized (~3–5% per hour)
- [ ] GPS noise filtering; store points locally (CoreData), sync to backend on ride completion
- [ ] Real-time distance and avg speed display during an active ride

### Ride Summary Card
**iOS**
- [ ] Branded visual card after ride completion — route map snapshot, distance, avg speed, duration, date, participants
- [ ] Share button → `UIActivityViewController` → save to Photos or share to Instagram Stories (via `instagram-stories` URL scheme)

---

## Phase N — Backlog
*Unordered ideas. Features move from here into a named phase when prioritized.*

### Rides
- [ ] Passenger Mode — "I have a free seat" toggle; join as a passenger option (relevant for the "want to ride" audience)

### Community
- [ ] Clubs — create/join private groups, member roles, club rides
- [ ] Club Chat — permanent group chat inside a club
- [ ] Direct Messages — rider-to-rider chat

### Platform
- [ ] Bike History — service and ownership records by plate number
- [ ] Marketplace — buy/sell gear and motorcycles
- [ ] Open API — public read-only API for event aggregators and navigation apps
- [ ] Android app — find a contributor or co-developer

---

## Notes

- Phase 1 = v1 App Store release. Nothing ships until the core loop is complete.
- Push notifications: backend uses `APNS_PRODUCTION` env var (not `NODE_ENV`) to control APNs endpoint; pre-production → sandbox, production → production APNs
- App Store review can take 1–14 days — submit early
- SOS and Moto Gymkhana are new event types in Phase 2 — they extend the existing rides infrastructure, not separate systems
- Local Feed replaces the "Coming Soon" News tab in Phase 2
- Ride Tracking (Phase 4): force-quitting the app loses the session — iOS limitation, no workaround. Who gets tracked (organizer only / all opt-in / hybrid) is an open design question.
- Phase N is an unordered backlog — features move from there into a named phase when prioritized
- Android is not planned for solo development — looking for a contributor or co-founder
