---
name: Search Artsy and browse shows, fairs and partners
description: >-
  Use the full-text search endpoint and traverse the exhibition graph — fairs to shows to partners to
  artworks.
api: openapi/artsy-public-api-openapi.yml
operations: [getApiSearch, getApiShows, getApiShowsId, getApiFairs, getApiFairsId, getApiPartners, getApiPartnersId, getApiProfiles, getApiProfilesId, getApiArtworks]
generated: '2026-09-07'
method: generated
source: openapi/artsy-public-api-openapi.yml, data-model/artsy-data-model.yml
---

# Search Artsy and browse shows, fairs and partners

Authenticate first — see `artsy-bootstrap-and-browse.md`.

## 1. Full-text search — `getApiSearch`

```
GET https://api.artsy.net/api/search?q=warhol
X-Xapp-Token: <token>
```

`q` is the only documented parameter; the API root declares the relation as the template
`/api/search{?q}`. Results come back in the standard HAL `_embedded` envelope.

Several collections also carry their own `term` filter, which is often a better fit than global search
when you already know the entity type:

```
GET /api/artists?term=warhol
GET /api/artworks?term=warhol
GET /api/users?term=...
```

## 2. The exhibition graph

```
fair ──< show >── partner
              └──< artwork
```

- `getApiFairs` — `GET /api/fairs?status=upcoming` (also `current`, `past`).
- `getApiShows` — `GET /api/shows?fair_id=<id>` or `?partner_id=<id>`, with the same `status` filter.
- `getApiPartners` — `GET /api/partners` for galleries, museums, institutions and auction houses.
  Filters: `partner_id`, `user_id`, `eligible_for_partner_analytics`.
- `getApiProfiles` — `GET /api/profiles/{id}` for the public profile attached to a partner or user.
- Artworks in a show: `GET /api/artworks?show_id=<id>`.

## 3. The `sample` trick

Every paginated collection accepts `sample`, which **redirects** you to the canonical URL of a random
element. It composes with filters, so a random upcoming show is:

```
GET https://api.artsy.net/api/shows?status=upcoming&sample=1
```

Follow the redirect. This is a documented feature, not an accident.

## 4. What is missing here

Viewing Rooms, Editorial articles, marketing collections and the curated discovery feeds are all
listed on <https://www.artsy.net/llms.txt> as first-class Artsy surfaces and **none of them exist in
this REST contract**. They are GraphQL-only. If your task needs them, this API is the wrong door —
see `mcp/artsy-tool-crosswalk.yml`.
