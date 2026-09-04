# Define the tasks

You now have three specification files covering the account topology, the services, and the security configuration. The `/btp-iac.tasks` skill consolidates all three into a **single dependency-ordered task list** — the direct input to code generation.

This step makes the ordering explicit and human-reviewable. You can read through the task list and verify: does the sequence make sense? Are any dependencies missing? Is there anything surprising? This is the last opportunity to adjust the specification before code is generated.

## What the task list looks like for our scenario

The agent will consolidate the three spec files into a task sequence *similar* **(NOT NECESSARLY SAME)** to the following. Tasks that can be executed in parallel are marked accordingly.

| # | Task | Depends on | Notes |
|---|---|---|---|
| T01 | Create subaccount `invmgmt-dev` in `cf-eu10` | — | Root resource; everything else depends on this |
| T02 | Enable Cloud Foundry environment | T01 | CF org is created with the subaccount |
| T03 | Create CF space `development` | T02 | |
| T04 | Add entitlements for all services | T01 | Can run in parallel with T02-T03 |
| T05 | Subscribe to SAP Cloud Identity Services | T04 | IAS tenant needed before trust configuration |
| T06 | Subscribe to SAP Build Work Zone | T04 | |
| T07 | Subscribe to SAP Build Code | T04 | |
| T08 | Configure IAS trust (application IdP) | T05 | |
| T09 | Create XSUAA service instance | T04, T03 | |
| T10 | Create HANA Cloud service instance | T04, T03 | Metered — cost centre tag verified |
| T11 | Create HDI container service instance | T10 | Depends on HANA Cloud being available |
| T12 | Create Audit Log service instance | T04, T03 | |
| T13 | Create Cloud Logging service instance | T04, T03 | |
| T14 | Create Destination service instance | T04, T03 | |
| T15 | Create `S4HANA-MaterialMaster` destination | T14 | |
| T16 | Create role collection `InventoryViewer` | T08, T09 | |
| T17 | Create role collection `InventoryManager` | T08, T09 | |
| T18 | Create role collection `InventoryAdmin` | T08, T09 | |
| T19 | Assign IAS groups to role collections | T16, T17, T18 | |

## 🛠️ Execute the tasks command

Execute the following command in the Claude Code terminal:

```bash
/btp-iac.tasks
```

The skill reads `specs/landscape.md`, `specs/services.md`, and `specs/trust.md`.

### 🔬 Look behind the curtains

Open the `btp-iac.tasks.md` skill file. Notice how the skill:

1. Reads all three spec files and identifies the resources defined in each
2. Builds a dependency graph — for example, IAS trust (T08) cannot be configured before the IAS subscription (T05) exists, which in turn cannot be created before the entitlement (T04) is in place
3. Identifies tasks that can run **in parallel** (e.g., creating the HANA Cloud instance and the Audit Log instance have no dependency on each other)
4. Assigns a task ID to each item so that the design step (Exercise 8) can annotate each task with the Terraform file it will be written to

The task list is the **bridge between specification and implementation** — it transforms the human-readable specs into something the code generator can execute methodically.

### ✅ Result

After the command completes:

- Open `specs/tasks.md` — you should see approximately 18–20 tasks in dependency order.
- Verify that HANA Cloud (T10) appears after the CF space (T03) and the entitlements (T04).
- Verify that role collections (T16–T18) appear after the IAS trust configuration (T08).
- Verify that parallel execution markers are present for tasks with no mutual dependencies.
- Cross-check what has happened in the `memory/` folder.

**Key insight:** This task list would take an experienced BTP architect an hour to produce correctly on paper. The spec-driven approach produces it in seconds, from the specifications you have been building up over the past exercises — and it is correct by construction because it reads from validated spec files.

## 📋 Summary

The task list is defined and dependency-ordered. In the next exercise, the agent will map each task to a concrete Terraform file path — defining the folder and file structure of the generated code before writing a single line of HCL.

Continue to - [Exercise 8 - Evaluate the design](../ex8/README.md)
