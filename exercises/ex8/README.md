# Define the tasks

You now have all specification files covering the account topology, the services, the security configuration and the connectivity setup. The `/btp-iac.tasks` skill consolidates all four into a **single dependency-ordered task list** — the direct input to code generation.

This step makes the ordering explicit and human-reviewable. You can read through the task list and verify: does the sequence make sense? Are any dependencies missing? Is there anything surprising? This is the last opportunity to adjust the specification before code is generated.


## 🛠️ Execute the tasks command

Execute the following command in the Claude Code terminal:

```bash
/btp-iac.tasks
```

The skill reads `specs/landscape.md`, `specs/services.md`, `specs/trust.md`, and `specs/connectivity.md`. Based on the information it will derive the **dependency-ordered task list**.

The task sequence should be *similar* **(NOT NECESSARILY SAME)** to the following:

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-001 | Create subaccount invmgmt-dev | dev | — | — |
| T-002 | Enable Cloud Foundry environment | dev | T-001 | — |
| T-003 | Create Cloud Foundry space: dev | dev | T-002 | — |
| T-004 | Entitle + create SAP HANA Cloud instance | dev | T-001 | ✦ |
| T-005 | Entitle + create Audit Log Management Service instance | dev | T-001 | ✦ |
| T-006 | Entitle + create Cloud Logging instance | dev | T-001 | ✦ |
| T-007 | Entitle + subscribe SAP Business Application Studio | dev | T-001 | ✦ |
| T-008 | Entitle + subscribe Continuous Integration & Delivery | dev | T-001 | ✦ |
| T-009 | Entitle + subscribe SAP Build Work Zone, standard edition | dev | T-001 | ✦ |
| T-010 | Configure IdP trust (SAP Cloud Identity Services) | dev | T-001 | — |
| T-011 | Create role collection: InvMgmt_Dev_Admin | dev | T-010 | ✦ |
| T-012 | Create role collection: InvMgmt_Dev_Developer | dev | T-010 | ✦ |
| T-013 | Create role collection: InvMgmt_Dev_Viewer | dev | T-010 | ✦ |
| T-014 | Create role collection: InvMgmt_Dev_AuditLog_Viewer | dev | T-010 | ✦ |
| T-015 | Assign IAS group invmgmt-dev-admins → InvMgmt_Dev_Admin | dev | T-011 | ✦ |
| T-016 | Assign IAS group invmgmt-dev-developers → InvMgmt_Dev_Developer | dev | T-012 | ✦ |
| T-017 | Assign IAS group invmgmt-dev-viewers → InvMgmt_Dev_Viewer | dev | T-013 | ✦ |
| T-018 | Assign IAS group invmgmt-dev-audit-viewers → InvMgmt_Dev_AuditLog_Viewer | dev | T-014 | ✦ |
| T-019 | Create destination: S4HANA_CLOUD | dev | T-001 | — |

Tasks that can be executed in parallel are marked accordingly and additional meta information about each task is provided.

Confirm the proposed task list by selecting `Yes`. This will create a new file in `specs/` as `tasks.md`.

### 🔬 Look behind the curtains

Open the `btp-iac.tasks.md` skill file. Notice how the skill:

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

**Key insight:** This task list would take an experienced BTP architect an hour to produce correctly on paper. The spec-driven approach produces it in seconds, from the specifications you have been building up over the past exercises — and it is correct by construction because it reads from validated spec files.

## 📋 Summary

The task list is defined and dependency-ordered. In the next exercise, the agent will map each task to a concrete Terraform file path — defining the folder and file structure of the generated code before writing a single line of HCL.

Continue to - [Exercise 9 - Evaluate the design](../ex9/README.md)
