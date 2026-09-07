---
name: Read Artsy auctions, lots and bidding state
description: >-
  Traverse sales to their lots and read bidder and bidder-position state for auction and buy-now
  listings.
api: openapi/artsy-public-api-openapi.yml
operations: [getApiSales, getApiSalesId, getApiSaleArtworks, getApiSaleArtworksId, getApiBidders, getApiBiddersId, getApiBidderPositionsId, getApiLotStandings]
generated: '2026-09-07'
method: generated
source: openapi/artsy-public-api-openapi.yml, data-model/artsy-data-model.yml
---

# Read Artsy auctions, lots and bidding state

Authenticate first — see `artsy-bootstrap-and-browse.md`.

## The entity chain

```
sale ──< sale_artwork (the lot) ──< bidder_position
  └──< bidder
```

## 1. Find sales — `getApiSales`

```
GET https://api.artsy.net/api/sales?is_auction=true&live=true&published=true
```

Filters declared on the root template: `is_auction`, `live`, `published`.

## 2. Get the lots — `getApiSaleArtworks`

```
GET https://api.artsy.net/api/sale_artworks?sale_id=<sale id>&published=true
```

`sale_artwork` is the join entity that places an artwork into a sale as a lot. Filters: `sale_id`,
`published`.

## 3. Bidders and positions — `getApiBidders`, `getApiBidderPositionsId`

```
GET https://api.artsy.net/api/bidders?sale_id=<sale id>
GET https://api.artsy.net/api/bidder_positions?sale_artwork_id=<lot id>
```

`getApiLotStandings` (`GET /api/lot_standings`) reports the current standing for lots.

## Read-only, and deliberately so

**This API has no bidding write operation.** There is no create-bid, no place-offer, no checkout.
Everything in this skill is a read. Artsy's entire commerce surface — orders, offers, payments,
fulfilment — lives in the GraphQL gateway (`commerce*` mutations, see
`mcp/artsy-tool-crosswalk.yml`), not here. Do not attempt to transact through this contract.

## Pagination and limits

Cursor pagination with `_links.next`; `cursor` and `offset` are mutually exclusive; ask for
`total_count=1` if you need a count. Stay under 5 requests/second or take a 429 with no reset header.
