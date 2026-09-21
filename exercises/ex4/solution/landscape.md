# BTP Account Landscape

## Global Account

| Field | Value |
|-------|-------|
| Subdomain | terraformintprod |
| Directory structure | flat — no directories |

## Subaccounts

| Name | Region | Subdomain | Description | Cost Centre Tag |
|------|--------|-----------|-------------|-----------------|
| invmgmt-dev | eu10 | invmgmt-dev | Inventory Management — development | CC-123456 |
| invmgmt-staging | eu10 | invmgmt-staging | Inventory Management — staging | CC-123456 |
| invmgmt-prod | eu10 | invmgmt-prod | Inventory Management — production | CC-123456 |

## Runtime Environments

| Subaccount | Environment Type | CF Org Name | CF Space(s) |
|------------|-----------------|-------------|-------------|
| invmgmt-dev | Cloud Foundry | invmgmt-dev | dev |
| invmgmt-staging | Cloud Foundry | invmgmt-staging | staging |
| invmgmt-prod | Cloud Foundry | invmgmt-prod | prod |

## Governance Validation

| Rule | Status |
|------|--------|
| Regions (eu10 only) | PASS — all subaccounts in eu10 |
| Naming pattern `invmgmt-{stage}` | PASS — dev / staging / prod |
| Environment tiers | PASS — dev, staging, prod defined in governance |
| Cost centre tag `cost-centre = CC-123456` | PASS — applied to all subaccounts |
| Max name length ≤ 60 | PASS — longest is `invmgmt-staging` (16 chars) |
| CF org naming (`invmgmt-<env>`) | PASS — matches subaccount naming convention |
| CF space naming (stage name) | PASS — dev / staging / prod |

## Notes

- eu20 is approved as secondary/DR; not provisioned here — scenario specifies no cross-region HA requirement.
- Subdomains match subaccount names (standard BTP convention).
- Global account GUID must be supplied at terraform plan time via `terraform.tfvars` or env variable; only the subdomain is recorded here.
