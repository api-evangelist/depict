---
name: Get personalized product recommendations
description: Fetch personalized and related-product recommendations from the Depict Storefront API for a shopper or product context.
api: openapi/depict-storefront-openapi-original.json
operations:
  - Recommendations_v3_recommend_products_post
  - Get_related_recommendations_v3_search_related_post
---

# Get personalized product recommendations

Use the public Depict Storefront API (`https://api.depict.ai`) to place recommendation zones.

## Prerequisites
- The `merchant` identifier, plus `market` and `locale`.
- Context for the recommendation: a `product_id` (PDP), a set of `product_ids` (cart), a category, or
  a `session_id`/`user_id` for personalization.

## Steps
1. **Product recommendations.** Call `Recommendations_v3_recommend_products_post`
   (`POST /v3/recommend/products`) with `merchant`, `market`, `locale`, the product/user context, and
   the recommendation `type` (e.g. similar, frequently-bought-together). Pass `session_id` to
   personalize.
2. **Related to a search.** For search-results pages, call
   `Get_related_recommendations_v3_search_related_post` (`POST /v3/search/related`) with the `query`
   and merchant/market/locale to surface related products.
3. **Track interactions.** Send the resulting impressions/clicks with the Depict Performance Client
   (`@depict-ai/dpc`) so recommendations keep improving.

## Rules
- Recommendations respect the merchant's brand rules and boost/bury configuration automatically.
- JSON in/out; validation errors are HTTP 422 with `detail[]` (see `errors/depict-problem-types.yml`).
- Reads are safe/idempotent; no idempotency key is used.
