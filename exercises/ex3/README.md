# Define the scenario

With the governance guardrails in place, you now describe **what you are building** in plain language. The `/sap-iac.scenario` skill translates your description into a structured set of infrastructure requirements — the first document in the `specs/` folder.

This is where spec-driven development shines: you do not write Terraform, you do not configure services — you describe the application and its needs from a functional perspective. The AI agent extracts the infrastructure signals and captures them in a machine-readable specification.

## The scenario

Your goal is to sketch the developer environment for the **Inventory Management CAP application** to define the overall scenario.

## 🛠️ Execute the scenario command

Execute the following command in the Claude Code terminal:

```bash
/sap-iac.scenario
```

The command will first check if there is an existing scenario defined in the project. As we start from scratch, it will prompt us to define a new scenario.

When prompted, use the following description as your input:

```text
In this scenario we focus on the development stage only. I need a developer environment on Cloud Foundry for our Inventory Management CAP application.

It stores data in SAP HANA Cloud. All create, update, and delete operations on inventory records must be logged to SAP Audit Log Service for compliance.

Application and runtime logs are collected in SAP Cloud Logging Service.

The UI is built with SAP Fiori and deployed via SAP Build Work Zone. Developers use SAP Build Code.
Developers also use the Continuous Integration & Delivery service (application only).

Authentication uses SAP Cloud Identity Services — the company does not permit the default SAP ID Service for this application.
```

The command will analyze the input. It will then ask some additonal questions to detail the technical configuration of the scenario:

**CF sizing/memory allocation:**

```text
Use a 1GB single instance.
```

**Integration:**  

```text
A destination to S/4HANA Public Cloud with Basic Authentication is required.
```

**Subaccount structure:**

```text
Each stage gets its own dedicated subaccount.
```

After answering these follow-up questions the agent will validate the availability of the entitlements on the global account.

After that an overview of the scenario description is presented to you. Claude will ask for confirmation before finalizing the scenario. Confirm by selecting `Yes`. This will create a new file in `specs/` as `scenario.md`.

### 🔬 Look behind the curtains

The technical foundation of this step is the `sap-iac.scenario.md` skill in the `.claude/commands` folder. Notice how the skill:

1. Accepts a plain-language description of the application
2. Identifies the runtime (`Cloud Foundry`), integration points (HANA Cloud, Audit Log, Cloud Logging, Build Work Zone, Build Code, IAS, S/4HANA destination), and environment isolation needs
3. Asks follow-up questions to detail the scenario requirements for the runtime, integration points and stage setup.
4. It validates the requirements against the available entitlements on the global account.
5. Writes the result to `specs/scenario.md` as structured requirements

The skill also checks whether an `sap-docs` MCP server is available — if it is, it uses it to look up current service names and plan details. If not, it falls back to web search to fetch the correct names.

### ✅ Result

After the command completes:

- Open `specs/scenario.md` — you should see a structured document describing the infrastructure requirements derived from your plain-language input.
- The document should capture: runtime (CF), services needed (HANA Cloud, Audit Log, Cloud Logging, Build Work Zone, Build Code, Cloud Identity Service etc), environment scope (single `dev` subaccount), and the S/4HANA connectivity requirement.
- Cross-check what has happened in the `memory/` folder.

**Key insight:** Compare the plain-language description you typed with `specs/scenario.md`. The skill has extracted the infrastructure signals that a developer would know (e.g., that a HANA Cloud instance needs an HDI container for CAP, that XSUAA is required for application security). This is knowledge the skill brings — you did not have to specify it.

## 📋 Summary

The scenario is now defined as structured infrastructure requirements. In the next exercise, the agent will translate this into a concrete account topology — determining which subaccounts to create, in which region, and how to name them.

Continue to - [Exercise 4 - Detail the account setup](../ex4/README.md)
