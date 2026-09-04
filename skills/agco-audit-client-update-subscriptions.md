---
name: Audit an EDT client's update subscriptions
description: >-
  Determine which software an AGCO Electronic Diagnostic Tool installation is currently entitled to
  receive, and what else it could be subscribed to, using the AGCO Technical Support (ATS) API.
api: openapi/agco-ats-api-openapi.json
base_url: https://secure.agco-ats.com
operations:
  - Clients_Get
  - Clients_GetSubscriptions
  - Clients_GetAvailableSubscriptions
  - UpdateGroups_Get
  - PackageTypes_Get
  - Packages_GetPackages
  - Bundles_GetBundles
---

# Audit an EDT client's update subscriptions

A "Client" in this API is a registered installation of AGCO's Electronic Diagnostic Tool, not a
customer. This flow answers: what does this installation get, and what is it missing?

## Steps

1. **Find the client** — `Clients_Get` (`GET /api/v2/Clients`), paged with `limit`/`offset`.
   > **Contract gotcha:** the operationId `Clients_Get` is reused by AGCO for BOTH
   > `GET /api/v2/Clients` and `GET /api/v2/Clients/{ID}`. Generated clients will collide on it.
   > Bind on the path, not the operationId. The same defect applies to `PackageTypes_Get`,
   > `UpdateGroups_Get`, `Licenses_Get`, `Vouchers_Get` and
   > `AuthorizationCodeDefinitions_GetAuthorizationCodeDefinition`.

2. **Read current entitlements** — `Clients_GetSubscriptions`
   (`GET /api/v2/Clients/{ID}/UpdateGroupSubscriptions`) returns
   `UpdateSystem.Models.UpdateGroupSubscription` records, each tying a `ClientID` to an
   `UpdateGroupID` and a `PackageTypeID`.

3. **Read what is available but not taken** — `Clients_GetAvailableSubscriptions`
   (`GET /api/v2/Clients/{ID}/AvailableUpdateGroupSubscriptions`) returns
   `UpdateSystem.Models.AvailableUpdateGroupSubscription`, which nests the `UpdateGroup` and an
   `AvailableSubscriptions` array of `PackageType`. The gap between step 2 and step 3 is the audit
   finding.

4. **Resolve the names** — `UpdateGroups_Get` (`GET /api/v2/UpdateGroups`),
   `PackageTypes_Get` (`GET /api/v2/PackageTypes`), `Bundles_GetBundles` (`GET /api/v2/Bundles`)
   and `Packages_GetPackages` (`GET /api/v2/Packages`) turn the IDs above into human-readable
   package types, bundles and distributable packages. The relationship graph is in
   `data-model/agco-data-model.yml`.

## If you are going to CHANGE a subscription

Enrolling or removing a subscription is a write, and this API gives you no safety rails:

- **No idempotency key.** If a POST times out you cannot safely retry it — you may double-enrol.
- **No dry-run.** There is no validate-only mode.
- **Deletes are soft, but there is no stated window.** See `conventions/agco-conventions.yml`
  (`reversibility`). Confirm with a human before mutating a dealer's entitlements.

## Handling failures

All errors are the untyped `API.Models.ApiError` envelope on the `default` response — see
`errors/agco-problem-types.yml`.
