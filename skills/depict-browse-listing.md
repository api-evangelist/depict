---
name: Browse a product listing page
description: Navigate a merchant's listing hierarchy and render a product listing page (PLP) with Depict's ranking.
api: openapi/depict-storefront-openapi-original.json
operations:
  - Get_Listings_v3_listings_get
  - Get_Listing_v3_listings__listing_id__get
  - Get_Products_in_Listing_v3_listings__listing_id__products_post
---

# Browse a product listing page

Use the public Depict Storefront API (`https://api.depict.ai`) to build category/collection pages.

## Prerequisites
- The `merchant`, `market`, and `locale`.
- Either a Depict `listing_id` or the merchant's `external_id` (Shopify/Centra/WooCommerce ID).

## Steps
1. **Fetch the hierarchy.** Call `Get_Listings_v3_listings_get` (`GET /v3/listings`) to get the full
   listing tree for navigation menus and listing pickers; filter by listing type if needed.
2. **Resolve the listing.** Call `Get_Listing_v3_listings__listing_id__get`
   (`GET /v3/listings/{listing_id}`) — or `Get_Listing_via_External_ID_v3_listings_external_id__external_id__get`
   when you only have the merchant's external ID — to get the listing plus its ancestors, siblings and
   children.
3. **Load the products.** Call `Get_Products_in_Listing_v3_listings__listing_id__products_post`
   (`POST /v3/listings/{listing_id}/products`) with `merchant`, `market`, `locale`, and any `filters`
   / `sort` to get the ranked products for the PLP.
4. **Paginate** via the returned cursor for infinite scroll / pagination.

## Rules
- Depict returns products already sorted per the collection's pins and boost/bury rules.
- JSON in/out; HTTP 422 with `detail[]` on validation errors (see `errors/depict-problem-types.yml`).
- Reads only; no idempotency key required.
