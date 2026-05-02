# Tech Stack

## Overview

MotoCommunity is built as a mobile-first product with a REST API backend and a native iOS client.
The stack is chosen for speed of development, low operational cost, and ease of scaling.

---

## Backend

| Technology | Purpose |
|-----------|---------|
| **Node.js + TypeScript** | Runtime and language |
| **Express** | HTTP framework |
| **PostgreSQL** | Primary database |
| **Zod** | Request validation and env schema |
| **JWT** | Access and refresh token authentication |
| **Multer** | File upload handling |
| **Swagger / OpenAPI** | API documentation at `/api-docs` |
| **Jest** | Unit and integration testing |
| **ESLint** | Code quality |

### Architecture

Feature-based module structure — each domain (auth, users, rides, etc.) contains its own controller, service, repository, types, validation schema, and tests.

```
modules/
├── auth/
├── users/
├── rides/
├── rideParticipants/
├── tokens/
├── emailVerificationTokens/
├── health/
└── metrics/
```

### Key Design Decisions

**No ORM** — raw SQL via `node-postgres`. Keeps queries explicit and performant, avoids ORM abstraction overhead for a small team.

**Zod for validation** — single source of truth for request shape and environment variables. TypeScript types are inferred directly from schemas.

**Geo queries** — PostgreSQL `earthdistance` extension for radius-based ride search. Efficient server-side filtering without pulling all records into memory.

**Soft delete not used** — hard deletes with `ON DELETE CASCADE`. Simplicity over auditability at this stage.

---

## iOS

| Technology | Purpose |
|-----------|---------|
| **Swift** | Language |
| **SwiftUI** | UI framework |
| **MapKit** | Map and location features |
| **Keychain** | Secure token storage |
| **URLSession** | HTTP networking |
| **LocalAuthentication** | Face ID / Touch ID biometric login |
| **GoogleSignIn SDK** | Sign in with Google |
| **AuthenticationServices** | Sign in with Apple |

### Architecture

MVVM pattern — Views, ViewModels, and Services are separated by feature.

```
Features/
├── Auth/
├── Profile/
├── Settings/
├── Rides/       ← in progress
└── Map/         ← in progress

Core/
├── Networking/  ← NetworkClient, DTOs, Services
└── Storage/     ← TokenManager, KeychainManager, UserManager
```

### Key Design Decisions

**Auto token refresh** — `NetworkClient` intercepts 401 responses, refreshes the access token, and retries the original request automatically.

**Offline detection** — `NetworkClient` catches `URLError.notConnectedToInternet` and `networkConnectionLost` before any server error handling, surfacing a readable message to the user.

**Biometric auth** — `BiometricAuthManager` wraps `LAContext.evaluatePolicy` in `withCheckedContinuation`. Biometric refresh token is stored in a separate Keychain key (`biometric_refresh_token`) — not cleared on regular logout, only on account deletion or when user disables the feature. Token is rotated after each biometric login.

**No third-party networking library** — pure `URLSession`. Keeps dependencies minimal for a project at this stage.

**Certificate pinning** — `PinningURLSessionDelegate` validates the server's public key hash (SHA-256 of SPKI) on every TLS handshake. Pinning is skipped in `#if DEBUG` so dev tools like Charles Proxy work. At least two hashes should be kept (leaf + backup) to survive cert rotation. Disabled in DEBUG builds.

---

## Infrastructure

| Service | Purpose | Provider |
|---------|---------|---------|
| **Database** | PostgreSQL (serverless) | Neon |
| **Backend hosting** | API server | Railway |
| **File storage** | Avatars and media (S3-compatible) | Cloudflare R2 |
| **Email delivery** | Verification and transactional emails | Resend |

### Neon Branching Strategy

One Neon project, two child branches — no separate project needed for production.

```
Neon Project: moto-community
  └── main              → Railway pre-production service
  └── production        → Railway production service (after App Store release)
```

**How it works:**
- `production` is a **child branch** created from `main` at release time
- Neon uses copy-on-write internally — branch creation is instant with no data copying
- After branching, each environment is fully independent: data changes do not propagate between branches
- Each branch has its own `DATABASE_URL` — set it in the corresponding Railway service env vars
- Migrations must be run separately per branch

