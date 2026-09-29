# Define the scenario

With the governance guardrails in place, you now describe **what you are building** in plain language. The agent using the `/sap-iac.scenario` skill translates your description into a structured set of infrastructure requirements resulting in the first document in the `specs/` folder.

This is where spec-driven development shines: you do not write Terraform, you do not configure services, but you describe the application and its needs from a functional perspective. The agent extracts the infrastructure signals and captures them in a machine-readable specification.

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can copy into the `specs` folder. After copying, make sure that you clean the context of the agent using the `/clear` command.

## 🎯 Intent of the exercise

Your goal is to sketch the developer environment for the **Inventory Management CAP application** to define the overall scenario.

Let's see what we can achieve via plain language description handed over to the agent.

## 🛠️ Execute the scenario command

Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.scenario
```

The agent will first check if there is an existing scenario defined in the project. As we start from scratch, it will ask you to define a new scenario.

Use the following description as input:

```text
In this scenario we focus on the development stage only. I need a developer environment on Cloud Foundry for our Inventory Management CAP application.

It stores data in SAP HANA Cloud. All create, update, and delete operations on inventory records must be logged to SAP Audit Log Service for compliance.

Application and runtime logs are collected in SAP Cloud Logging Service.

A destination service is also required.

The UI is built with SAP Fiori and deployed via SAP Build Work Zone. Developers use SAP Build Code.
Developers also use the Continuous Integration & Delivery service (application only).

Authentication uses SAP Cloud Identity Services — the company does not permit the default SAP ID Service for this application.
```

> [!TIP]
> While we defined governance rules for all stages, we focus here only on the development stage for the rest of the hands-on exercise.


The agent will analyze the input and validate it against your global account. It will then ask additional questions to detail the technical configuration of the scenario. Answer these questions to provide the necessary details as given here:

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

After you have answered the follow-up questions the agent will validate the availability of the entitlements on the global account.

When successful it will present an overview of the scenario description to you. The agent will ask for confirmation before finalizing the scenario and writing the specification document. Confirm by selecting `Yes`.

The agent will create a new file in `specs/scenario.md`.

### 🔬 Look behind the curtains

The technical foundation of this step is the skill `.claude/commands/sap-iac.scenario.md`. Open the file and notice how the skill:

1. Accepts a plain-language description of the application
2. Identifies the runtime (`Cloud Foundry`), integration points (HANA Cloud, Audit Log, Cloud Logging, Build Work Zone, Build Code, IAS, S/4HANA destination), and environment isolation needs
3. Asks follow-up questions to detail the scenario requirements for the runtime, integration points and stage setup.
4. Validates the requirements against the available entitlements on the global account.
5. Writes the result to `specs/scenario.md` as structured requirements

### ✅ Result

- The requirements deduced from your input are captured in `specs/scenario.md`
- The document also captures the details about runtime sizing, destinations and subaccount structure
- All requirements were validated against the previously defined governance constraints.

**Key insight:** Compare the plain-language description you typed with `specs/scenario.md`. You will notice that the agent extracted the relevant signals of your input into structured infrastructure requirements enriching it with the details it explicitly asked for during the follow-up questions.

## 📋 Summary

The scenario description is captured and translated into structured infrastructure requirements. In the next exercise, the agent will translate this into a concrete account topology determining which subaccounts to create, in which region, and how to name them.

Continue to - [Exercise 4 - Detail the account setup](../ex4/README.md)
