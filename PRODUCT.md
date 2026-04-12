# Product Vision

## Problem

Motorcycle riders have no dedicated tool for real-time ride coordination.
Today this happens through Telegram chats, Facebook groups, and word of mouth — fragmented, slow, and unsafe.

Specific pain points:
- No way to quickly find who's riding nearby right now
- No trusted way to meet new riders in a new city
- No structured SOS system when something goes wrong on the road
- People who want to experience motorcycle riding have no safe entry point
- Moto clubs have no proper tool to manage members and events

---

## Solution

A mobile platform that covers the full lifecycle of a motorcycle rider's social experience:

1. **Find** — discover rides and riders nearby in real time
2. **Connect** — join rides, offer or request a passenger seat
3. **Trust** — build reputation through post-ride ratings
4. **Stay safe** — SOS system for roadside emergencies
5. **Belong** — clubs, events, and community around shared passion

---

## Target Audience

### Primary
- Motorcycle riders aged 18–45 in Eastern Europe
- Riders who want to ride with others but lack a network
- People new to a city looking for a local riding scene

### Secondary
- People without a motorcycle who want to experience riding (passenger mode)
- Moto club organizers managing groups and events
- Girls and beginners looking for a safe, trusted community to ride with

### Future (v2–v3)
- STO (motorcycle service station) owners seeking visibility
- Moto gear shops and brands wanting to reach a local rider audience
- Moto event and rally organizers
- Used motorcycle buyers and sellers (bike history verification)

---

## Core Features by Version

### v1 — MVP
| Feature | Description |
|---------|-------------|
| Rider profile | Registration, avatar, bio, Instagram, location |
| Ride creation | Date, route, bike type filter, engine size filter, max participants |
| Ride discovery | Map + list view, geo-based search, filters |
| Join / Leave | One-tap participation with capacity enforcement |
| Passenger mode | Riders offer a seat; non-riders can join as passengers |
| SOS Help | Emergency signal with GPS to nearby riders |
| Push notifications | Required for SOS — riders nearby get an instant alert even with the app closed |
| Rider ratings | Star rating after completed rides and SOS interactions |
| Local feed | Activity feed filtered by region — nearby rides, community events, SOS activity |
| Email verification | Verified accounts only |
| Localization | English, Ukrainian, Polish, German |

> **Note:** Push notifications require an Apple Developer Program account ($99/year) and APNs backend integration. This is a prerequisite for the App Store launch regardless.

### v2 — Community
| Feature | Description |
|---------|-------------|
| Clubs | Private groups with membership, roles, and events |
| STO Catalog | Directory of motorcycle service stations with reviews |
| Local business announcements | Geo-targeted posts from shops, events, and brands |
| Chat | Direct messaging between riders |

### v3 — Platform
| Feature | Description |
|---------|-------------|
| Bike History | Service and ownership history by plate number |
| Marketplace | Buy/sell motorcycle gear and bikes |
| Android App | Expand reach beyond iOS |

---

## Monetization

The app is free to use. Revenue model options being considered:

| Model | Description | Timeline |
|-------|-------------|----------|
| **Freemium** | Free core features; premium subscription for advanced filters, priority SOS, club tools | v2 |
| **Local business announcements** | Geo-targeted announcements from moto-related businesses to riders nearby | v2 |
| **STO listings** | Paid placement and featured profiles for service stations | v2 |
| **Event promotion** | Paid promotion for moto events and club rides | v2 |
| **Data & partnerships** | Anonymized route and usage data for insurance companies, city planners, moto manufacturers | v3 |
| **Marketplace commission** | % fee on gear/bike transactions | v3 |

### Local Business Announcements — detailed

A business (gear shop, STO, event organizer, manufacturer) registers a business account and sends a geo-targeted announcement to riders within a chosen radius.

**Why it works:**
- 100% qualified audience — every user on the platform is a motorcycle rider
- No audience setup required — unlike Instagram or Facebook Ads where you configure interests and demographics, here the targeting is built in
- Hyperlocal by default — a shop in Warsaw reaches Warsaw riders, not random people across the country

**Who pays for this:**
- Moto gear and clothing shops
- Helmet and jacket brands running local promotions
- STO owners — "seasonal checkup, 20% off this week"
- Moto rally and festival organizers
- Motorcycle dealerships — test ride events, new model launches
- Riding schools

**How it works (proposed):**
1. Business registers a `business account`
2. Creates an announcement — title, text, image, link
3. Sets a target radius (e.g. 30 km from Kyiv)
4. Pays a flat fee per send — or a monthly subscription for unlimited sends
5. Riders in that area see it in a dedicated "Local" feed or receive a push notification (opt-in)

**Key UX principle:** announcements are clearly labeled as business posts, never mixed with organic ride content. Users can opt out of business notifications without losing ride alerts.

---

## Market

MotoCommunity targets a global audience from day one, with an initial focus on markets where the founding network exists.

### Launch markets
| Market | Registered motorcycles | Notes |
|--------|----------------------|-------|
| Ukraine | ~500,000 | Home market, initial community |
| Poland | ~1,200,000 | Strong moto culture, direct network |
| Germany | ~4,700,000 | Largest moto market in Europe |
| Europe total | ~35,000,000+ | Long-term expansion target |

### Global picture
- ~200,000,000+ registered motorcycles worldwide
- Southeast Asia and Latin America are the fastest-growing regions
- No single dominant community app globally — the space is fragmented by country and platform

**Comparable platforms for reference:**
- Rever (US) — route tracking and social, not real-time ride coordination
- Calimoto (Germany) — navigation focused, no community layer
- Strava (cycling) — proof that community + activity tracking is a globally sticky product

---

## Competitive Advantage

| Factor | MotoCommunity | Telegram/Facebook | Rever / Calimoto |
|--------|--------------|-------------------|-----------------|
| Real-time ride discovery | Yes | No | No |
| Passenger mode | Yes | No | No |
| SOS system | Yes | No | No |
| Rider trust ratings | Yes | No | No |
| Built for mobile | Yes | Partial | Yes |
| Eastern Europe focus | Yes | Yes | No |

---

## Success Metrics (v1)

| Metric | Target (3 months post-launch) |
|--------|-------------------------------|
| Registered users | 1,000+ |
| Completed rides | 200+ |
| SOS activations | Tracked (any number validates feature) |
| D7 retention | > 20% |
| Average rating given | > 4.0 / 5.0 |
