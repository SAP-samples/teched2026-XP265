# Define the governance constraints

In this exercise, you will define the *governance constraints* for the inventory management project. These constraints are rules that the agent will enforce automatically at every subsequent step. They ensure the generated infrastructure complies with your company's policies.

This is a central exercise in the session because it demonstrates a key advantage of spec-driven development: **governance is encoded once and validated everywhere**, rather than being checked manually at the end (or maybe not at all).

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can copy into the `memory` folder. After copying, make sure that you clean the context of the agent using the `/clear` command.

## 🎯 Intent of the exercise

With this exercise we want to make the agent aware of the governance constraints. The manufacturing company running this project has the following policies that we must enforce:

| Category | Rule |
|---|---|
| **Preferred infrastructure provider** | The preferred infrastructure provider is **Microsoft Azure**. |
| **Region** | The subaccount should be placed in the **EU** due to data residency requirements — inventory data is subject to GDPR and must stay in the EU. Therefore the **primary region** is `eu20`, **secondary region** is `eu22`. All other regions are forbidden. |
| **Environment** | The environment is **Cloud Foundry only**, no Kyma is used. Per stage one org and one space should be provided. |
| **Naming** | The naming pattern is given by `invmgmt-<stage>`. The allowed values for `<stage>` are `dev`, `staging`, `prod`. Maximum name length: 60 characters. The directory structure is flat i.e. only subaccounts. |
| **Service plans** | For the `dev` stage free plans should be used if available. HANA Cloud uses the `hana` plan. Paid plans (e.g., `premium`) are forbidden in `dev`. No restrictions for `staging` and `prod`. If multiple plans are available always use the `standard` plan for all stages. |
| **Security** | A custom IdP of type OIDC (SAP Cloud Identity Services) is required. The default SAP ID Service is not permitted as application IdP. There are no defaults with regards to role collections and user group mappings. |
| **Cost controls** | A warning should be raised for HANA Cloud as a metered service. A cost center label should be mandatory on the subaccounts. The label key is `cost-center`, the label value is `CC-123456`. |

Let's see how we can transfer these constraints using our spec-driven development approach.

## 🛠️ Execute the govern command

> [!TIP]
> The easiest way to walk through the exercises is to open an embeded terminal within VS Code. To do so use the key combination `Ctrl + Shift + P` and then select `Terminal: Create New Terminal in Editor Area`.

### Step 1 - btp CLI login

👉 Navigate to the terminal from the previous exercise and execute the following command to log in to the btp CLI:

```powershell
btp login --idp ainfvn15r --user 'XP265-<Group Number>' --password '<your-password>'
```

👉 Target the global account subdomain you want to use (copy the value from the SAP BTP cockpit):

```powershell
btp target --global-account '<your-global-account-subdomain>'
```

> [!NOTE]
> Being logged in to the btp CLI allows the agent to fetch live data from your global account to check whether the constraints can be applied.

### Step 2 - Start the agentic tooling

Make sure that in your terminal you are in the project folder you created in Exercise 1 (`SAPTechEd2026-0XX`).

👉 Execute the following command in the terminal:

```powershell
claude
```

Claude will ask you if you trust this folder.

👉 Confirm by selecting `Yes`.

In addition, **make sure** that you are in **manual mode** of Claude Code. You can cycle through the modes via the key combination `Shift + Tab`.

### Step 3 - Execute the govern command

> [!NOTE]
> LLMs are probabilistic, so the agent's output may look a little different each time. Questions might appear as free text or as a list, and their order within a section can vary. *No need to worry* — just read each question carefully and enter or select the answer based on the values provided in the exercises.


