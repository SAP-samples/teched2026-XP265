# Detail the service setup

With the account topology defined, the agent now resolves the full set of SAP BTP services required — entitlements, subscriptions, and service instances — in the correct dependency order.

This is the exercise where the richness of the inventory management scenario really pays off. The agent must reason about which services the CAP application needs, which plans are appropriate for a `dev` environment, and which services depend on other services being in place first. It also performs another round of governance validation, this time checking every service plan choice against the governance rules you defined in Exercise 2.

## What services the inventory management scenario needs

The agent will derive the following from `specs/scenario.md` and `specs/landscape.md`:

| # | Service | Plan | Type | Why |
|---|---|---|---|---|
| 1 | Cloud Foundry Runtime | `MEMORY` | Entitlement | Runtime for the CAP application |
| 2 | SAP HANA Cloud | `hana` | Entitlement + Instance | Persistent database for inventory data |
| 3 | SAP HANA Cloud | `hdi-shared` | Service Instance | HDI container used by CAP for schema management |
| 4 | SAP Audit Log Service | `auditlog-management`, `free` plan | Entitlement + Instance | Compliance logging for all inventory data changes |
| 5 | SAP Cloud Logging Service | `cloud-logging`, `free` plan | Entitlement + Instance | Centralized log aggregation for developers |
| 6 | SAP Build Work Zone | `SAPLaunchpad`, `standard` | Entitlement + Subscription | Fiori launchpad for the inventory UI |
| 7 | SAP Build Code | `build-code`, `free` | Entitlement + Subscription | CAP development tools and CI/CD |
| 8 | SAP Cloud Identity Services | `sap-identity-services-onboarding`, `default` | Entitlement + Subscription | Enterprise identity provider |
| 9 | Authorization and Trust Management (XSUAA) | `xsuaa`, `application` | Entitlement + Instance | Application-level authorization for CAP |
| 10 | Destination Service | `destination`, `lite` | Entitlement + Instance | Connectivity to the S/4HANA system |

> [!NOTE]
> The agent understands that a CAP application on Cloud Foundry requires XSUAA for application security. This is a BTP-specific dependency the skill brings — you described an "Inventory Management CAP app" and the skill inferred the need for XSUAA without you having to ask for it.

## Governance check: service plan validation

For the `dev` environment tier, the governance file specifies:
- `free` plan is permitted for Audit Log and Cloud Logging ✓
- `hana` plan is permitted for HANA Cloud ✓
- `standard` plan is permitted for Build Work Zone ✓
- `premium` plan is **forbidden** in `dev`

If the agent were to suggest a `premium` plan for any service, the governance enforcement would stop it with an explicit error. Try modifying a plan after the exercise and re-running to see the enforcement in action.

## 🛠️ Execute the services command

Execute the following command in the Claude Code terminal:

```bash
/btp-iac.services
```

The skill reads `specs/scenario.md`, `specs/landscape.md`, and `memory/governance.md`. No additional input is needed — the scenario description already contains all the signals.

### 🔬 Look behind the curtains

Open the `btp-iac.services.md` skill file. Notice:

1. The skill builds the service list **in dependency order** — services that other services depend on are listed first. HANA Cloud entitlement before HANA Cloud instance; XSUAA before the CAP application binding.
2. For each service, it identifies the **environment tier** from `specs/landscape.md` (in this case `dev`) and validates the plan choice against the governance file's `## Service Plans` section.
3. The output `specs/services.md` is structured as a dependency-ordered list, not a flat catalogue. This ordering will be preserved all the way into the generated Terraform code.

### ✅ Result

After the command completes:

- Open `specs/services.md` — you should see the service entries listed in dependency order, with their plans and the type of BTP resource (entitlement, subscription, or service instance).
- Verify that the `free` plans for Audit Log and Cloud Logging are listed — these are the governance-compliant choices for `dev`.
- Check that HANA Cloud has both an `hana` entitlement entry and an `hdi-shared` service instance entry for the HDI container.
- Cross-check what has happened in the `memory/` folder.

**Key insight:** A senior BTP architect could have derived this list from experience. The spec-driven approach encodes that expertise in the skill — a developer who has never set up HANA Cloud for CAP before still gets the correct dependency-ordered service list.

## 📋 Summary

All services, entitlements, and subscriptions are now specified in the correct dependency order. The next exercise defines the security configuration — the trust with SAP Cloud Identity Services, the role collections for inventory users, and the destination for S/4HANA connectivity.

Continue to - [Exercise 6 - Detail the security setup](../ex6/README.md)
