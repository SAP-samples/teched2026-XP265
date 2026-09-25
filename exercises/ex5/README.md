# Detail the service setup

With the account topology defined, the agent now resolves the full set of services required — entitlements, subscriptions, and service instances — in the correct dependency order.

This is the exercise where the richness of the inventory management scenario really pays off. The agent must reason about which services the CAP application needs, which plans are appropriate for a `dev` environment, and which services depend on other services being in place first. It also performs another round of governance validation, this time checking every service plan choice against the governance rules you defined in Exercise 2.

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can use as a **cross reference**. Do not copy it as it might not be perfectly fitting to the previous specifications that have been created by you.

## 🎯 Intent of the exercise

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

## 🛠️ Execute the services command

Execute the following command in the Claude Code terminal:

```bash
/sap-iac.services
```

The skill reads `specs/scenario.md`, `specs/landscape.md`, and `memory/governance.md`. No additional input is needed — the scenario description already contains all the signals. The skill will validate the settings using the btp CLI. You might be prompted to accept these calls of the CLI.

After validating the settings using the btp CLI, it will check for available plans and categories of the services. If adjustments concerning the selections are required, it will ask you to confirm them. Confirm the proposals.

The skill will then then ask you for each service/app whether you want to create a service instance/app subscription or if you only want to entitle it. Confirm the default choice for each service/app by stating `default`.

After that it will ask you about the deployment target for the services as you can deploy them to Cloud Foundry or directly to the platform
(environment "Other" in the cockpit). Select `BTP` except for the destination service that should be deployed to Cloud Foundry.

> [!TIP]
> The flow considers any rules defined in governance which can lead to less manual decision-making and ensures compliance with organizational policies.

As some services require parameters, the skill will prompt you to provide the necessary values during the execution.

For **hana-cloud / hana** key in the following values:

```text
memory: 32
edition: cloud
generateSystemPassword: true
```

For the optional parameters of HANA Cloud enter the following statement:

```text
Use default or leave empty for all optional parameters.
```

For **destination / lite** key in the following values:

```text
HTML5Runtime_enabled true
```

After answering the questions it will present you the summary including the passed governance checks for confirmation before proceeding. Confirm by selecting `Yes`. This will create a new file in `specs/` as `services.md`.

### 🔬 Look behind the curtains

Open the `sap-iac.services.md` skill file. Notice:

1. The skill builds the service list **in dependency order** — services that other services depend on are listed first. HANA Cloud entitlement before HANA Cloud instance; XSUAA before the CAP application binding.
2. For each service, it identifies the **environment tier** from `specs/landscape.md` (in this case `dev`) and validates the plan choice against the governance file's `## Service Plans` section.
3. The skill asks for each service/app if it should be provisioned or entitled only as well as into which environment it should be provisioned to if no restrictions are already defined in the governance step.
4. The output `specs/services.md` is structured as a dependency-ordered list, not a flat catalogue. This ordering will be preserved all the way into the generated Terraform code.

### ✅ Result

After the command completes:

- Open `specs/services.md` — you should see the service entries listed in dependency order, with their plans and the type of resource (entitlement, subscription, or service instance).
- Verify that the `free` plans for Audit Log and Cloud Logging are listed — these are the governance-compliant choices for `dev`.
- Check that HANA Cloud has both an `hana` entitlement entry and an `hdi-shared` service instance entry for the HDI container.
- Cross-check what has happened in the `memory/` folder.

**Key insight:** A senior architect could have derived this list from experience. The spec-driven approach encodes that expertise in the skill — a developer who has never set up HANA Cloud for CAP before still gets the correct dependency-ordered service list.

## 📋 Summary

All services, entitlements, and subscriptions are now specified in the correct dependency order. The next exercise defines the security configuration — the trust with SAP Cloud Identity Services, the role collections for inventory users, and the destination for S/4HANA connectivity.

Continue to - [Exercise 6 - Detail the security setup](../ex6/README.md)
