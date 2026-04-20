# API Reference

The backend exposes a REST API with full interactive documentation via Swagger UI.

**Interactive docs (live):**
[https://moto-community-backend-pre-production.up.railway.app/docs/](https://moto-community-backend-pre-production.up.railway.app/docs/)

---

## Endpoints Overview

### Auth
| Method | Path | Description |
|--------|------|-------------|
| POST | `/auth/register` | Register new user |
| POST | `/auth/login` | Login, returns access + refresh tokens |
| POST | `/auth/logout` | Invalidate refresh token |
| POST | `/auth/refresh` | Refresh access token |
| GET | `/auth/verify-email` | Verify email by token |
| POST | `/auth/resend-verification` | Resend verification email |

### Users
| Method | Path | Description |
|--------|------|-------------|
| GET | `/users/me` | Get current user profile |
| GET | `/users/:user_id` | Get user by ID |
| PATCH | `/users/me` | Update profile |
| PATCH | `/users/change-password` | Change password |
| DELETE | `/users/me` | Delete account |
| POST | `/users/avatar` | Upload avatar |

### Rides
| Method | Path | Description |
|--------|------|-------------|
| GET | `/rides` | List rides with filters and pagination |
| POST | `/rides` | Create ride |
| GET | `/rides/:ride_id` | Get ride by ID |
| PATCH | `/rides/:ride_id` | Update ride |
| PATCH | `/rides/:ride_id/status` | Change ride status |
| POST | `/rides/:ride_id/join` | Join a ride |
| POST | `/rides/:ride_id/leave` | Leave a ride |

### System
| Method | Path | Description |
|--------|------|-------------|
| GET | `/health` | Health check |
| GET | `/api/metrics` | Prometheus metrics |

---

## Authentication

All protected routes require a Bearer token in the Authorization header:

```
Authorization: Bearer <access_token>
```

Access tokens are short-lived. Use `POST /auth/refresh` with the refresh token to obtain a new access token.

---

## GET /rides — Query Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `status` | `string[]` | `active,planned` | Filter by ride status |
| `lat` | `string` | — | Latitude for geo-filter |
| `lng` | `string` | — | Longitude for geo-filter |
| `radius` | `number` | `20` | Search radius in km |
| `date_from` | `date` | — | Start of date range |
| `date_to` | `date` | — | End of date range |
| `moto_types` | `string[]` | — | Filter by motorcycle type |
| `cc_ranges` | `string[]` | — | Filter by engine displacement |
| `page` | `number` | `1` | Page number |
| `limit` | `number` | `20` | Items per page (max 100) |

### Moto types
`cross` `enduro` `sport` `naked` `touring` `cruiser` `adventure`

### CC ranges
| Value | Description |
|-------|-------------|
| `up_to_400` | Up to 400cc |
| `from_400_to_750` | 400 – 750cc |
| `over_750` | 750cc and above |

Rides without `moto_types` or `cc_ranges` set are open to everyone and always appear in filtered results.
