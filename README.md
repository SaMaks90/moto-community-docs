# MotoCommunity

> Find riders nearby. Plan rides together. Stay safe on the road.

MotoCommunity is a mobile app for motorcycle riders — connecting people who want to ride together, organizing spontaneous and planned trips, and providing a real-time SOS system for emergencies on the road.

---

## The Problem

You want to ride this evening. Your usual group is busy.
You just moved to a new city and don't know any local riders.
You're 80 km from home with a flat tire and no one around.
You've always wanted to feel what it's like to ride a motorcycle — but don't have one.

Existing apps help you **plan a route** or **track where you rode**.
None of them help you find who's riding **right now, near you**.

---

## The Solution

MotoCommunity connects riders in real time:

- **Spontaneous rides** — see who's planning a ride nearby tonight or this weekend, join in one tap
- **Organized events** — create rides with a defined route, time, and requirements (bike type, engine size)
- **Passenger seat** — riders can offer a free seat; anyone who wants to experience riding can join as a passenger
- **SOS Help** — send an emergency signal to nearby riders when you need assistance on the road
- **Rider ratings** — rate your experience after every ride or SOS interaction; build a trusted reputation in the community
- **Clubs** — private groups for existing moto communities *(v2)*
- **STO Catalog** — trusted motorcycle service stations with reviews *(v2)*
- **Bike History** — service and ownership history for a motorcycle by plate number *(v3)*

---

## Who It's For

| User | Pain Point |
|------|-----------|
| Everyday rider | Wants company for evening rides but has no quick way to find people |
| New in the city | Doesn't know the local riding scene |
| Adventure rider | Needs a reliable way to coordinate group trips |
| Girl riders | Wants a safe, vetted community to ride with |
| Passenger seeker | Wants to experience motorcycle riding without owning a bike |
| Club organizer | Needs a tool to manage members and announce events |
| STO owner | Wants to be discoverable by local riders |

---

## Key Features

### Rides
Create or join rides filtered by location, date, motorcycle type, and engine displacement. Rides can be open to everyone or restricted by bike category.

### Passenger Mode
A rider can mark that they have a free seat. Anyone — even without a motorcycle — can request to join as a passenger. Perfect for people who want to experience riding for the first time or riders who enjoy giving someone that experience.

### SOS Help
One tap sends an emergency signal with your GPS location to nearby riders. Whether it's a flat tire, running out of fuel, or a breakdown — someone from the community can respond. After the interaction, both sides can rate each other.

### Rider Ratings
After every completed ride or SOS response, participants can rate each other with a star rating. Ratings are visible on profiles and build a trust layer across the community — similar to how driver ratings work in ride-sharing apps, but for a moto community.

---

## Status

> Currently in active development. Backend MVP is complete. iOS app is in progress.

| Component | Status |
|-----------|--------|
| Backend API | MVP complete |
| iOS App | Core screens done — Rides (list, create, edit, my rides, join/leave), Profile, Settings, Avatar upload. Map + Ride details screen in progress |
| Android App | Planned |
| Passenger Mode | Planned (v1) |
| Rider Ratings | Planned (v1) |
| SOS System | Planned (v1) |
| Push Notifications | Planned (v1, required for SOS) |
| Local Feed | Planned (v1) |
| Localization (EN, UK, PL, DE) | In progress |
| Clubs | Planned (v2) |
| STO Catalog | Planned (v2) |
| Bike History | Planned (v3) |

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Backend | Node.js, TypeScript, Express |
| Database | PostgreSQL (Neon) |
| iOS | Swift, SwiftUI |
| Storage | Cloudflare R2 |
| Hosting | Railway |
| Email | Resend |

---

## Repositories

| Repo | Description | Visibility |
|------|-------------|------------|
| moto-community-backend | REST API | Private (opening soon) |
| moto-community-ios-app | iOS application | Private (opening soon) |
| [moto-community-docs](https://github.com/SaMaks90/moto-community-docs) | This repository — product docs | Public |

---

## Documentation

- [Product Vision](./PRODUCT.md) — problem, audience, monetization
- [Roadmap](./ROADMAP.md) — what's built, what's next
- [Tech Stack](./TECH.md) — architecture and infrastructure decisions
- [API Reference](./API.md) — endpoints overview
- [Interactive API Docs (Swagger)](https://moto-community-backend-pre-production.up.railway.app/docs/) — live, try it directly
- [Ideas](./IDEAS.md) — future feature backlog

---

## Why This Exists

Motorcycle culture is growing globally — but the tooling hasn't caught up.
Riders in Ukraine, Poland, Germany, and beyond still coordinate through Telegram chats and Facebook groups.
MotoCommunity is built to give this community a real home.

Starting with Ukraine and Poland, expanding across Europe and globally.
Launching with 4 languages: English, Ukrainian, Polish, and German.

The long-term vision is a platform layer for the motorcycle world:
from finding a ride partner to servicing your bike to verifying a used motorcycle's history.

---

## Contributing

This is an open-development project. Contributions are welcome across all areas:

- Backend (Node.js / TypeScript)
- iOS (SwiftUI)
- Android (future)
- Design / UX
- Ideas and feature feedback

See [CONTRIBUTING.md](./CONTRIBUTING.md) for details.

---

## Contact

Built by [@SaMaks90](https://github.com/SaMaks90)

For questions, ideas or partnership inquiries — open an issue or reach out directly.
