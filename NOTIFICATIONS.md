# Push Notifications

Push notifications are sent via Apple Push Notification service (APNs) using token-based auth (`.p8` key, ES256 JWT). The HTTP/2 connection uses Node.js built-in `http2` — no extra dependencies.

---

## Stack

- **Transport:** APNs HTTP/2 (Node.js built-in `http2`)
- **Auth:** `.p8` key + ES256 JWT (token-based, not certificate-based)
- **Backend service:** `src/services/apns.service.ts`
- **iOS registration:** `AppDelegate` → `didRegisterForRemoteNotificationsWithDeviceToken`
- **Device token storage:** `device_tokens` table (user_id, device_token, device_id)

---

## Notification Types & Triggers

| Type | Trigger | Sender | Recipients |
|------|---------|--------|------------|
| `joined_your_ride` | User joins a ride | Participant | Ride creator |
| `ride_status_changed` | Creator manually changes status to `planned` or `cancelled` | Creator | All participants (except creator) |
| `ride_started` | Cron job auto-activates ride (`planned` → `active`, every minute) | System | All participants |
| `ride_updated` | Creator edits ride (date, location, title, etc.) | Creator | All participants (except creator) |
| `new_chat_message` | Message sent in ride chat | Sender | All participants (except sender) |
| `new_ride_from_circle` | User creates a new ride | Creator | All users who have the creator in their Riding Circle |
| `organizer_ride_reminder` | Cron job — ride still `active` 4h+ after last status change (runs hourly) | System | Ride creator only |
| `rate_organizer` | Creator manually sets status to `completed` | Creator | All participants (except creator) |

> **Note:** `ride_status_changed` is NOT sent when status changes to `active` (covered by `ride_started`) or `completed` (covered by `rate_organizer`).

---

## User Preferences

Each user has a `notification_preferences` JSONB column. A notification is only sent if the preference is not explicitly `false` (`IS NOT FALSE` — so `NULL` = enabled).

### Global toggle
| Preference key | Controls |
|----------------|----------|
| `notifications_enabled` | All notifications (master switch) |

### Per-type toggles
| Preference key | Controls |
|----------------|----------|
| `joined_your_ride` | Someone joined your ride |
| `ride_status_changed` | Ride status changed (planned / cancelled) |
| `ride_updated` | Ride details updated |
| `new_chat_message` | New message in ride chat |
| `new_ride_from_circle` | New ride from a Circle member |

> `ride_started`, `organizer_ride_reminder`, and `rate_organizer` have **no individual toggle** — they only respect `notifications_enabled`.

**Default JSONB** (set on column creation in `initDb.ts`):
```json
{
  "notifications_enabled": true,
  "joined_your_ride": true,
  "ride_status_changed": true,
  "ride_updated": true,
  "new_chat_message": true,
  "new_ride_from_circle": true
}
```

**Endpoints:**
```
GET   /api/users/notification-preferences
PATCH /api/users/notification-preferences
```

---

## iOS Handling

### Registration flow
1. App launches → `AppState.bootstrap()` checks existing notification permission
2. If authorized → `UIApplication.shared.registerForRemoteNotifications()`
3. `AppDelegate.didRegisterForRemoteNotificationsWithDeviceToken` → converts `Data` to hex string → saves to `DeviceTokenManager` → sends `POST /device-tokens` to backend

### Device token lifecycle
- **On login:** token registered via `POST /device-tokens`
- **On logout:** token unregistered via `DELETE /device-tokens`
- **Token stored locally:** `DeviceTokenManager` (UserDefaults)

### Tap-to-navigate routing (`MainTabView`)

| Notification type | Action on tap |
|-------------------|---------------|
| `new_chat_message` | Opens `RideChatView` for the ride |
| `rate_organizer` | Opens `PostRideReviewView` directly via `reviewVM.loadForRide` |
| All others | Fetches ride and opens `RideDetailView` |

### Notification states
| App state | Behavior |
|-----------|----------|
| Foreground | Banner shown; handled in `UNUserNotificationCenterDelegate` |
| Background | Banner shown by system; tap opens app and routes |
| Terminated | Banner shown by system; tap launches app and routes |

---

## Backend Setup (Railway)

```
APNS_PRIVATE_KEY=-----BEGIN PRIVATE KEY-----\nMIGT...\n-----END PRIVATE KEY-----
APNS_KEY_ID=36A6U5S3KJ
APNS_TEAM_ID=<Team ID from Apple Developer Portal>
APNS_BUNDLE_ID=SamchenkoMaks.MotoCommunity
APNS_PRODUCTION=false   # false = sandbox (Debug / pre-production), true = production APNs (App Store)
```

If any of the four `APNS_*` vars (except `APNS_PRODUCTION`) are missing on startup — server logs a warning and continues with push notifications disabled.

> **`APNS_PRODUCTION`** controls the APNs endpoint independently of `NODE_ENV`:
> - `false` → `api.sandbox.push.apple.com` (use for Debug builds and TestFlight with pre-production)
> - `true` → `api.push.apple.com` (use for App Store / production builds)

**Convert `.p8` file to single-line env var:**
```bash
cat AuthKey_XXXXXXXXXX.p8 | awk 'NF {printf "%s\\n", $0}' | sed 's/\\n$//'
```

---

## Testing

### Requirements
- Real device (APNs does not work on simulator)
- Backend deployed with all four `APNS_*` env vars set
- `APNs initialized` visible in Railway logs on startup

### Test scenarios

| Scenario | How to trigger | Expected notification |
|----------|---------------|----------------------|
| Someone joins your ride | Simulator: login as User B → join User A's ride | iPhone (User A): "New rider joined" |
| Ride status → planned/cancelled | iPhone: change status manually | All participants: "Ride status changed" |
| Ride auto-activated | Wait for cron (`planned` ride past `date_time`) | All participants: "Ride started!" |
| Ride completed | iPhone: change status to `completed` | All participants: "How was the ride?" |
| Organizer reminder | Ride stays `active` for 4h+ | Creator: "Don't forget to close the ride" |
| New chat message | Simulator: send message in ride chat | iPhone: "New message" |
| New ride from Circle | iPhone: add User B to Riding Circle → Simulator: create a ride | iPhone: "New ride from your circle" |

### Quickest smoke test
1. iPhone — login as User A, create a ride with status `planned`
2. Simulator — login as User B, join that ride
3. iPhone should receive `joined_your_ride` push
