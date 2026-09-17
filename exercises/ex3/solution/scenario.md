# BTP Infrastructure Scenario

## Application
- **Name**: Inventory Management (invmgmt)
- **Type**: SAP CAP (Cloud Application Programming Model)
- **UI**: SAP Fiori, deployed via SAP Build Work Zone
- **Runtime**: SAP BTP Cloud Foundry

## Environments
Each stage runs in its own dedicated subaccount.

| Stage | Subaccount Name |
|-------|----------------|
| dev | invmgmt-dev |
| staging | invmgmt-staging |
| prod | invmgmt-prod |

## Runtime (dev)
- Memory: 1 GB
- Instances: 1

## BTP Services Required

| Service | Plan | Purpose |
|---------|------|---------|
| SAP HANA Cloud | hana | Primary data store |
| SAP Audit Log Service | free (dev) / standard (staging, prod) | Compliance logging for CUD operations on inventory records |
| SAP Cloud Logging Service | free (dev) / standard (staging, prod) | Application and runtime log collection |
| SAP Build Work Zone | standard | Fiori UI hosting |
| SAP Build Code | standard | Developer tooling |
| SAP Continuous Integration & Delivery | standard | CI/CD pipelines |
| SAP Destination Service | lite | S/4HANA Public Cloud connectivity |
| SAP Cloud Identity Services | application | Authentication (OIDC); SAP ID Service is not permitted |

## Integrations
- **S/4HANA Public Cloud**: outbound connection via SAP Destination Service; destination configured per subaccount.

## Authentication
- Custom IdP: SAP Cloud Identity Services (OIDC)
- SAP ID Service: forbidden as application IdP (governance requirement)

## Compliance & Audit
- All create, update, and delete operations on inventory records must be logged to SAP Audit Log Service.
- Audit log retention and access governed by compliance team.

## Governance Alignment
- Regions: eu10 (primary), eu20 (secondary) — GDPR EU-only residency
- Naming: follows `invmgmt-{stage}` pattern
- Cost centre tag: `cost-centre = CC-123456` mandatory on all subaccounts
- Metered service: HANA Cloud (warning enabled)
