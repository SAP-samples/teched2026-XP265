# Detail the service setup

With the account topology defined, the agent resolves the full set of services required for the scenario comprising entitlements, subscriptions, and service instances.

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can use as a **cross reference**. Do **not** copy it as it might not be perfectly fitting to the previous specifications that have been created by you.

## 🎯 Intent of the exercise

Based on the input we provided in the previous exercises the agent will derive the full set of services required for the scenario. We will review the list and provide detailed information as we left some ambiguity in the scenario that needs to be sorted out by us.

## 🛠️ Execute the services command

Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.services
```

The agent reads `specs/scenario.md`, `specs/landscape.md`, and `memory/governance.md`. The scenario description already contains all the signals to derive the required services. The skill will validate the settings using the btp CLI. You might be prompted to accept these calls of the btp CLI.

After successful validation it will check for available plans and categories of the services. If adjustments concerning the selections are required, it will ask you to confirm them. Confirm the proposals.

### Ambiguities in the scenario

However, we left some ambiguity in the scenario when it comes to the question if a service/application should be entitled only and in case of services in which environment the service instance should be created. As this information cannot be inferred automatically, we need to provide explicit guidance.

> [!TIP]
> The flow considers any rules defined in governance. If we would have defined this information already there, less manual decision-making (and probably less chances to make errors) would have been required.

#### Choice of entitlement vs. instance creation/app subscription

The agent will ask you for each service/app whether you want to create a service instance/app subscription or if you only want to entitle it.

Confirm the default choice for each service/app by stating `default`.

#### Choice of deployment target for services

It will also ask you about the deployment target for the services as you can deploy them to Cloud Foundry or directly to the platform (environment "Other" in the cockpit).

Select `BTP` *except for the destination service* that should be deployed to Cloud Foundry.

### Provide service parameters

As some services require parameters, the skill will ask you to provide the necessary values.

For **hana-cloud/hana** key in the following values:

```text
memory: 32
edition: cloud
generateSystemPassword: true
```

For handling optional parameters of HANA Cloud enter the following statement:

```text
Use default or leave empty for all optional parameters.
```

For **destination/lite** key in the following values:

```text
HTML5Runtime_enabled true
```

After answering the questions the agent will present you the summary including the passed governance checks for confirmation before proceeding. Confirm by selecting `Yes`.

The agent will create a new file in `specs/services.md`.

### 🔬 Look behind the curtains

The technical foundation of this step is the skill `.claude/commands/sap-iac.services.md`. Open the file and notice how the skill:

1. The skill builds the service list.
2. For each service, it identifies the stage from `specs/landscape.md` (in this case `dev`) and validates the plan choice against the governance file.
3. The skill asks for each service/app if it should be provisioned or entitled only as well as into which environment it should be provisioned to if no restrictions are already defined in the governance step.
4. The output `specs/services.md` contains a dependency-order that will be preserved for the Terraform code generation.

### ✅ Result

After the command completes:

- The service-specific requirements are collected in the `specs/services.md` document.
- A consistent description of what services get deployed in which environment and which services/apps are entitlement-only.

**Key insight:** Even a experienced administrator benefits from the spec-driven approach, as it ensures that all service/app specific information is captured including required parameters.

## 📋 Summary

All services and subscriptions are specified. The next exercise defines the security configuration namely the trust with SAP Cloud Identity Services and the role collections for inventory users.

Continue to - [Exercise 6 - Detail the security setup](../ex6/README.md)
