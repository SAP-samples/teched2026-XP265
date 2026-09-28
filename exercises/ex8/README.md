# Define the tasks

We have all specification documents covering the account topology, the services, the security configuration and the connectivity setup in place. These documents will now be translated into a actionable task list that serves as input for the code generation.

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can use as a **cross reference**. Do not copy it as it might not be perfectly fitting to the previous specifications that have been created by you.

## 🎯 Intent of the exercise

In this exercise, we instruct the agent to consolidates all previously defined specifications into a single, dependency-ordered task list. This allows you to review and verify the sequence of tasks before generating the corresponding Terraform code. The goal is to ensure that all dependencies are correctly captured and that tasks that can be executed in parallel are identified.


## 🛠️ Execute the tasks command

Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.tasks
```

Based on all specification documents and the government constraints, the agent derives a dependency-ordered task list.

The task sequence should be *similar* to the file in the [sample folder](./sample/tasks.md).

Tasks that can be executed in parallel are marked accordingly and additional meta information about each task and its dependencies is provided.

Confirm the proposed task list by selecting `Yes`. The agent will create a new file `specs/tasks.md`.

### 🔬 Look behind the curtains

The technical foundation of this step is the skill `.claude/commands/sap-iac.tasks.md`. Open the file and notice how the skill:

1. Reads all four spec files and identifies the resources defined in each
2. Builds a dependency graph — for example, service instances cannot be created before the necessary entitlements are in place.
3. Identifies tasks that can run in parallel
4. Assigns a task ID to each item so that the design step ([Exercise 9](../ex9/README.md)) can annotate each task with the Terraform file it will be written to

**Key insight:** The task list acts as the bridge between specification and implementation. It transforms the specification documents into a list of tasks required to be executed when generating the code.

### ✅ Result

- The task list is available in `specs/tasks.md`.
- The list reflects the required tasks described in all previous steps in a consistent and dependency-ordered manner.

## 📋 Summary

The task list translating the specifications into an actionable check list is defined and dependency-ordered. The next exercise will enrich the task list with a mapping to the corresponding Terraform files.

Continue to - [Exercise 9 - Evaluate the design](../ex9/README.md)
