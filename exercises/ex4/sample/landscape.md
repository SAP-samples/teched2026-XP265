# BTP Account Landscape — Inventory Management CAP

## Global Account

| Field | Value |
|---|---|
| Subdomain | `terraformintprod` |
| Display name | terraform-integration-prod |

## Directory Structure

Flat — no directory groupings. Subaccounts sit directly under the global account.

## Subaccounts

### `invmgmt-dev`

| Field | Value |
|---|---|
| Display name | `invmgmt-dev` |
| Subdomain | `invmgmt-dev` |
| Region | `eu20` |
| Infrastructure provider | Microsoft Azure |
| Description | Inventory Management CAP — developer environment |
| Stage | `dev` |
| Cost center tag | `cost-center: CC-123456` |

#### Cloud Foundry Environment

| Field | Value |
|---|---|
| CF org name | `invmgmt-cf-dev` |
| CF space | `dev` |

#### Governance Checks

| Rule | Value | Status |
|---|---|---|
| Region allowed | `eu20` in `[eu20, eu22]` | compliant |
| Region preferred | `eu20` (Azure) matches preferred provider Microsoft Azure | compliant |
| Subaccount name pattern | `invmgmt-dev` matches `invmgmt-{stage}` | compliant |
| CF org pattern | `invmgmt-cf-dev` matches `invmgmt-cf-{stage}` | compliant |
| CF space pattern | `dev` matches `{stage}` | compliant |
| Allowed environment | Cloud Foundry | compliant |
| Directory structure | flat | compliant |
