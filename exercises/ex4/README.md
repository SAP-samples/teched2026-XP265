# Detail the account setup

As the scenario is defined, the agent needs to translate them into a concrete account topology.

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can use as a **cross reference**. Do **not** copy it as it might not be perfectly fitting to the previous specifications that have been created by you.

## 🎯 Intent of the exercise

Based on the scenario definition and the governance rules the agent using the `/sap-iac.accounts` skill will determine the appropriate account setup automatically.

In addition, it will make a proposal for the `beta_enabled` and `usage` attributes based on the governance rules and the environment.

## 🛠️ Execute the accounts command

Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.accounts
```

The agent reads `specs/scenario.md` and `memory/governance.md`. As you already provided all information necessary for the account setup, it will present you the summary including the passed governance checks. The result should look similar to this:

| Field | Value |
|---|---|
| Subaccount name | invmgmt-dev |
| Region | eu20 |
| Subdomain | invmgmt-dev |
| Environment | Cloud Foundry enabled |
| CF org | Automatically created with the subaccount |
| CF space | dev |
| Cost center tag | CC-123456 (as defined by governance) |

The agent will also derive a setting for the `beta_enabled` and the `usage` attribute of the subaccount.

Accept the proposed setup by keying in `Accept`.

The Agent will present the final summary and ask for confirmation before writing the specification document. Confirm by selecting `Yes`.

The agent will create a new file in `specs/landscape.md`.

<details>

<summary><b>🔬 Look behind the curtains</b></summary>

The technical foundation of this step is the skill `.claude/commands/sap-iac.accounts.md`. Open the file and notice how the skill:

1. Starts with a governance check
2. Derives the account setup based on the governance rules and the scenario
2. Derives of the `beta_enabled` and `usage` attributes
3. Writes the result into `specs/landscape.md`
</details>

### ✅ Result

- The topology of your account setup is captured in `specs/landscape.md`
- The whole setup was automatically derived by the agent based on the scenario and governance rules.
- All requirements were validated against the previously defined governance constraints.

**Key insight:** The agent chose `eu20` not because it is the default, but because it is the preferred region per the governance file. If you were to repeat this flow for a project with no governance file, the agent would ask which region to use. The governance file removes that ambiguity as you defined this information once as guardrail.

## 📋 Summary

The account topology is defined and governance-validated. The next exercise resolves which services, entitlements, and subscriptions are needed and can be used based on the governance rules.

Continue to - [Exercise 5 - Detail the service setup](../ex5/README.md)
