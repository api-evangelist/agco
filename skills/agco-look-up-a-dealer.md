---
name: Look up an AGCO dealer
description: >-
  Find an AGCO dealer by dealer code, or page the full dealer list, using the AGCO Technical Support
  (ATS) API. Also covers the country roll-up used to size a market.
api: openapi/agco-ats-api-openapi.json
base_url: https://secure.agco-ats.com
operations:
  - Authentication_IsAlive
  - Dealers_GetDealers
  - Dealers_GetDealerbyDealerCode
  - DealerByCountry_GetCountries
---

# Look up an AGCO dealer

Every operation below is grounded in `openapi/agco-ats-api-openapi.json`, the Swagger 2.0 contract
AGCO serves at `https://secure.agco-ats.com/swagger/docs/v1`.

## Before you start

- **You need credentials.** There is no self-service signup. The ATS API is reached with
  credentials issued through the AGCO EDT Support Portal — `EDT_Support@agcocorp.com`. See
  `authentication/agco-ats-authentication.yml`.
- **The contract declares no security scheme.** A client generated straight from AGCO's document
  sends no credentials at all and every call will fail. You must add the header or OIDC flow
  yourself; `overlays/agco-ats-api-overlay.yaml` records the two observable mechanisms.
- **Check the surface is up** with `Authentication_IsAlive`
  (`GET /api/v2/Authentication/IsAlive`). There is no status page — this is the only liveness
  signal AGCO publishes.

## Steps

1. **Page the dealer list** — `Dealers_GetDealers` (`GET /api/v2/Dealers`).
   Pass `limit` and `offset`. The response is `API.PagedResponse[DealerDB.Models.Dealer]`:
   read `Metadata.TotalCount` to size the result up front and `Entities` for the page. There are no
   cursors; advance `offset` by `limit`.

2. **Resolve one dealer by code** — `Dealers_GetDealerbyDealerCode`
   (`GET /api/v2/Dealers/{DealerCode}`). Use this whenever you already hold a dealer code; do not
   scan the paged list for it.

3. **Roll up by country** — `DealerByCountry_GetCountries` (`GET /api/v2/DealerByCountry`) returns a
   total count of dealers per country as `API.PagedResponse[DealerDB.Models.DealersPerCountry]`.

## Handling failures

The contract declares **no 4xx or 5xx status codes**. Every failure arrives as the `default`
response carrying `API.Models.ApiError`:

```json
{ "DeveloperMessage": "...", "UserMessage": "...", "ErrorCode": 0, "MoreInfo": "..." }
```

`ErrorCode` is an unenumerated `int32` and AGCO publishes no code reference, so branch on
`DeveloperMessage` and surface `UserMessage`. See `errors/agco-problem-types.yml`.

## Rules that apply to every call

- **No idempotency key exists.** This flow is read-only so that is safe here, but do not carry the
  pattern to a write.
- **No rate-limit headers and no 429** are declared. Back off on your own schedule.
- **Content negotiation**: JSON and XML are both served. Send `Accept: application/json`.
