# Generate the code

This is the step that the entire spec-driven flow has been building toward. The `/sap-iac.generate` skill reads the annotated task list and generates complete, validated Terraform HCL — writing each resource to the file path defined in the design step.

But before writing a single file, the skill runs a **full governance validation pass** across all five categories. This is the final safety net — even if a decision was missed or changed since Exercise 2, the governance rules still catch it here.

## 🎯 Intent of the exercise

Before any Terraform file is written, the agent validates:

| Category | What is checked |
|---|---|
| **Regions** | Every `btp_subaccount` resource uses `cf-eu10`. Any other region → STOP. |
| **Naming** | Every subaccount name matches `invmgmt-<env>`. `invmgmt-dev` ✓ |
| **Service plans** | HANA Cloud uses `hana` plan ✓, Audit Log uses `free` ✓, Cloud Logging uses `free` ✓. Any `premium` plan → STOP. |
| **Security** | IAS trust is configured ✓. No reference to the default SAP ID Service ✓. Role collections present ✓. |
| **Cost controls** | HANA Cloud triggers the metered-service warning (does not block). The `cost-centre` tag is present on the subaccount resource → validated. Missing tag → STOP. |

Only after all five checks pass does the agent begin writing files.

## 🛠️ Execute the generate command

Execute the following command in the Claude Code terminal:

```bash
/sap-iac.generate
```

The skill reads `specs/tasks.md` (with file path annotations) and `memory/governance.md`.

If the skill asks you which stages to generate make sure that you generate for the `dev` stage only.

The skill will ask you how you want to authenticate

- For the Terraform Provider for SAP BTP chose: `Username / password + custom IdP`.
- For the Terraform Provider for Cloud Foundry chose: `Username / password + custom origin`.

The skill will ask you to confirm the connection to the Terraform registry via the MCP server. Do so as this enables the skill to fetch the latest provider schemas and ensure that the generated Terraform code is compliant with the current provider specifications.

After collecting all required information the skill will ask for confirmation to create the Terraform file directories. Confirm by selecting `Yes, and always allow access to...`.

When the files get created you will get asked if the agent is allowed to do edits. Confirm by selecting ` Yes, and switch to accept edits ...`.

After the files are generated the skill wants to run `terraform init`. Confirm the execution if asked.


After generating all files, the skill will run:

```bash
terraform fmt
terraform validate
```

Watch the output — you will see each task being processed, the governance validation results, and finally the `terraform validate` result.

After finishing the skill will provide additional information how to proceed.

### 🔬 Look behind the curtains

Open the `sap-iac.generate.md` skill file. Notice:

1. The **pre-generation governance pass** happens before the first `Write` call — this is not a post-hoc check, it prevents non-compliant code from being written at all.
2. The skill iterates through tasks **in the order defined in `specs/tasks.md`** — the dependency ordering from Exercise 7 is preserved in the generated code.
3. The **Terraform MCP server** is used to look up the exact resource schema for `btp_subaccount`, `btp_subaccount_entitlement`, `btp_subaccount_service_instance`, etc. — ensuring the generated HCL uses valid argument names and current provider version syntax.
4. After all files are written, `terraform fmt` normalizes spacing and indentation, and `terraform validate` checks the schema and references.

### ✅ Result

After the command completes:

- Open the `terraform/` folder — you should see all the files defined in the design step: `main.tf`, `variables.tf`, `outputs.tf`, `subaccount.tf`, `entitlements.tf`, `subscriptions.tf`, `services.tf`, `trust.tf`, `destinations.tf`, `roles.tf`.
- Open `terraform/subaccount.tf` — verify that the subaccount resource uses region `cf-eu10` and includes the `cost-centre` tag.
- Open `terraform/services.tf` — verify that the HANA Cloud instance uses the `hana` plan and that the HDI container instance depends on the HANA Cloud instance.
- Open `terraform/trust.tf` — verify that IAS is configured as the application IdP and that no reference to the default SAP ID Service exists.
- Open `terraform/roles.tf` — verify that all three role collections (`InventoryViewer`, `InventoryManager`, `InventoryAdmin`) are present with IAS group assignments.
- Confirm that `terraform validate` passed — there should be no schema errors.

**Key insight:** Compare the `terraform/` output to what you would get from a vibe-coded session — "hey ChatGPT, write me Terraform for HANA Cloud." The spec-driven result is: governance-compliant, dependency-ordered, consistently structured, and validated. The vibe-coded result is: possibly correct on a good day, definitely not reproducible next week.

## 📋 Summary

The Terraform code is generated, formatted, and validated. Every resource is compliant with the governance rules, correctly ordered by dependency, and ready to be deployed to SAP Business AI Platform.

Continue to - [Exercise 11 - Deploy the generated code](../ex11/README.md)
