# Evaluate the design

The task list tells us *what* to build and in what order. The design step decides *where* each piece of Terraform code should be put when the code is finally generated. This depends on our choice how to structure the Terraform code.

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can use as a **cross reference**. Do not copy it as it might not be perfectly fitting to the previous specifications that have been created by you.

## 🎯 Intent of the exercise

In this exercise, the agent evaluates the Terraform code structure based on the task list and the user's decision about the Terraform code layout.

## 🛠️ Execute the design command

Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.design
```

Based on the task list, the agent will ask you about the desired Terraform file layout with the two options:

- Per-stage directories — terraform/dev/btp/ and terraform/dev/cf/ (one directory per stage)
- Single configuration — terraform/btp/ and terraform/cf/ with a dev.tfvars file per stage

Choose:

```text
Single Configuration
```

The proposed layout should look similar to this (visual representation might vary):

```powershell
terraform/
    btp/   { main.tf  variables.tf  outputs.tf  providers.tf  backend.tf  dev.tfvars  terraform.tfvars.example }
    cf/    { main.tf  variables.tf  outputs.tf  providers.tf  backend.tf  dev.tfvars  terraform.tfvars.example }
```

> [!TIP]
> You will recognize that the setup correctly derives a tfvars file per stage namely a `dev.tfvars` in our case. The setup also adds an example tfvars file called `terraform.tfvars.example` which enables a quick local execution without bothering about the location of the variable values.

Next the agent will ask to annotate the tasks in accordance to the proposed layout i.e. where to put each piece of Terraform code. Confirm the proposal.

The agent will update the task list with the corresponding Terraform file paths.

### 🔬 Look behind the curtains

The technical foundation of this step is the skill `.claude/commands/sap-iac.design.md`. Open the file and notice how the skill:

1. Introduces the different design and layout options for the Terraform code based on standard layouts
2. Annotates the tasks in `specs/tasks.md` with the corresponding Terraform file paths based on the chosen layout

**Key insight:** The final ingredient required for the code generation is the annotated task list. The agent now has all the information it needs to generate the Terraform code.

### ✅ Result

An enhanced task list is available in `specs/tasks.md` with additional file path annotations indicating where each piece of Terraform code should be placed.

## 📋 Summary

The task list is now fully annotated with the target file paths. In the next exercise, we will instruct the agent to generate the Terraform code.

Continue to - [Exercise 10 - Generate the code](../ex10/README.md)
