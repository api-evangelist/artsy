---
name: Walk an artist to their artworks, genes and editions
description: >-
  Resolve an artist by slug or id and traverse the HAL link graph out to their artworks, Art Genome
  genes, editions and images.
api: openapi/artsy-public-api-openapi.yml
operations: [getApiArtistsId, getApiArtworks, getApiGenes, getApiEditions, getApiImages, getApiArtists]
generated: '2026-09-07'
method: generated
source: >-
  openapi/artsy-public-api-openapi.yml, data-model/artsy-data-model.yml,
  https://developers.artsy.net/v2/docs/links
---

# Walk an artist to their artworks, genes and editions

Authenticate first — see `artsy-bootstrap-and-browse.md`. Send `X-Xapp-Token` on every call below.

## 1. Resolve the artist — `getApiArtistsId`

```
GET https://api.artsy.net/api/artists/andy-warhol
```

Item paths accept either the 24-character hex id (`4d8b92b34eb68a1b2c0003f4`) or the human-readable
slug. Both resolve to the same resource.

## 2. Do not construct URLs — follow `_links`

Artsy is a JSON+HAL API and the provider's stated navigation model is to walk from the root. Every
resource carries a `_links` object; a link with `"templated": true` is an RFC 6570 template you expand.
An artist resource links out to its artworks, genes and similar artists.

## 3. The relationships that exist

Read from the templated links the API root actually serves (`data-model/artsy-data-model.yml`):

| From | To | Via |
|---|---|---|
| artwork | artist | `GET /api/artworks?artist_id=` |
| artwork | partner | `GET /api/artworks?partner_id=` |
| artwork | show | `GET /api/artworks?show_id=` |
| gene | artist | `GET /api/genes?artist_id=` |
| gene | artwork | `GET /api/genes?artwork_id=` |
| edition | artwork | `GET /api/editions?artwork_id=` |
| artist | similar artists | `GET /api/artists?similar_to_artist_id=&similarity_type=` |
| artwork | similar artworks | `GET /api/artworks?similar_to_artwork_id=` |

## 4. Genes are the classification layer — `getApiGenes`

Genes are Artsy's Art Genome Project taxonomy: style, period, subject matter, medium. They are the
right axis for "find me more like this", and they are a **first-party vocabulary**, not an industry
standard — do not treat a gene name as portable to another art data source.

## 5. Images — `getApiImages`

Image resources carry templated URLs for the available versions. Remember the licence constraint: only
public-domain works are accessible, and use is limited to educational and other non-commercial
purposes.

## What this API cannot tell you

No response schemas are declared in the published contract, so nothing here is type-checkable ahead of
a live call. And auction results, price insights, editorial and viewing rooms are **not** on this REST
surface at all — they exist only in Artsy's GraphQL gateway. See `mcp/artsy-tool-crosswalk.yml`.
