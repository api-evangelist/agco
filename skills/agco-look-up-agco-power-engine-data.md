---
name: Look up AGCO Power engine and ECU data
description: >-
  Retrieve production calibration data and injector (IQA) codes for an AGCO Power engine by serial
  number, and check connectivity to AGCO Power Web Services, using the AGCO Technical Support (ATS)
  API. Read-only; the ECU write is called out but deliberately not performed.
api: openapi/agco-ats-api-openapi.json
base_url: https://secure.agco-ats.com
operations:
  - AftermarketServices_GetConnectionStatus
  - AftermarketServices_GetProductionData
  - AftermarketServices_GetEngineIQACodes
  - AftermarketServices_GetUserStatus
  - AftermarketServices_GetCerts
---

# Look up AGCO Power engine and ECU data

AGCO Power is AGCO's engine division. These operations sit in front of AGCO Power Web Services and
are the surface an EDT diagnostic kit uses in the field.

## Before you start

Most operations here require an `EDTInstanceId` query parameter — "the EDT Instance Id of the kit
calling this method", per the contract — and it is `required: true`. You cannot call them without a
registered diagnostic kit identity.

## Steps

1. **Check the downstream is reachable** — `AftermarketServices_GetConnectionStatus`
   (`GET /api/v2/AftermarketServices/Hello`) checks connectivity to AGCO Power Web Services. Do this
   first: a failure here is a downstream outage, not a bad serial number.

2. **Get production calibration data** — `AftermarketServices_GetProductionData`
   (`GET /api/v2/AftermarketServices/Engines/{serialNumber}/ProductionData`) returns an array of
   `AGCOPowerServices.Models.ProductionData`, each a `DataType` (e.g. `PowerCalibration`) plus
   base64 `DataValues` holding the raw calibration bytes.

3. **Get injector codes** — `AftermarketServices_GetEngineIQACodes`
   (`GET /api/v2/AftermarketServices/Engines/{serialNumber}/IQACodes`).

4. **Check a kit registration** — `AftermarketServices_GetUserStatus`
   (`GET /api/v2/AftermarketServices/UserStatuses`) takes `voucherCode` and `dealerCode` and returns
   `AGCOPowerServices.Models.UserStatus` — the status of an EDT kit registration with AGCO Power
   Web Services.

5. **Certificates** — `AftermarketServices_GetCerts`
   (`GET /api/v2/AftermarketServices/Certificates`). The contract carries no description for this
   operation ("No Documentation Found.").

## The write on this surface — do not automate it

`AftermarketServices_PutECU` (`PUT /api/v2/AftermarketServices/ECUs/{serialNumber}`) **activates,
deactivates, or reports as damaged** a physical engine control unit. Note carefully:

- Deactivation reverses activation, but the contract states **no path back from the Damaged state**.
- There is **no idempotency key**, so a retry after a timeout is not safe.
- `SerialNumber` is constrained by the contract to `^0?\d{23}$` — validate before sending.

Treat this as human-in-the-loop. `overlays/agco-ats-api-overlay.yaml` marks it
`x-consequence: high`.

## Handling failures

`API.Models.ApiError` on the `default` response, with an unenumerated `ErrorCode`. See
`errors/agco-problem-types.yml`.
