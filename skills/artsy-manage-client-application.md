---
name: Manage an Artsy client application and its tokens
description: >-
  The complete write surface of the Artsy Public API — create, update and delete a client application,
  register/deregister devices, and mint or revoke tokens — with the reversibility facts for each.
api: openapi/artsy-public-api-openapi.yml
operations: [getApiApplications, postApiApplications, getApiApplicationsId, putApiApplicationsId, deleteApiApplicationsId, getApiTokens, postApiTokensXappToken, deleteApiTokensAccessToken, getApiDevices, getApiDevicesId, deleteApiDevicesId, getApiCurrentUser]
generated: '2026-09-07'
method: generated
source: >-
  openapi/artsy-public-api-openapi.yml, conventions/artsy-conventions.yml,
  https://developers.artsy.net/v2/docs/authentication
---

# Manage an Artsy client application and its tokens

This is the **entire** mutating surface of the Artsy Public API: six write operations. Everything else
in the contract is a read.

## Read this before you write anything

**There is no idempotency mechanism.** No `Idempotency-Key` header, no request-deduplication window,
no replay-safety statement anywhere in the documentation or the contract. A retried
`POST /api/applications` creates a **second application**. If a write times out, do not blind-retry —
`GET /api/applications` first and reconcile.

## Applications

| Operation | Call | Reversible? |
|---|---|---|
| `getApiApplications` | `GET /api/applications` | n/a |
| `postApiApplications` | `POST /api/applications` body `{"name": "..."}` → **201** | Yes, by `deleteApiApplicationsId`. No window is published. |
| `getApiApplicationsId` | `GET /api/applications/{id}` | n/a |
| `putApiApplicationsId` | `PUT /api/applications/{id}` body `{"name": "..."}` → **200** | Only by writing the previous value back. The API returns no prior-version record, so **capture the current value before you update**. |
| `deleteApiApplicationsId` | `DELETE /api/applications/{id}` → **204** | **No.** No restore, undelete or trash operation is documented. The `client_id` and `client_secret` go with it. |

`name` is the only documented property; it is required on create and optional on update.

## Tokens

- `postApiTokensXappToken` — `POST /api/tokens/xapp_token` with `client_id` + `client_secret` → **201**.
  No revoke operation exists for an XAPP token; it lapses at the `expires_at` in its own response.
- `deleteApiTokensAccessToken` — `DELETE /api/tokens/access_token` with the `access_token` → **204**.
  This is the logout path for a user token. Reversible only by re-running an `/oauth2/access_token`
  grant, which needs the user again.
- `getApiTokens` — `GET /api/tokens` lists tokens.

Token lifetime is 60 days by default. Passing `scope=offline_access` on an `/oauth2/access_token`
grant requests a token that expires in **25 years** instead — a real decision, not a tuning knob.
Prefer the 60-day default unless the use case genuinely justifies a two-decade credential.

## Devices

`getApiDevices`, `getApiDevicesId`, and `deleteApiDevicesId` (`DELETE /api/devices/{id}` → 204) manage
push-notification device registrations. Filters: `user_id`, `app_id`. Deregistering is undone only by
re-registering; no window is published.

## Content type

POST and PUT accept `application/json` or `application/x-www-form-urlencoded` only. Anything else is a
**406 Not Acceptable**.

## When a write fails

Errors are `{"type","message","detail"}`. A `param_error` carries a `detail` map keyed by field name
with an array of reasons per field — read it rather than the `message` string. Branch on the numeric
status: a 401 may be returned with the reason phrase `401 Broadway`.
