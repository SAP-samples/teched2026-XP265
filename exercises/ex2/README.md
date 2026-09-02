# Define the governance constraints

In this exercise, you will define the governance constraints of the project setup as a first step in the spec-driven development flow. These constraints serve as a guardrail for the project and will be used to validated in each step. They are a very important step in the spec-driven development flow as they ensure that the generated code adheres to the constraints and policies of your organization.

## 🛠️ Execute the govern command

## Step 1 - Start the agentic tooling

Execute the following command in the terminal:

```bash
claude
```

> [!NOTE]
> Make sure that you start claude inside of the folder that you have created (`SAPTechEd2026-XXX`).

## Step 2 - Execute the govern command

Execute the following command in the claude terminal:

```bash
/btp-iac.govern
```

### 🔬 Look behind the curtains

The technical foundation of the step is the `btp-iac.govern.md` skill located in the `.claude/commands` folder. The skill contains the instructions for the agentic tooling covering some important governance constraints. If you are curious check out what is contained in this file.

### ✅ Result

- Result + Check the created markdown file in the `specs` folder. Cross check what has happened in the `memory` folder.

## 📋 Summary

Now that you have ...
Continue to - [Exercise 3 - Define the scenario](../ex3/README.md)
