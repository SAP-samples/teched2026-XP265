# Define the scenario

With governance guardrails in place, you now describe **what you are building** in plain language. The `/btp-iac.scenario` skill translates your description into a structured set of BTP infrastructure requirements — the first document in the `specs/` folder.

This is where spec-driven development shines: you do not write Terraform, you do not configure services — you describe the application and its needs as a business person would. The AI agent extracts the infrastructure signals and captures them in a machine-readable specification.

## The scenario prompt

You are setting up the developer environment for the **Inventory Management CAP application**. Use the following description as your prompt when running the command:

> I need a developer environment on Cloud Foundry for our Inventory Management CAP application. It stores data in SAP HANA Cloud. All create, update, and delete operations on inventory records must be logged to SAP Audit Log Service for compliance. Application and runtime logs are collected in SAP Cloud Logging Service. The UI is built with SAP Fiori and deployed via SAP Build Work Zone. Developers use SAP Build Code for the CAP tooling and CI/CD pipeline. Authentication uses SAP Cloud Identity Services — the company does not permit the default SAP ID Service for this application.



## 🛠️ Execute the scenario command

Execute the following command in the Claude Code terminal:

```bash
/btp-iac.scenario
```

When prompted, paste or type the scenario description above. Answer the follow-up questions as described.

### 🔬 Look behind the curtains

The technical foundation of this step is the `btp-iac.scenario.md` skill in the `.claude/commands` folder. Notice how the skill:

1. Accepts a plain-language description of the application
2. Identifies the runtime (`Cloud Foundry`), integration points (HANA Cloud, Audit Log, Cloud Logging, Build Work Zone, Build Code, IAS, S/4HANA destination), and environment isolation needs
3. Asks **at most three follow-up questions** — it does not bombard you with an endless form
4. Writes the result to `specs/scenario.md` as structured requirements

The skill also checks whether an `sap-docs` MCP server is available — if it is, it uses it to look up current service names and plan details. If not, it falls back to web search.

### ✅ Result

After the command completes:

- Open `specs/scenario.md` — you should see a structured document describing the infrastructure requirements derived from your plain-language input.
- The document should capture: runtime (CF), services needed (HANA Cloud, Audit Log, Cloud Logging, Build Work Zone, Build Code, Cloud Identity Service etc), environment scope (single `dev` subaccount), and the S/4HANA connectivity requirement.
- Cross-check what has happened in the `memory/` folder.

**Key insight:** Compare the plain-language description you typed with `specs/scenario.md`. The skill has extracted the infrastructure signals that a developer familiar with BTP would know (e.g., that a HANA Cloud instance needs an HDI container for CAP, that XSUAA is required for application security). This is knowledge the skill brings — you did not have to specify it.

## 📋 Summary

The scenario is now defined as structured infrastructure requirements. In the next exercise, the agent will translate this into a concrete BTP account topology — determining which subaccounts to create, in which region, and how to name them.

Continue to - [Exercise 4 - Detail the account setup](../ex4/README.md)