👉 Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.govern
```

> [!NOTE]
> If Claude Code does not recognize the command, make sure that you started Claude Code **inside the project folder** you created in the previous exercise (`SAPTechEd2026-0XX`).


The agent will check the basic layout of your project folder to ensure it meets the expected structure.

> [!NOTE]
> You might be asked to confirm the execution of some PowerShell as well as btp CLI commands. <br>
> **Allow** the agent to scan the project folder and run btp CLI commands. <br>
> 👉 Confirm by selecting `Yes`. <br>
> Once the agent has done this, it will target the btp CLI to your global account.

The agent will then ask you targeted questions about the governance requirements.

👉 Use/Copy the following answers. Select `Other` or `Type something` if the proposals of Claude Code do not fully cover your intent:

👉 **Preferred infrastructure provider:**

```text
Microsoft Azure
```

👉 **Regions:**

```text
The subaccount should be placed in the EU, primary region is `eu20`, secondary region is `eu22`.
All other regions are forbidden.
Data residency requirement — inventory data is subject to GDPR and must stay in the EU.
```

👉 **Environment:**

```text
The environment should be Cloud Foundry.
No Kyma is used in this project.
One org and one space per stage should be created.
```

👉 **Naming conventions:**

```text
The naming pattern is given by `invmgmt-<stage>`.
The allowed values for `<stage>` are `dev`, `staging`, `prod`.
Maximum name length: 60 characters.
Kyma is not used in this project.
For CF orgs the naming convention is `invmgmt-cf-<stage>`, the CF space is then `<stage>`.
No directories are required.
```

👉 **Service plans:**

```text
For the `dev` stage: SAP Audit Log Service and SAP Cloud Logging use the `standard` plan.
HANA Cloud uses the `hana` plan.
Paid plans (e.g., `premium`) are forbidden in `dev`.
If there is a free plan in `dev` use it.
If multiple plans are available but no free plan, always use the `standard` plan for all stages.
No restrictions for `staging` and `prod`.
```

👉 **Security:**

```text
Custom IdP required. Type: OIDC (SAP Cloud Identity Services).
The default SAP ID Service is not permitted as application IdP.
There are no defaults with regards to role collections and user group mappings.
```

👉 **Cost controls:**

```text
A warning should be enabled for HANA Cloud as a metered service.
A cost center label should be mandatory on the subaccounts.
The label key is `cost-center`, the label value is `CC-123456`.
```

Once you have answered all questions, the agent will summarize the governance rules and ask you to confirm if the captured data is correct. Be aware that the summary might be opened as a document in a new tab in VS Code. Review it but do not close the tab.

👉  Confirm by selecting `Yes`.

> [!NOTE]
> If you closed the tab with the summary, Claude will interrupt the flow. <br>
> 👉 Type `Continue and write the document` in the Claude Code terminal to resume the process.

🤖 The agent will create a new file in `memory/` called `governance.md`.

<details>

<summary><b>🔬 Look behind the curtains</b></summary>

The technical foundation of this step is skill `.claude/commands/sap-iac.govern.md`. It is used by the agent to collect your governance requirements.

Open the file and notice how the skill defines a governance schema. This structure is reflected in the `memory/governance.md` file that contains the captured governance rules as markdown.

The agent writes its output to `memory/governance.md`. This file is **not a spec** but a contract. From this point forward every agent execution in the spec-kit flow  will open `memory/governance.md` before doing anything and validate its decisions against it. If a later step tries to use a forbidden region or a excluded service plan, the agent will stop with an error message pointing to the violated rule.
</details>

### ✅ Result

- The governance requirements collected by the agent are stored in `memory/governance.md`.
- There are no `specs/` files yet as governance is stored in `memory/` because it is not a requirement, but a constraint applied to all specification documents.

**Key insight:** Without spec-driven development, these rules would live in a team wiki, a Jira ticket or in the worst case in the head of the person responsible. In any case they will not be enforced consistently. Here they are stored as markdown and enforced automatically in the consequent steps.

## 📋 Summary

The governance guardrails are in place.In the next exercise you will describe the inventory management application in plain language and the agent will translate that into structured infrastructure requirements.

👉 Continue to - [Exercise 4 - Define the scenario](../ex4/README.md)