**When to create the production branch:** right before the App Store release, once `main` is in a stable state.

---

### Why these services

**Neon** — serverless PostgreSQL with a generous free tier, child branching for environment isolation, and scale-to-zero for cost efficiency early on.

**Railway** — simple deployment from GitHub, environment variables UI, built-in metrics. No infrastructure config needed.

**Cloudflare R2** — S3-compatible storage with zero egress fees. Significant cost saving vs AWS S3 once traffic grows.

**Resend** — modern email API with high deliverability. Simple SDK, good developer experience.

---

## API Documentation

Interactive Swagger UI is available at:
```
GET /api-docs
```

Full endpoint reference, request/response schemas, and authentication flow documented there.

---

## Localization

The iOS app is built with localization support from the start.

| Language | Status | Notes |
|----------|--------|-------|
| English | In progress | Primary language, base for all translations |
| Ukrainian | In progress | Home market |
| Polish | Planned (before v1 launch) | Translation by a native speaker from the community |
| German | Planned (v1–v2) | Largest motorcycle market in Europe |

The localization framework (`Localization.swift`, `LocalizationManager.swift`) is already in place in the iOS app — new languages are added by extending the translations file, no architectural changes needed.

---

## Security

### Backend measures in place (as of April 2026)

| Area | Implementation |
|------|----------------|
| **Security headers** | `helmet` — sets X-Frame-Options, HSTS, CSP, X-Content-Type-Options |
| **Rate limiting** | `express-rate-limit` — 100 req/15min globally; 10 req/15min on login and register |
| **CORS** | `origin: false` — browser cross-origin requests blocked; mobile clients unaffected |
| **Auth — token storage** | Refresh tokens stored as SHA-256 hashes in DB (raw token never persisted) |
| **Auth — token expiry** | Access token: 15 min · Refresh token: 30 days (JWT and DB aligned) |
| **Auth — password** | bcrypt (10 rounds) · min 8 chars, uppercase, number, special character |
| **Authorization** | Ride update and status change verify `creator_id === userId` → 403 |
| **File upload** | Avatar limited to 5 MB · MIME whitelist: `image/jpeg`, `image/png`, `image/webp` |
| **Input validation** | Zod on all request body, params, and query — including coordinate range checks |
| **DB queries** | Parameterized queries throughout — no string interpolation |
| **Error responses** | Stack traces never exposed — generic 500 for unexpected errors |

### iOS measures in place (as of April 2026)

| Area | Implementation |
|------|----------------|
| **Certificate pinning** | `PinningURLSessionDelegate` — SHA-256 SPKI hash check on every TLS handshake; skipped in DEBUG |
| **Token storage** | Both access and refresh tokens stored in Keychain with `kSecAttrAccessibleWhenUnlockedThisDeviceOnly` |
| **Biometric token** | Separate `biometric_refresh_token` Keychain key — survives logout, cleared only on account deletion or feature toggle-off |
| **Password fields** | `SecureField` with `textContentType(.password)` and `privacySensitive()` — prevents copy and marks content as sensitive |
| **App Switcher privacy** | `.privacyScreen()` modifier on auth and password screens — overlays black when app is not active |
| **Password validation** | Complexity enforced client-side on register and change-password (mirrors backend rules); real-time `PasswordHintsView` |
| **Memory hygiene** | Passwords cleared from `@Published` properties immediately after successful submit |
| **Debug logging** | Only `[statusCode] /path` logged in DEBUG — no response bodies, no tokens, no PII |
| **Biometric auth** | Face ID / Touch ID via `LocalAuthentication`; `NSFaceIDUsageDescription` in Info.plist |

### Password requirements (backend + iOS)

- Minimum 8 characters
- At least one uppercase letter
- At least one number
- At least one special character

Rules are enforced on the backend (Zod schema) and mirrored in iOS UI validation (`RegisterViewModel`, `ChangePasswordViewModel`).

### Certificate pinning — operational notes

Before each production release, update `PinningURLSessionDelegate.pinnedHashes` with current hashes:

```bash
openssl s_client -connect <host>:443 </dev/null 2>/dev/null \
  | openssl x509 -pubkey -noout \
  | openssl pkey -pubin -outform DER \
  | openssl dgst -sha256 -binary \
  | base64
```

