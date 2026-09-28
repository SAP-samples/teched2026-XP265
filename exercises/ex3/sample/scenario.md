# BTP Infrastructure Scenario — Inventory Management CAP (dev)

## Application

| Field | Value |
|---|---|
| Name | Inventory Management CAP |
| Description | CAP-based inventory management application with SAP Fiori UI |
| Stage | dev |

## Account Structure

- **Topology**: one dedicated subaccount per stage (flat directory structure per governance)
- **Subaccount name**: `invmgmt-dev`
- **Region**: `eu20` (Microsoft Azure, EU — preferred per governance)
- **Global account**: `terraformintprod`

## Cloud Foundry Environment

| Field | Value |
|---|---|
| CF org name | `invmgmt-cf-dev` |
| CF space name | `dev` |
| App memory | 1 GB |
| App instances | 1 |

## Services

All plans follow the dev-stage governance constraint (free plans where available; no paid plans).

| Service | Plan | Notes |
|---|---|---|
| `hana-cloud` | `hana` | SAP HANA Cloud — persistence for CAP application. Plan `hana` permitted for dev per governance. |
| `auditlog` | `oauth2` | SAP Audit Log Service — compliance logging for CUD operations on inventory records. `oauth2` plan for programmatic write access. |
| `cloud-logging` | `dev` | SAP Cloud Logging Service — application and runtime log collection. `dev` plan is the free/dev-tier plan. |
| `destination` | `lite` | BTP Destination Service — required for the S/4HANA Public Cloud destination. `lite` is the free elastic plan. |
| `identity` | `application` | SAP Cloud Identity Services — custom OIDC IdP. Default SAP ID Service not permitted per governance and scenario requirement. |

## Subscriptions

| Application | Plan | Notes |
|---|---|---|
| `build-workzone-standard` | `standard` | SAP Build Work Zone — Fiori launchpad hosting. No free plan available; standard is the base plan. |
| `build-code` | `free` | SAP Build Code — developer tooling (SAP Business Application Studio). Free plan available. |
| `cicd-app` | `free` | SAP Continuous Integration & Delivery — CI/CD pipelines. Free plan available. |

> **Note**: `build-code` and `cicd-app` are available in `eu20` but not `eu22`. The governance-preferred region `eu20` satisfies this constraint without any conflict.

## Destinations

| Name | Type | Target | Purpose |
|---|---|---|---|
| `S4HANA_PC` | BasicAuthentication | S/4HANA Public Cloud | CAP application integration with S/4HANA Public Cloud APIs via OAuth2 authentication |

## Security

| Field | Value |
|---|---|
| Custom IdP | Required |
| IdP type | OIDC |
| IdP provider | SAP Cloud Identity Services (`identity` / `application` plan) |
| Default SAP ID Service | Not permitted as application IdP |

## Governance Compliance

| Check | Status |
|---|---|
| Runtime | Cloud Foundry only — compliant |
| Region | `eu20` (Azure, EU) — compliant |
| Data residency | EU only — compliant |
| Dev service plans | `hana` (HANA Cloud), `oauth2` (Audit Log), `dev` (Cloud Logging), `lite` (Destination), `free` (Build Code, CICD) — compliant |
| `build-workzone-standard` standard plan | No free plan exists for this service — standard plan is the only available option; noted as exception |
| Custom IdP | SAP Cloud Identity Services (OIDC) — compliant |
| Cost center tag | `cost-center: CC-123456` — required on subaccount |
| Metered service warning | `hana-cloud` — metered, warning enabled per governance |
| Naming | Subaccount `invmgmt-dev`, CF org `invmgmt-cf-dev`, CF space `dev` — compliant with patterns |
