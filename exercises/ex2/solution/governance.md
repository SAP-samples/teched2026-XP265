# BTP Governance

## Regions
- Allowed: eu10, eu20
- Forbidden: all others
- Data residency: EU only
- Compliance reason: GDPR — inventory data must not leave the EU
- Preferred region: eu10

## Naming
- Subaccount pattern: invmgmt-{stage}
- Pattern tokens: {stage} = environment tier (dev, staging, prod)
- Environments: dev, staging, prod
- Directory structure: flat
- Max name length: 60

## Service Plans
### dev
- Permitted: free (SAP Audit Log Service, SAP Cloud Logging), hana (HANA Cloud)
- Forbidden: premium
- Forbidden reason: paid plans not permitted in dev
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
- IdP type: OIDC (SAP Cloud Identity Services)
- Default IdP note: SAP ID Service is not permitted as application IdP
- Default role collections: none defined
- Role collection scope: N/A

## Cost Controls
- Metered service warning: enabled
- Metered services: hana-cloud
- Cost centre tag: required
- Tag key name: cost-centre
- Tag value: CC-123456
