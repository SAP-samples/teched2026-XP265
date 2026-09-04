# Evaluate the design

The task list tells us *what* to build and in what order. The design step decides *where* each piece of Terraform code lives. The `/btp-iac.design` skill translates the task list into a concrete Terraform folder structure — determining how resources are split across files, and annotating each task with the file path where its Terraform HCL will be written.

This step is important because it separates the structural decision from code generation. You can review the proposed file layout, ask the agent to reorganize it, and only then proceed to generation.

## The expected Terraform file layout for our scenario

Based on the task list, the agent will propose a file structure similar to this:

```
terraform/
├── main.tf              # Provider configuration and global account data source
├── variables.tf         # Parameterizable values (global account subdomain, cost centre, etc.)
├── outputs.tf           # Output values (subaccount ID, CF org GUID, etc.)
├── subaccount.tf        # T01: subaccount resource + CF environment + CF space + cost centre tag
├── entitlements.tf      # T04: all service entitlements in one place
├── subscriptions.tf     # T05–T07: SaaS subscriptions (IAS, Build Work Zone, Build Code)
├── services.tf          # T09–T14: service instances (XSUAA, HANA Cloud, HDI, Audit Log, Cloud Logging, Destination)
├── trust.tf             # T08: IAS trust configuration (platform + application IdP)
├── destinations.tf      # T15: destination resources (S4HANA-MaterialMaster)
└── roles.tf             # T16–T19: role collections and IAS group assignments
```

This layout groups resources by their logical concern rather than by the order they were defined. Entitlements together, subscriptions together, services together, security together. A developer unfamiliar with the project can navigate to the right file immediately.

> [!NOTE]
> The agent may propose a slightly different layout depending on its current reasoning. If you would prefer a different file structure (e.g., a single large `main.tf`, or per-service files), you can ask the agent to reorganize before proceeding to code generation.

## 🛠️ Execute the design command

Execute the following command in the Claude Code terminal:

```bash
/btp-iac.design
```

The skill reads `specs/tasks.md` and annotates each task with a `file:` field indicating which Terraform file it belongs to.

### 🔬 Look behind the curtains

Open the `btp-iac.design.md` skill file. Notice how the skill:

1. Groups tasks by logical concern — subaccount, entitlements, subscriptions, service instances, security
2. Decides whether to introduce Terraform modules (for this scenario: no, the flat file structure is sufficient for a single subaccount)
3. Determines how environment-specific values will be handled (via `variables.tf` rather than hardcoding)
4. Annotates each task ID in `specs/tasks.md` with a `file:` path — this annotation is what the generate skill reads

After this step, `specs/tasks.md` contains not just the *what* and *order* but also the *where* for every resource. The generate skill will use these annotations as its sole guide for where to write each piece of code.

### ✅ Result

After the command completes:

- Open `specs/tasks.md` — each task should now have a `file:` annotation, e.g.:
  - `T01 → file: terraform/subaccount.tf`
  - `T04 → file: terraform/entitlements.tf`
  - `T08 → file: terraform/trust.tf`
  - `T10 → file: terraform/services.tf`
  - `T16–T19 → file: terraform/roles.tf`
- Verify that the `terraform/` folder is **still empty** — no code has been written yet. The design is a plan, not an implementation.
- Cross-check what has happened in the `memory/` folder.

**Key insight:** This step demonstrates a core principle of spec-driven development — decisions are separated from implementation. By reviewing the file layout before code generation, you catch structural problems early. Moving a resource from `services.tf` to a separate `hana.tf` is a one-line edit in the spec; reorganizing generated Terraform code is much more work.

## 📋 Summary

The design is finalized. Every task in the task list now has a target file path. The agent is ready to generate the Terraform code — and it will run a full governance validation pass before writing a single file.

Continue to - [Exercise 9 - Generate the code](../ex9/README.md)
