# Evaluate the design

The task list tells us *what* to build and in what order. The design step decides *where* each piece of Terraform code lives. The `/sap-iac.design` skill translates the task list into a concrete Terraform folder structure — determining how resources are split across files, and annotating each task with the file path where its Terraform HCL will be written.

This step is important because it separates the structural decision from code generation. You can review the proposed file layout, ask the agent to reorganize it, and only then proceed to generation.

## The expected Terraform file layout for our scenario

Based on the task list, the agent will ask you about the desired Terraform file layout with the two options:

```bash
- Per-stage directories — terraform/dev/btp/ and terraform/dev/cf/ (one directory per stage)
- Single configuration — terraform/btp/ and terraform/cf/ with a dev.tfvars file per stage
```

Chose the `Single Configuration`.

The proposed layout shoudl look like this:

```bash
terraform/
    btp/   { main.tf  variables.tf  outputs.tf  providers.tf  backend.tf  dev.tfvars  terraform.tfvars.example }
    cf/    { main.tf  variables.tf  outputs.tf  providers.tf  backend.tf  dev.tfvars  terraform.tfvars.example }
```


Next the agent will ask to annotate the tasks in accordance to the proposed layout i.e. where to put each piece of Terraform code. We confirm that.

## 🛠️ Execute the design command

Execute the following command in the Claude Code terminal:

```bash
/sap-iac.design
```

Based on the task list, the agent will ask you about the desired Terraform file layout with the two options:

```bash
- Per-stage directories — terraform/dev/btp/ and terraform/dev/cf/ (one directory per stage)
- Single configuration — terraform/btp/ and terraform/cf/ with a dev.tfvars file per stage
```

Chose the `Single Configuration`.

The proposed layout shoudl look like this:

```bash
terraform/
    btp/   { main.tf  variables.tf  outputs.tf  providers.tf  backend.tf  dev.tfvars  terraform.tfvars.example }
    cf/    { main.tf  variables.tf  outputs.tf  providers.tf  backend.tf  dev.tfvars  terraform.tfvars.example }
```


Next the agent will ask to annotate the tasks in accordance to the proposed layout i.e. where to put each piece of Terraform code. We confirm that and the agent will update the task list accordingly.

### 🔬 Look behind the curtains

Open the `sap-iac.design.md` skill file. Notice how the skill:

1. Groups tasks by logical concern — subaccount, entitlements, subscriptions, service instances, security
2. Decides whether to introduce Terraform modules (for this scenario: no, the flat file structure is sufficient for a single subaccount)
3. Determines how environment-specific values will be handled (via `variables.tf` rather than hardcoding)
4. Annotates each task ID in `specs/tasks.md` with a `file:` path — this annotation is what the generate skill reads

After this step, `specs/tasks.md` contains not just the *what* and *order* but also the *where* for every resource. The generate skill will use these annotations as its sole guide for where to write each piece of code.

### ✅ Result

After the command completes:

- Open `specs/tasks.md` — each task should now have a `file:` annotation, e.g.:

```markdown
- [ ] T-001 `[dev]` Create subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount`, `subaccount = invmgmt-dev`, `subdomain = invmgmt-dev`, `region = eu20`, `usage = NOT_USED_FOR_PRODUCTION`, `beta_enabled = true`, `labels = { cost-center = CC-123456 }`
  - File: `terraform/btp/main.tf`
```

- Verify that the `terraform/` folder is **still empty** — no code has been written yet. The design is a plan, not an implementation.
- Cross-check what has happened in the `memory/` folder.

**Key insight:** This step demonstrates a core principle of spec-driven development — decisions are separated from implementation. By reviewing the file layout before code generation, you catch structural problems early. Moving a resource from `services.tf` to a separate `hana.tf` is a one-line edit in the spec; reorganizing generated Terraform code is much more work.

## 📋 Summary

The design is finalized. Every task in the task list now has a target file path. The agent is ready to generate the Terraform code — and it will run a full governance validation pass before writing a single file.

Continue to - [Exercise 10 - Generate the code](../ex10/README.md)
