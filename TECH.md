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

**No third-party networking library** — pure `URLSession`. Keeps dependencies minimal for a project at this stage.

---

## Infrastructure

| Service | Purpose | Provider |
|---------|---------|---------|
| **Database** | PostgreSQL (serverless) | Neon |
| **Backend hosting** | API server | Railway |
| **File storage** | Avatars and media (S3-compatible) | Cloudflare R2 |
| **Email delivery** | Verification and transactional emails | Resend |

### Why these services

**Neon** — serverless PostgreSQL with a generous free tier, branching for dev/prod environments, and scale-to-zero for cost efficiency early on.

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

## Future Infrastructure Considerations

| Need | Option |
|------|--------|
| Push notifications | APNs (Apple) — requires Apple Developer Program ($99/year) |
| Android | React Native or native Kotlin — to be decided |
| Real-time (SOS alerts) | WebSockets or Server-Sent Events on the existing Express server |
| Background jobs | Simple cron via Railway or a lightweight queue (BullMQ) |
