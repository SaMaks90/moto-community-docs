# Contributing to MotoCommunity

First of all — thanks for your interest. MotoCommunity is an open-development project and contributions of any kind are welcome.

---

## What We're Looking For

### Right Now (Active Needs)

| Area | Skills | Priority |
|------|--------|----------|
| iOS development | Swift, SwiftUI | High |
| Backend | Node.js, TypeScript | Medium |
| Design / UX | Figma or similar | Medium |
| Android | Kotlin or React Native | Future |

### Always Welcome
- Bug reports and bug fixes
- Documentation improvements
- Feature ideas and feedback
- Testing the app and reporting issues

---

## How to Contribute

### 1. Find something to work on

- Check open [Issues](https://github.com/SaMaks90/moto-community-docs/issues) — look for `good first issue` or `help wanted` labels
- Or open a new issue describing what you want to build or fix
- Comment on the issue before starting so we can discuss the approach

### 2. Fork and clone the relevant repository

| What you want to work on | Repository | Visibility |
|--------------------------|-----------|------------|
| Backend API | moto-community-backend | Private — request access via issue |
| iOS app | moto-community-ios-app | Private — request access via issue |
| Documentation | [moto-community-docs](https://github.com/SaMaks90/moto-community-docs) | Public |

### 3. Set up locally

Backend and iOS repositories are currently private. To get access, open an issue in [moto-community-docs](https://github.com/SaMaks90/moto-community-docs) describing what you'd like to contribute — access will be granted individually.

Once you have access:
- Backend: follow `README.md` in the backend repo
- iOS: open `MotoCommunity.xcodeproj` in Xcode 15+, set your development team in project settings, run on simulator or device

### 4. Make your changes

- Keep changes focused — one feature or fix per pull request
- Follow the existing code style (ESLint on backend, SwiftUI conventions on iOS)
- Write or update tests if you're touching backend logic
- Keep commit messages clear: `fix: join ride capacity check` or `feat: ride details screen`

### 5. Open a pull request

- Target the `main` branch
- Describe what you changed and why
- Link the related issue
- Screenshots or a short video for UI changes are very appreciated

---

## Areas That Need Help Most

### iOS — Rides & Map (High Priority)

The backend is complete. The iOS app has auth and profile done.
What's missing and most needed right now:

- Rides list screen
- Ride details screen
- Create ride screen
- Map screen (MapKit, nearby rides)

If you're an iOS developer looking to contribute something meaningful — this is where the impact is.

### Design

No Figma files exist yet. If you want to design screens for the missing iOS flows — that would be hugely valuable. Open an issue with your draft and let's talk.

### Android

Not started. If you're an Android developer (Kotlin) or React Native developer interested in building the Android version — reach out. This could be a co-founder conversation, not just a contribution.

---

## Code Style

### Backend
- TypeScript strict mode
- ESLint config in repo root
- Feature modules — keep logic in the right layer (controller → service → repository)
- Zod for all request validation

### iOS
- SwiftUI only (no UIKit)
- MVVM — Views are dumb, logic lives in ViewModels
- No third-party dependencies without discussion first

---

## Questions?

Open an issue or contact [@SaMaks90](https://github.com/SaMaks90) directly.

If you're interested in a bigger role — co-founder, lead Android dev, or long-term iOS contributor — mention it. This project has startup ambitions and the right people are welcome at the table.
