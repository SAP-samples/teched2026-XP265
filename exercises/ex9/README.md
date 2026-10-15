# Define the tasks

We have all specification documents covering the account topology, service setup, security configuration, and connectivity configuration in place. These documents will now be translated into a task list that serves as input for code generation.

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can use as a **cross reference**. Do not copy it as it might not be perfectly fitting to the previous specifications that have been created by you.

## 🎯 Intent of the exercise

In this exercise, the agent consolidates all previously defined specifications into a single, dependency-ordered task list. This allows you to review and verify the tasks before generating the corresponding Terraform code. The goal is to ensure that all tasks as well as their dependencies are correctly captured.


## 🛠️ Execute the tasks command

👉 Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.tasks
```

Based on all specification documents and the governance constraints, the agent derives a task list including metadata and dependencies between tasks.

The result should look *similar* to the file in the [sample folder](./sample/tasks.md).

👉 Confirm the proposed task list by selecting `Yes`.

The agent will create a new file `specs/tasks.md`.

<details>

<summary><b>🔬 Look behind the curtains</b></summary>

The technical foundation of this step is the skill `.claude/commands/sap-iac.tasks.md`. Open the file and notice how the skill:

1. Reads all specification documents and identifies the resources defined in each
2. Builds a dependency graph and identifies a task sequence based on dependencies
</details>

**Key insight:** The task list acts as the bridge between specification and implementation, basically a ordered todo list that steers the code generation process.

### ✅ Result

- The task list is available in `specs/tasks.md`.
- The list reflects the required tasks described in all previous steps in a consistent and dependency-ordered manner.

## 📋 Summary

The task list translating the specifications into a checklist is defined and dependency-ordered. The next exercise will enrich the task list with a mapping to the corresponding Terraform files.

👉 Continue to - [Exercise 10 - Evaluate the design](../ex10/README.md)
