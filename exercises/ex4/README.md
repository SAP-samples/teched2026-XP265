# Detail the account setup

The scenario is defined. Now the agent needs to translate the abstract requirements into a concrete BTP account topology: which subaccounts to create, in which region, with which Cloud Foundry configuration, and how they are named.

This step is also where the governance guardrails run for the first time. Before writing anything, the `/btp-iac.accounts` skill opens `memory/governance.md` and validates every decision it makes.

## What the account topology looks like for our scenario

Based on the scenario definition (a single `dev` environment), the expected result is:

| Field | Value |
|---|---|
| Subaccount name | `invmgmt-dev` |
| Region | `eu10` (Frankfurt) |
| Subdomain | `invmgmt-dev` (must be globally unique — you may be asked to adjust) |
| Environment | Cloud Foundry enabled |
| CF org | Automatically created with the subaccount |
| CF space | `development` |
| Cost centre tag | Required — you will provide your team's cost centre identifier |

The governance rules will be applied automatically:

- `eu10` is the only allowed region — the agent checks this before creating the subaccount resource ✅
- The name `invmgmt-dev` matches the `invmgmt-<env>` pattern ✅
- The tier `dev` is a defined environment tier ✅

If any of these checks failed, the agent would stop with an error message describing the exact rule violated — rather than silently generating non-compliant infrastructure.

## 🛠️ Execute the accounts command

Execute the following command in the Claude Code terminal:

```bash
/btp-iac.accounts
```

The skill reads `specs/scenario.md` and `memory/governance.md`. As some information is missing, it will ask you for the following information:

- Your **global account subdomain** (needed for the Terraform provider configuration)
- The Cloud Foundry setup.

Look up the Global account subdomain in the SAP BTP Cockpit and provide the following input when prompted:

```text
The global account subdomain of my account is <your-subdomain>.
The Cloud Foundry setup should be one CF environment per subaccount. The naming convention of the org should follow `invmgmt-<env>` as for the subaccount. Within each org, create a space that is named as the stage of the subaccount namely dev, staging and prod.
```

All checks should pass. After answering these follow-up questions an overview of the landscape description is presented to you. Claude will ask for confirmation before finalizing the accounts setup. Confirm by selecting `Yes`. This will create a new file in `specs/` as `landscape.md`.

### 🔬 Look behind the curtains

Open the `btp-iac.accounts.md` skill file. Notice:

1. The skill starts with a **governance check** — it loads `memory/governance.md` and prepares to validate against it before writing anything.
2. Region validation: if you somehow specified a region outside `eu10`, the skill would `STOP` — not warn, not continue, `STOP`.
3. Naming validation: the `invmgmt-<env>` pattern is enforced. A subaccount named `inventory-development` would fail.
4. The output — `specs/landscape.md` — becomes the **authoritative account structure** that the services and security skills read in the next exercises. It is not regenerated later; it is referenced.

### ✅ Result

After the command completes:

- Open `specs/landscape.md` — you should see the subaccount definition for `invmgmt-dev` in region `eu10`, with the CF environment enabled and a `development` space.
- Verify the cost centre tag is present in the subaccount definition.
- Cross-check what has happened in the `memory/` folder.

**Key insight:** The agent chose `cf-eu10` not because it is the default, but because it is the only allowed region per the governance file. If you were to repeat this flow for a project with no governance file, the agent would ask which region to use. The governance file removes that ambiguity — it is answered once and enforced everywhere.

## 📋 Summary

The BTP account topology is now defined and governance-validated. The next exercise resolves which SAP BTP services, entitlements, and subscriptions are needed — and validates each service plan choice against the governance rules.

Continue to - [Exercise 5 - Detail the service setup](../ex5/README.md)
