# Define the governance constraints

In this exercise, you will define the governance constraints for the inventory management project. These constraints are the **guardrails** — rules that the AI agent will enforce automatically at every subsequent step. They ensure the generated infrastructure complies with your company's policies before any code is written.

This is one of the most important exercises in the session because it demonstrates a key advantage of spec-driven development: **governance is encoded once and validated everywhere**, rather than being checked manually at the end (or not at all).

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can copy into the `memory` folder. After copying, make sure that you clean the context of the agent via the `/clear` command.

## 🎯 Intent of the exercise

The manufacturing company running this project has the following policies:

| Category | Rule |
|---|---|
| **Preferred infrastructure provider:** | The preferred infrastructure provider is Microsoft Azure. |
| **Provider** | The preferred infrastructure provider is Microsoft Azure. |
| **Region** | The subaccount should be placed in the EU, primary region is `eu10`, secondary region is `eu20`. All other regions are forbidden. Data residency requirement — inventory data is subject to GDPR and must stay in the EU. |
| **Environment** | The environment is Cloud Foundry only (no Kyma is used here). One org and one space per stage should be created. |
| **Naming** | The naming pattern is given by `invmgmt-<stage>`. The allowed values for `<stage>` are `dev`, `staging`, `prod`. Maximum name length: 60 characters. The directory structure is flat. |
| **Service plans** | For the `dev` stage: SAP Audit Log Service and SAP Cloud Logging use the `free` plan. HANA Cloud uses the `hana` plan. Paid plans (e.g., `premium`) are forbidden in `dev`. No restrictions for `staging` and `prod`. If multiple plans are available always use the `standard` plan for all stages. |
| **Security** | Custom IdP required. Type: OIDC (SAP Cloud Identity Services). The default SAP ID Service is not permitted as application IdP. There are no defaults with regards to role collections and user group mappings. |
| **Cost controls** | A warning should be enabled for HANA Cloud as a metered service. A cost center label should be mandatory on the subaccounts. The label key is `cost-center`, the label value is `CC-123456`. |

In this exercise we will make the spek-kit aware of the governance constraints by executing the `/sap-iac.govern` command.

## 🛠️ Execute the govern command

### Step 0 - btp CLI login

Execute the following command in the terminal to log in to the BTP CLI:

```bash
btp login --idp ainfvn15r --user 'XP265-<Group Number>' --password '<your-password>'
```

And target the global account subdomain you want to use:

```bash
btp target --global-account '<your-global-account-subdomain>'
```

### Step 1 - Start the agentic tooling

Navigate into the project folder you created in Exercise 1 (`SAPTechEd2026-XXX`). Execute the following command in the terminal:

```bash
claude
```

Claude will ask you if you trust this folder, confirm by selecting `Yes`.

In addition make sure that you are in *manual mode*. You can cycle through the modes via `Shift + Tab`.

### Step 2 - Execute the govern command

Execute the following command in the Claude Code terminal:

```bash
/sap-iac.govern
```

> [!NOTE]
> If Claude Code does not recognize the command, make sure that you started Claude Code **inside the project folder** you created in Exercise 1 (`SAPTechEd2026-XXX`).

The skill will check the basic layout of your project folder to ensure it meets the expected structure. You might be asked to confirm the execution of some PowerShell commands as well as the btp CLI. Do so to allow the agent to scan the project folder. Once it has done this, it will target the btp CLI to your global account. Confirm by selecting `Yes`.

It will then proceed to ask you the targeted governance questions.

The skill will ask you up to one targeted question per governance category. For this session, use the following answers:

**Preferred infrastructure provider:**

```text
Microsoft Azure
```

**Regions:**

```text
The subaccount should be placed in the EU, primary region is `eu20`, secondary region is `eu22`.
All other regions are forbidden.
Data residency requirement — inventory data is subject to GDPR and must stay in the EU.
```

**Environment:**

```text
The environment should be Cloud Foundry.
No Kyma is used in this project.
One org and one space per stage should be created.
```

**Naming:**

```text
The naming pattern is given by `invmgmt-<stage>`.
The allowed values for `<stage>` are `dev`, `staging`, `prod`.
Maximum name length: 60 characters.
Kyma is not used in this project.
For CF orgs the naming convention is `invmgmt-cf-<stage>`, the CF space is then `<stage>`.
No directories are required.
```

**Service plans:**

```text
For the `dev` stage: SAP Audit Log Service and SAP Cloud Logging use the `standard` plan.
HANA Cloud uses the `hana` plan.
Paid plans (e.g., `premium`) are forbidden in `dev`.
If there is a free plan in `dev` use it.
If multiple plans are available but no free plan, always use the `standard` plan for all stages.
No restrictions for `staging` and `prod`.
```

**Security:**

```text
Custom IdP required. Type: OIDC (SAP Cloud Identity Services). The default SAP ID Service is not permitted as application IdP. There are no defaults with regards to role collections and user group mappings.
```

**Cost controls:**

```text
A warning should be enabled for HANA Cloud as a metered service. A cost center label should be mandatory on the subaccounts. The label key is `cost-center`, the label value is `CC-123456`.
```

Once all questions have been answered, the skill will summarize the governance rules and ask you to confirm if the captured data is correct. Confirm by selecting `Yes`. This will create a new file is in `memory/` as `governance.md`.

### 🔬 Look behind the curtains

The technical foundation of this step is the `sap-iac.govern.md` skill in the `.claude/commands` folder. Open the file and notice how the skill defines a precise governance schema — each category with specific fields.

The skill writes its output to `memory/governance.md`. This file is **not a spec** — it is a contract. Every skill from this point forward will open `memory/governance.md` before doing anything and validate its decisions against it. If a later step tries to use a forbidden region or a forbidden service plan, the agent will stop with a clear error message pointing to the violated rule.

### ✅ Result

After the command completes:

- Check `memory/governance.md` — you should see all categories populated with the rules you specified.
- Notice there is no `specs/` file yet — governance is stored in `memory/` because it is not a requirement, it is a constraint applied to all requirements.

**Key insight:** Without spec-driven development, these rules would live in a team wiki nobody reads. Here they are machine-readable and enforced automatically.

## 📋 Summary

The governance guardrails are in place. Every skill from this point will validate against these rules. In the next exercise you will describe the inventory management application in plain language — and the AI agent will translate that into structured infrastructure requirements.

Continue to - [Exercise 3 - Define the scenario](../ex3/README.md)