Keep the old hash alongside the new one during the transition window so existing app versions continue to work.

---

## Logging

### Strategy

Structured JSON logging via **`pino`** written to stdout. Every hosting platform (Railway, AWS, Fly.io, Render, VPS) reads stdout natively — no code changes needed when migrating infrastructure.

```
pino (JSON to stdout) → hosting platform stdout → [optional] external log sink
```

### Log levels

| Level | When |
|-------|------|
| `info` | Every request: method, path, status code, duration |
| `warn` | Non-critical issues: validation failures, token refresh attempts |
| `error` | Exceptions, unhandled rejections, DB errors (with stack trace) |

### External sink (optional, recommended before launch)

Railway supports **Log Drains** — forwards all stdout to an external service without touching code. Recommended: **Better Stack** (free tier: 1 GB/month, 30-day retention).

When migrating hosting: reconnect the Log Drain to the new server. Log history stays in Better Stack. Zero code changes.

---

## Scalability Plan

### Current baseline (Phase 1, ~1 000 users)

| Layer | Current setup | Limit |
|-------|--------------|-------|
| Backend | Railway (single instance, Node.js) | ~500 concurrent requests |
| Database | Neon serverless PostgreSQL | ~100 concurrent connections |
| Storage | Cloudflare R2 | Practically unlimited |
| Email | Resend | 100 emails/day (free tier) |

### When to scale and how (no rewrites needed)

**~5 000 users — minor changes:**
- Railway: upgrade plan or add a second instance (Railway supports horizontal scaling with a toggle)
- Neon: upgrade to a paid plan for more compute and connections
- Resend: upgrade to paid plan ($20/month for 50 000 emails)

**~20 000 users — infrastructure changes, no code rewrite:**
- Migrate backend from Railway to **AWS ECS / Fly.io** — same Docker container, new hosting. `pino` logs continue flowing to Better Stack via Log Drain
- Add a **connection pooler** in front of Neon (PgBouncer or Neon's built-in pooling)
- Add **Redis** for session caching and rate limiting state (replacing in-memory `express-rate-limit`)

**~100 000+ users — architectural changes:**
- Horizontal scaling behind a load balancer (Nginx / AWS ALB)
- Read replicas for DB-heavy queries (ride list, map feed)
- CDN in front of Cloudflare R2 for avatars (already on Cloudflare network, minimal work)
- Consider splitting into microservices if team grows — but module structure is already feature-separated

### Key decisions already made for scalability

- **Stateless JWT auth** — any instance can validate any token, no sticky sessions needed
- **Feature-based module structure** — easy to extract a module into a separate service later
- **pino stdout logging** — hosting-agnostic, works on any platform
- **Cloudflare R2** — zero egress fees, no vendor lock-in vs AWS S3
- **Parameterized SQL** — no ORM means query optimization is explicit and straightforward

---

## Community Infrastructure

| Service | Platform | Details |
|---------|----------|---------|
| Telegram Channel | Telegram | `@motocommunityapp` — анонси, новини |
| Telegram Bot | Railway (окремий service) | `@motocommunity_bot` — feedback від користувачів |
| Donations | Ko-fi | `https://ko-fi.com/samchenkoms` |

### Telegram Feedback Bot

- **Репо:** `moto-community-telegram-bot` — окремий GitHub репо
- **Stack:** Node.js + TypeScript + Telegraf.js
- **Deploy:** Railway — окремий service, `npm run build && npm start`
- **Session:** in-memory `Map` — достатньо для single-instance, без Redis
- **Flow:** `/start` → категорія → повідомлення → форвард у приватну групу → підтвердження юзеру

---

## Future Infrastructure Considerations

| Need | Option |
|------|--------|
| Push notifications | APNs (Apple) — requires Apple Developer Program ($99/year) |
| Android | React Native or native Kotlin — to be decided |
| Real-time (SOS alerts) | WebSockets or Server-Sent Events on the existing Express server |
| Background jobs | Simple cron via Railway or a lightweight queue (BullMQ) |
| Log aggregation | Better Stack via Railway Log Drain (no code changes needed) |
