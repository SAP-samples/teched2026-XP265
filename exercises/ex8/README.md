# Define the tasks

You now have all specification files covering the account topology, the services, the security configuration and the connectivity setup. The `/sap-iac.tasks` skill consolidates all four into a **single dependency-ordered task list** — the direct input to code generation.

This step makes the ordering explicit and human-reviewable. You can read through the task list and verify: does the sequence make sense? Are any dependencies missing? Is there anything surprising? This is the last opportunity to adjust the specification before code is generated.

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can use as a **cross reference**. Do not copy it as it might not be perfectly fitting to the previous specifications that have been created by you.

## 🎯 Intent of the exercise

In this exercise, the agent consolidates all previously defined specifications into a single, dependency-ordered task list. This allows you to review and verify the sequence of tasks before generating the corresponding Terraform code. The goal is to ensure that all dependencies are correctly captured and that tasks that can be executed in parallel are identified.


## 🛠️ Execute the tasks command

Execute the following command in the Claude Code terminal:

```bash
/sap-iac.tasks
```

The skill reads `specs/landscape.md`, `specs/services.md`, `specs/trust.md`, and `specs/connectivity.md`. Based on the information it will derive the **dependency-ordered task list**.

The task sequence should be *similar* **(NOT NECESSARILY SAME)** to the file in the [sample folder](./sample/tasks.md).

Tasks that can be executed in parallel are marked accordingly and additional meta information about each task and its dependencies is provided.

Confirm the proposed task list by selecting `Yes`. This will create a new file in `specs/` as `tasks.md`.

### 🔬 Look behind the curtains

Open the `sap-iac.tasks.md` skill file. Notice how the skill:

1. Reads all four spec files and identifies the resources defined in each
2. Builds a dependency graph — for example, service instances cannot be created before the necessary entitlements are in place.
3. Identifies tasks that can run **in parallel** (e.g., creating the HANA Cloud instance and the Audit Log instance have no dependency on each other)
4. Assigns a task ID to each item so that the design step (Exercise 9) can annotate each task with the Terraform file it will be written to

The task list is the **bridge between specification and implementation** — it transforms the human-readable specs into something the code generator can execute methodically.

### ✅ Result

After the command completes:

- Open `specs/tasks.md` — you should see approximately 36 tasks in dependency order.
- Verify that HANA Cloud appears after the CF space and the entitlements.
- Verify that role collections appear after the IAS trust configuration.
- Verify that parallel execution markers are present for tasks with no mutual dependencies.

**Key insight:** This task list would take an experienced architect an hour to produce correctly on paper. The spec-driven approach produces it in seconds, from the specifications you have been building up over the past exercises — and it is correct by construction because it reads from validated spec files.

## 📋 Summary

The task list is defined and dependency-ordered. In the next exercise, the agent will map each task to a concrete Terraform file path — defining the folder and file structure of the generated code before writing a single line of HCL.

Continue to - [Exercise 9 - Evaluate the design](../ex9/README.md)
