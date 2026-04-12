# Ideas & Future Features

A backlog of ideas that are not yet planned but worth keeping.
Some of these may make it into the roadmap, some may not.

---

## Product Ideas

### Ride Replay
After a ride is completed, show the route on a map based on participants' GPS tracks.
"Here's where we rode last Saturday" — shareable as a story or post.

### Weather Integration
Show weather forecast for the ride date and start location when viewing or creating a ride.
Nobody wants to plan a Sunday ride and find out it'll rain.

### Rider Verification Badge
Optional identity verification (driver's license photo or ID).
Verified badge on profile builds extra trust — especially useful for passenger mode.

### Ride Templates
Save a ride configuration as a template — same route, same filters — and repost it weekly.
Useful for organizers who run recurring rides.

### Seasonal Stats
End-of-season summary for each rider — total km, rides joined, rides created, top routes.
Shareable card for social media.

### Moto Type Compatibility Filter
When searching for a ride, filter by "compatible with my bike" automatically.
User sets their bike type once in profile — the app shows only relevant rides by default.

### Emergency Contact Integration
When SOS is activated, optionally also send a notification to a pre-set emergency contact (not just nearby riders).
Could include GPS location and a "I need help" message via SMS or email.

### Ride Chat
Temporary group chat that opens automatically when a rider joins a ride.
Auto-closes 24 hours after the ride ends.
Lighter than a full messaging system — purpose-built for ride coordination.

### Club Leaderboards
Within a club, track who rides the most, who responds to SOS, who organizes events.
Gamification layer for engaged communities.

---

## Monetization Ideas

### Local Business Announcements
Moto gear shops, STO owners, event organizers, and brands can register a business account and send geo-targeted announcements to riders in a chosen radius.

Key insight: unlike Instagram or Facebook Ads, no audience setup is needed — everyone on the platform is already a motorcycle rider. A shop in Warsaw reaches Warsaw riders instantly.

Potential clients:
- Moto gear and clothing shops
- Helmet and jacket brands
- STO owners ("seasonal checkup, 20% off this week")
- Moto rally and festival organizers
- Motorcycle dealerships — test ride events, new model launches
- Riding schools

→ Detailed breakdown in [PRODUCT.md](./PRODUCT.md#local-business-announcements--detailed)

### Sponsored Rides
A brand or shop sponsors a community ride — their logo appears on the ride card, they can add a short message.
Example: "This ride is brought to you by [Gear Shop Name] — 10% discount for all participants today."

### Insurance Partnership
Partner with a motorcycle insurance provider.
Riders get a discount code for signing up. Platform gets a referral fee.
Data angle: active riders with a track record may qualify for better rates.

### Premium Profile
Optional paid profile upgrade — custom profile URL, highlighted in search results, extended ride history.

---

## Technical Ideas

### Offline Mode
Cache the last known nearby rides list for areas with poor signal.
Show a "last updated X minutes ago" indicator when offline.

### Ride Matching Algorithm
Instead of manual browsing, proactively suggest rides to users based on:
- Their usual ride times
- Their bike type and engine size
- Their location history

### Anti-Spam for SOS
Rate limiting and abuse detection on SOS activations.
False activations hurt trust — need a way to flag and review.

### Open API
Once the platform has enough users, offer a public read-only API for:
- Moto event aggregators
- Navigation apps that want to show community rides on their maps
