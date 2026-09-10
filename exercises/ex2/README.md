# Define the governance constraints

In this exercise, you will define the governance constraints for the inventory management project. These constraints are the **guardrails** — rules that the AI agent will enforce automatically at every subsequent step. They ensure the generated infrastructure complies with your company's policies before any code is written.

This is one of the most important exercises in the session because it demonstrates a key advantage of spec-driven development: **governance is encoded once and validated everywhere**, rather than being checked manually at the end (or not at all).

## The governance rules for our scenario

The manufacturing company running this project has the following policies:

| Category | Rule |
|---|---|
| **Region** | All BTP resources must be deployed in `cf-eu10` (Frankfurt). Data residency requirement — inventory data is subject to GDPR and must stay in the EU. |
| **Naming** | Subaccounts must follow the pattern `invmgmt-<env>`. Valid environment tiers are `dev`, `staging`, `prod`. Maximum name length: 60 characters. |
| **Service plans** | For the `dev` tier: SAP Audit Log Service and SAP Cloud Logging use the `free` plan. HANA Cloud uses the `hana` plan. Paid plans (e.g., `premium`) are forbidden in `dev`. |
| **Security** | SAP Cloud Identity Services is **required** as the identity provider (type: OIDC). The default SAP ID Service is not permitted as the application IdP. |
| **Cost controls** | HANA Cloud is a metered service — a warning must be issued. The tag key `cost-centre` is mandatory on all subaccounts. |

When you run `/btp-iac.govern`, the skill will ask you a targeted question for each category that is not already fully specified. Answer using the values from the table above.

## 🛠️ Execute the govern command

### Step 1 - Start the agentic tooling

Execute the following command in the terminal:

```bash
claude
```

> [!NOTE]
> Make sure that you start Claude Code inside the project folder you created in Exercise 1 (`SAPTechEd2026-XXX`).

### Step 2 - Execute the govern command

Execute the following command in the Claude Code terminal:

```bash
/btp-iac.govern
```

The skill will ask you up to one targeted question per governance category. For this session, use the following answers:

- **Regions:** `cf-eu10` is the only allowed region. Data residency reason: GDPR.
- **Naming:** Pattern is `invmgmt-<env>`. Tiers: `dev`, `staging`, `prod`.
- **Service plans:** For `dev`: `free` for Audit Log and Cloud Logging; `hana` for HANA Cloud. Forbidden: `premium` in dev.
- **Security:** Custom IdP required. Type: OIDC (SAP Cloud Identity Services). Default SAP ID Service is not permitted as application IdP.
- **Cost controls:** Metered service warning enabled for HANA Cloud. Tag key `cost-centre` is mandatory.

### 🔬 Look behind the curtains

The technical foundation of this step is the `btp-iac.govern.md` skill in the `.claude/commands` folder. Open the file and notice how the skill defines a precise governance schema — five categories, each with specific fields.

The skill writes its output to `memory/governance.md`. This file is **not a spec** — it is a contract. Every skill from this point forward will open `memory/governance.md` before doing anything and validate its decisions against it. If a later step tries to use a forbidden region or a forbidden service plan, the agent will stop with a clear error message pointing to the violated rule.

### ✅ Result

After the command completes:

- Check `memory/governance.md` — you should see all five categories populated with the rules you specified.
- Notice there is no `specs/` file yet — governance is stored in `memory/` because it is not a requirement, it is a constraint applied to all requirements.

**Key insight:** Without spec-driven development, these rules would live in a team wiki nobody reads. Here they are machine-readable and enforced automatically.

## 📋 Summary

The governance guardrails are now in place. Every skill from this point will validate against these rules. In the next exercise you will describe the inventory management application in plain language — and the AI agent will translate that into structured BTP infrastructure requirements.

Continue to - [Exercise 3 - Define the scenario](../ex3/README.md)
