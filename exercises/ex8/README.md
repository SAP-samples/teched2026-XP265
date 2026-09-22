# Define the tasks

You now have all specification files covering the account topology, the services, the security configuration and the connectivity setup. The `/btp-iac.tasks` skill consolidates all four into a **single dependency-ordered task list** — the direct input to code generation.

This step makes the ordering explicit and human-reviewable. You can read through the task list and verify: does the sequence make sense? Are any dependencies missing? Is there anything surprising? This is the last opportunity to adjust the specification before code is generated.


## 🛠️ Execute the tasks command

Execute the following command in the Claude Code terminal:

```bash
/btp-iac.tasks
```

The skill reads `specs/landscape.md`, `specs/services.md`, `specs/trust.md`, and `specs/connectivity.md`. Based on the information it will derive the **dependency-ordered task list**.

The task sequence should be *similar* **(NOT NECESSARILY SAME)** to the following.

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-001 | Create subaccount `invmgmt-dev` (region: `eu20`, cost-centre: `CC-123456`) | dev | — | ✦ |
 T-002 | Enable Cloud Foundry environment — org `invmgmt-cf-dev` | dev | T-001 | — |
| T-003 | Create CF space `dev` in org `invmgmt-cf-dev` | dev | T-002 | — |
| T-004 | Assign entitlement `xsuaa` / `application` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-005 | Assign entitlement `html5-apps-repo` / `app-host` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-006 | Assign entitlement `html5-apps-repo` / `app-runtime` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-007 | Assign entitlement `destination` / `lite` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-008 | Assign entitlement `auditlog` / `oauth2` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-009 | Assign entitlement `cloud-logging` / `dev` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-010 | Assign entitlement `hana-cloud` / `hana` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-011 | Assign entitlement `hana` / `hdi-shared` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-012 | Assign entitlement `identity` / `application` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-013 | Assign entitlement `build-workzone-standard` / `standard` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-014 | Assign entitlement `build-code` / `free` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-015 | Assign entitlement `cicd-app` / `free` to `invmgmt-dev` | dev | T-001 | ✦ |
| T-016 | Subscribe to `build-workzone-standard` / `standard` | dev | T-013 | ✦ |
| T-017 | Subscribe to `build-code` / `free` | dev | T-014 | ✦ |
| T-018 | Subscribe to `cicd-app` / `free` | dev | T-015 | ✦ |
| T-019 | Create service instance `invmgmt-xsuaa` (`xsuaa` / `application`) in CF space `dev` | dev | T-004, T-003 | ✦ |
| T-020 | Create service instance `invmgmt-destination` (`destination` / `lite`) in CF space `dev` | dev | T-007, T-003 | ✦ |
| T-021 | Create service instance `invmgmt-auditlog` (`auditlog` / `oauth2`) in CF space `dev` | dev | T-008, T-003 | ✦ |
| T-022 | Create service instance `invmgmt-cloud-logging` (`cloud-logging` / `dev`) in CF space `dev` | dev | T-009, T-003 | ✦ |
| T-023 | Create service instance `invmgmt-hana-cloud` (`hana-cloud` / `hana`) — subaccount level | dev | T-010 | — |
| T-024 | Create service instance `invmgmt-hdi-container` (`hana` / `hdi-shared`) in CF space `dev` | dev | T-011, T-023, T-003 | — |
| T-025 | Create service instance `invmgmt-identity` (`identity` / `application`) in CF space `dev` | dev | T-012, T-003 | ✦ |
| T-026 | Create service instance `invmgmt-html5-host` (`html5-apps-repo` / `app-host`) in CF space `dev` | dev | T-005, T-003 | ✦ |
| T-027 | Configure custom IdP trust — SAP Cloud Identity Services (OIDC) on `invmgmt-dev` | dev | T-025 | — |
| T-028 | Create role collection `InvMgmt_Dev_Admin` | dev | T-027 | ✦ |
| T-029 | Create role collection `InvMgmt_Dev_Developer` | dev | T-027 | ✦ |
| T-030 | Create role collection `InvMgmt_Dev_Viewer` | dev | T-027 | ✦ |
| T-031 | Create role collection `InvMgmt_Dev_AuditLog_Viewer` | dev | T-027 | ✦ |
| T-032 | Map IAS group `invmgmt-dev-admins` → role collection `InvMgmt_Dev_Admin` | dev | T-028 | ✦ |
| T-033 | Map IAS group `invmgmt-dev-developers` → role collection `InvMgmt_Dev_Developer` | dev | T-029 | ✦ |
| T-034 | Map IAS group `invmgmt-dev-viewers` → role collection `InvMgmt_Dev_Viewer` | dev | T-030 | ✦ |
| T-035 | Map IAS group `invmgmt-dev-audit-viewers` → role collection `InvMgmt_Dev_AuditLog_Viewer` | dev | T-031 | ✦ |
| T-036 | Create destination `S4HANA_PC` (HTTP / BasicAuthentication / Internet) on subaccount `invmgmt-dev` | dev | T-001 | ✦ |

Tasks that can be executed in parallel are marked accordingly.

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
