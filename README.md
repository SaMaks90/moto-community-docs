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
- **STO Catalog** — trusted motorcycle repair shops with reviews *(v2)* — STO: UA Станція Технічного Обслуговування / BY СТА / EN motorcycle repair shop / PL warsztat / DE Werkstatt
- **Bike History** — service and ownership history for a motorcycle by plate number *(v3)*

---

## Who It's For

| User                               | Pain Point                                                          |
|------------------------------------|---------------------------------------------------------------------|
| Everyday rider                     | Wants company for evening rides but has no quick way to find people |
| New in the city                    | Doesn't know the local riding scene                                 |
| Adventure rider                    | Needs a reliable way to coordinate group trips                      |
| Girl riders                        | Wants a safe, vetted community to ride with                         |
| Passenger seeker                   | Wants to experience motorcycle riding without owning a bike         |
| Club organizer                     | Needs a tool to manage members and announce events                  |
| STO owner (motorcycle repair shop) | Wants to be discoverable by local riders                            |

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

> Currently, in active development. Phase 1 is nearly complete — targeting App Store release August 2026.

| Component                             | Status                                                                                                                      |
|---------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| Backend API                           | ✅ Complete — auth, rides, participants, geo-filtering, Trust & Riding Circle, Rider Languages, OAuth, logging               |
| iOS App                               | ✅ Core complete — Auth (incl. Face ID, Google, Apple), Rides, Map, Profile, Trust & Riding Circle, Rider Languages, Support |
| Localization (EN, UK, PL, DE, RU, BE) | ✅ Complete                                                                                                                  |
| Website + Privacy Policy              | ✅ Complete — multilingual Next.js landing, SEO, OG Image, JSON-LD                                                           |
| Trust & Riding Circle                 | ✅ Complete (backend + iOS)                                                                                                  |
| Rider Languages                       | ✅ Complete (backend + iOS)                                                                                                  |
| Forgot / Reset Password               | ✅ Complete                                                                                                                  |
| Push Notifications                    | ⏳ Planned (v1, blocked — requires Apple Developer Program $99)                                                              |
| Production infrastructure             | ✅ Complete                                                                                                                  |
| App Store release                     | 🎯 Target: July 2026                                                                                                        |
| Android App                           | Planned                                                                                                                     |
| Passenger Mode                        | Planned (v2)                                                                                                                |
| SOS System                            | Planned (v2)                                                                                                                |
| Local Feed                            | Planned (v2)                                                                                                                |
| Clubs                                 | Planned (v2)                                                                                                                |
| STO Catalog                           | Planned (v2)                                                                                                                |
| Bike History                          | Planned (v3)                                                                                                                |

---

## Tech Stack

| Layer     | Technology                           |
|-----------|--------------------------------------|
| Backend   | Node.js, TypeScript, Express         |
| Database  | PostgreSQL (Neon)                    |
| iOS       | Swift, SwiftUI                       |
| Website   | Next.js 16, TypeScript, Tailwind CSS |
| Storage   | Cloudflare R2                        |
| Hosting   | Railway                              |
| Email     | Resend                               |
| Analytics | Cloudflare Web Analytics             |

---

## Repositories

| Repo                                                                   | Description                                                                            | Visibility             |
|------------------------------------------------------------------------|----------------------------------------------------------------------------------------|------------------------|
| moto-community-backend                                                 | REST API                                                                               | Private (opening soon) |
| moto-community-ios-app                                                 | iOS application                                                                        | Private (opening soon) |
| [moto-community-web](https://github.com/SaMaks90/moto-community-web)   | Next.js landing page + Privacy Policy — [motocommunity.app](https://motocommunity.app) | Public                 |
| moto-community-telegram-bot                                            | Support & feedback bot (@motocommunity_bot)                                            | Private (opening soon) |
| [moto-community-docs](https://github.com/SaMaks90/moto-community-docs) | This repository — product docs                                                         | Public                 |

---

## Documentation

- [Live Website](https://motocommunity.app) — landing page and Privacy Policy
- [Product Vision](./PRODUCT.md) — problem, audience, monetization
- [Roadmap](./ROADMAP.md) — what's built, what's next
- [Tech Stack](./TECH.md) — architecture and infrastructure decisions
- [API Reference](./API.md) — endpoint overview
- [Interactive API Docs (Swagger)](https://moto-community-backend-pre-production.up.railway.app/docs/) — live, try it directly
- [Ideas](./IDEAS.md) — future feature backlog

---

## Why This Exists

Motorcycle culture is growing globally — but the tooling hasn't caught up.
Riders in Ukraine, Poland, Germany, and beyond still coordinate through Telegram chats and Facebook groups.
MotoCommunity is built to give this community a real home.

Starting with Ukraine and Poland, expanding across Europe and globally.
Launching with six languages: English, Ukrainian, Polish, German, Russian, and Belarusian.

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

## Community & Support

|                         |                                                                                                        |
|-------------------------|--------------------------------------------------------------------------------------------------------|
| 📣 **Telegram Channel** | [@motocommunityapp](https://t.me/motocommunityapp) — news, updates, release announcements              |
| 🤖 **Telegram Bot**     | [@motocommunity_bot](https://t.me/motocommunity_bot) — bug reports, questions, feedback                |
| ☕ **Ko-fi**             | [ko-fi.com/samchenkoms](https://ko-fi.com/samchenkoms) — support the development                       |
| 🎗️ **Patreon**         | [patreon.com/samchenko_maksym](https://patreon.com/samchenko_maksym) — monthly support                 |
| 💙 **PayPal**           | [Donate via PayPal](https://www.paypal.com/donate/?hosted_button_id=5QP62QR8V23RL) — one-time donation |

Follow the Telegram channel to stay updated on new releases and features.
If you found a bug or have a suggestion — write to the bot; it goes straight to the developer.

---

## Contact

Built by [@SaMaks90](https://github.com/SaMaks90)

For questions, ideas, or partnership inquiries — open an issue, write to [@motocommunity_bot](https://t.me/motocommunity_bot), or reach out directly.
