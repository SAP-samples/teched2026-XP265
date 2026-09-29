# Scenario: Inventory Management CAP Application — dev

## Overview

A developer environment on Cloud Foundry for a CAP-based Inventory Management application.
Scope: `dev` stage only.

## Subaccount structure

- One dedicated subaccount per stage: `invmgmt-dev`
- Region: `eu20` (Microsoft Azure, EU — GDPR)
- One Cloud Foundry org: `invmgmt-cf-dev`
- One Cloud Foundry space: `dev`

## Application

- Name: `inventory-management`
- Runtime: Cloud Foundry
- Framework: SAP Cloud Application Programming Model (CAP)
- Stage: dev
- Memory: 1024 MB, 1 instance

### Cloud Foundry applications
- name: inventory-management
  stage: dev
  memory_mb: 1024

## BTP services

| Service | Offering | Plan | Type | Purpose |
|---|---|---|---|---|
| SAP HANA Cloud | `hana-cloud` | `hana` | service instance | Persistent data store for inventory records |
| SAP Audit Log Service | `auditlog` | `standard` | service instance | Compliance logging for CUD operations on inventory records |
| SAP Cloud Logging Service | `cloud-logging` | `standard` | service instance | Application and runtime log collection |
| Destination Service | `destination` | `lite` | service instance | Destination configuration for outbound connectivity |
| SAP Cloud Identity Services | `identity` | `application` | service instance | OIDC-based authentication; replaces default SAP ID Service |
| Cloud Foundry Runtime | `cloudfoundry` | `standard` | environment | CF org and space for CAP app deployment |

## Subscriptions

| Application | Offering | Plan | Purpose |
|---|---|---|---|
| SAP Build Work Zone | `build-workzone-standard` | `standard` | Fiori launchpad for UI delivery |
| SAP Build Code | `build-code` | `free` | Developer tooling (free plan available in dev) |
| CI/CD Service | `cicd-app` | `free` | Continuous integration and delivery (application subscription; free plan used in dev) |

## Destinations

| Name | Target system | Auth type | Purpose |
|---|---|---|---|
| `S4HANA_PUBLIC_CLOUD` | SAP S/4HANA Public Cloud | Basic Authentication | Integration with S/4HANA Public Cloud APIs |

## Security

- Authentication: SAP Cloud Identity Services (OIDC)
- Default SAP ID Service must not be used as application IdP
- Custom IdP trust to be configured on the subaccount

## Governance alignment

- Region `eu20`: allowed, Microsoft Azure, EU data residency — GDPR compliant
- Plans: `hana`, `standard`, `lite`, `free` — all permitted in dev; no forbidden plans used
- `build-code/free` and `cicd-app/free`: free plans preferred in dev per governance
- Cost centre tag `cost-center: CC-123456` must be applied to the subaccount
- HANA Cloud is a metered service — cost warning applies
