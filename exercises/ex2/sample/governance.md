# BTP Governance

## Regions
- Preferred infrastructure provider: Microsoft Azure
- Allowed: eu20, eu22
- Forbidden: all others
- Data residency: EU only
- Compliance reason: GDPR — inventory data must not leave the EU
- Preferred region: eu20

## Account Setup
- Allowed environments: Cloud Foundry

## Naming
- Subaccount pattern: invmgmt-{stage}
- Pattern tokens: {stage} = environment tier (dev, staging, prod)
- Cloud Foundry org pattern: invmgmt-cf-{stage}
- Kyma environment pattern: N/A
- Cloud Foundry space pattern: {stage}
- Directory structure: flat
- Max name length: 60

## Service Plans

### dev

- Permitted: free (SAP Audit Log Service, SAP Cloud Logging), hana (HANA Cloud)
- Forbidden: premium, and all paid plans
- Forbidden reason: no paid plans in dev to control costs
- Default plan: standard

### staging

- Permitted: all
- Forbidden: none
- Default plan: standard

### prod

- Permitted: all
- Forbidden: none
- Default plan: standard

## Security

- Custom IdP: required
- IdP type: OIDC
- IdP provider: SAP Cloud Identity Services
- Default SAP ID Service: not permitted as application IdP
- Default role collections: none
- Role collection scope: N/A

## Cost Controls

- Metered service warning: enabled
- Metered services: hana-cloud
- Cost centre tag: required
- Tag key name: cost-center
- Tag value: CC-123456
