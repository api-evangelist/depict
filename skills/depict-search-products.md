---
name: Search a storefront catalog
description: Run a hybrid AI + keyword product search against a Depict-powered storefront and offer autocomplete suggestions.
api: openapi/depict-storefront-openapi-original.json
operations:
  - Get_suggestions_v3_search_suggestions_get
  - Get_results_v3_search_results_post
---

# Search a storefront catalog

Use the public Depict Storefront API (`https://api.depict.ai`) to search a merchant's catalog.

## Prerequisites
- The `merchant` identifier for the shop.
- The `market` and `locale` the shopper is browsing in (list them with `get_markets_v3_markets_get` / `get_locales_v3_locales_get` if unknown).
- Optionally a `session_id` to personalize results and attribute tracking.

## Steps
1. **(Optional) Autocomplete.** As the shopper types, call `Get_suggestions_v3_search_suggestions_get`
   (`GET /v3/search/suggestions`) with `merchant`, `market`, `locale`, and the partial `query` to
   render query and listing suggestions.
2. **Run the search.** Call `Get_results_v3_search_results_post` (`POST /v3/search/results`) with the
   full `query`, `merchant`, `market`, `locale`, and any `filters` / `sort`. Pass `session_id` for
   personalization.
3. **Paginate.** Follow the returned cursor to fetch additional result pages (see
   `conventions/depict-conventions.yml` — cursor pagination).
4. **Render** the ranked products, respecting the merchandising rules Depict already applied.

## Rules
- Requests and responses are JSON; the API is designed to be called from the browser.
- Validation failures return HTTP 422 with a `detail[]` array (see `errors/depict-problem-types.yml`).
- No idempotency key is required or supported for reads.
