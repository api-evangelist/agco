# AGCO (agco)
AGCO is a global leader in the design, manufacture, and distribution of agricultural machinery and precision ag technology. The AGCO AgCommand API enables approved third-party developers and service providers to access machine telemetry data, location tracking, and performance metrics from AGCO Connect-ready equipment including Fendt, Massey Ferguson, Challenger, and Valtra brands.

**URL:** [https://get.agcoconnect.com](https://get.agcoconnect.com)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - Agriculture, Farm Equipment, IoT, Precision Farming, Telematics

## Timestamps

- **Created:** 2026-03-01
- **Modified:** 2026-04-19

## APIs

### AGCO AgCommand API
The AGCO AgCommand API provides approved third-party developers and service providers with access to machine telemetry data from AGCO equipment. The API enables farming application developers to build management dashboards and mobile apps that access real-time machine data including location, performance metrics, and diagnostic information from AGCO Connect-ready machines. AGCO uses JSON API profiles for standardized filtering, search, and change events.

**Human URL:** [https://get.agcoconnect.com/](https://get.agcoconnect.com/)

#### Tags:

 - Agriculture, Farm Equipment, IoT, Precision Farming, Telematics

#### Properties

- [Portal](https://get.agcoconnect.com/)
- [Documentation](https://github.com/agco/agco-json-api-profiles)
- [OpenAPI](openapi/agco-agcommand-api-openapi.yml)

## Common Properties

- [Portal](https://get.agcoconnect.com/)
- [GettingStarted](https://github.com/agco/agco-json-api-profiles)
- [GitHubOrganization](https://github.com/agco)
- [TermsOfService](https://www.agcocorp.com/legal/privacy-policy.html)

## Features

| Name | Description |
|------|-------------|
| Machine Telematics | Real-time access to machine performance data including engine speed, load, fuel consumption, and fault codes from AGCO Connect-ready equipment. |
| Fleet Location Tracking | GPS-based machine location history enabling field work tracking and fleet management dashboards. |
| Diagnostic Fault Codes | Remote access to machine diagnostic codes enabling proactive maintenance and reducing downtime. |
| JSON API Profiles | Standardized filtering, search, and change event profiles for consistent API behavior across all resources. |
| Multi-Brand Coverage | Single API access to data from Fendt, Massey Ferguson, Challenger, and Valtra agricultural equipment. |

## Use Cases

| Name | Description |
|------|-------------|
| Farm Management Dashboard | Build web and mobile dashboards that display real-time machine location, performance, and fuel status for farm operators. |
| Predictive Maintenance | Monitor machine fault codes and engine hours remotely to schedule preventive maintenance before failures occur. |
| Field Work Tracking | Track machine location and activity data to document field operations, coverage areas, and productivity metrics. |
| Fuel Management | Monitor fuel levels and consumption rates across a fleet to optimize refueling logistics and reduce costs. |
| Telematics Integration | Integrate AGCO machine data into existing farm management or precision agriculture software platforms. |

## Integrations

| Name | Description |
|------|-------------|
| Procore | Integration of AGCO telematics data with Procore construction and project management workflows. |
| Precision Ag Software | Integration with precision agriculture software platforms for combined field and machine data analysis. |

## Artifacts

Machine-readable API specifications organized by format.

### OpenAPI

- [agco-agcommand-api-openapi](openapi/agco-agcommand-api-openapi.yml)

### JSON Schema

- [agco-location-schema](json-schema/agco-location-schema.json)
- [agco-machine-schema](json-schema/agco-machine-schema.json)
- [agco-telemetry-schema](json-schema/agco-telemetry-schema.json)

### JSON Structure

- [agco-location-structure](json-structure/agco-location-structure.json)
- [agco-machine-structure](json-structure/agco-machine-structure.json)
- [agco-telemetry-structure](json-structure/agco-telemetry-structure.json)

### JSON-LD

- [agco-telematics-context](json-ld/agco-telematics-context.jsonld)

### Examples

- [agco-location-example](examples/agco-location-example.json)
- [agco-machine-example](examples/agco-machine-example.json)
- [agco-telemetry-example](examples/agco-telemetry-example.json)

## Capabilities

Naftiko capabilities organized as shared per-API definitions composed into customer-facing workflows.

### Shared Per-API Definitions

- [agcommand-api](capabilities/shared/agcommand-api.yaml)

### Workflow Capabilities

| Workflow | APIs Combined | Tools | Persona |
|----------|--------------|-------|---------|
| [AGCO Precision Farming](capabilities/precision-farming.yaml) | agco-agcommand | 3 | Farm Manager |

## Vocabulary

- [AGCO Vocabulary](vocabulary/agco-vocabulary.yaml) — Unified taxonomy mapping resources, actions, workflows, and personas

## Rules

- [agco-spectral-rules](rules/agco-spectral-rules.yml) — 24 rules enforcing AGCO API conventions
