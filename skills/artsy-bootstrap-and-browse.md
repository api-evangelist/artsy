---
name: Bootstrap an Artsy token and browse the catalog
description: >-
  Mint an XAPP token from a client_id/client_secret pair and use it to page through Artsy's public
  artist and artwork catalog with cursor pagination.
api: openapi/artsy-public-api-openapi.yml
operations: [postApiTokensXappToken, getApiArtists, getApiArtworks, getApiStatus]
generated: '2026-09-07'
method: generated
source: >-
  openapi/artsy-public-api-openapi.yml, https://developers.artsy.net/v2/docs/authentication,
  https://developers.artsy.net/v2/docs/pagination, https://developers.artsy.net/v2/docs/rate_limiting
---

# Bootstrap an Artsy token and browse the catalog

**Before you start.** Artsy has publicly announced that this Public API is being retired and may be
taken down at any time without additional notice (`lifecycle/artsy-lifecycle.yml`). Treat anything you
build on it as short-lived. Access is also restricted by the provider to images of historic artwork and
related information for educational and other non-commercial purposes, and only public-domain works are
accessible.

## 1. Get credentials

Register a client application at <https://developers.artsy.net/client_applications> to obtain a
`client_id` and `client_secret`. There is no payment step.

## 2. Mint an XAPP token — `postApiTokensXappToken`

```
POST https://api.artsy.net/api/tokens/xapp_token
Content-Type: application/x-www-form-urlencoded

client_id=...&client_secret=...
```

The response is `{"type":"xapp_token","token":"...","expires_at":"..."}`. Put `token` in an
`X-Xapp-Token` header on every subsequent request. Cache it until `expires_at`; re-minting on every
call will burn your rate limit for nothing.

Only `application/json` and `application/x-www-form-urlencoded` are accepted on POST/PUT. Any other
`Content-Type` returns **406 Not Acceptable**.

## 3. Confirm you are up — `getApiStatus`

`GET /api/status` is the cheapest call that proves the token works.

## 4. Page the catalog — `getApiArtists`, `getApiArtworks`

```
GET https://api.artsy.net/api/artworks?size=25&total_count=1
X-Xapp-Token: <token>
```

- Pagination is **cursor-based**. Do not build page URLs. Read `_links.next.href` from the response and
  follow it. When `_links.next` is absent you are on the last page.
- `cursor` and `offset` are **mutually exclusive** — sending both returns an explicit 400.
- `total_count` is **not** computed unless you ask for it with `total_count=1`, and the provider warns
  it is an estimate that can drift between calls. Do not use it as a loop bound.
- Results live under `_embedded.<resource>`, not at the top level.

Useful filters (declared as RFC 6570 templates on the API root): `artist_id`, `partner_id`, `show_id`,
`similar_to_artwork_id`, `published`, `term`.

## 5. Respect the limit

**5 requests per second per client application ID.** Over it you get **429 Too many requests** — the
provider throttles rather than blacklists. There are **no** rate-limit response headers, so you get no
remaining-budget or reset signal. Pace yourself below 5 rps rather than reacting to a header that will
never arrive.

## Errors

The envelope is `{"type", "message", "detail"}` — **not** RFC 9457 problem+json. `type` is one of
`auth_error`, `param_error`, `other_error`.

**Branch on the numeric status code, never the reason phrase.** Artsy ships a documented easter egg
where a 401 may come back as `401 Broadway` (the address of Artsy HQ) instead of `401 Unauthorized`.
