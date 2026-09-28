# Evaluate the design

The task list tells us *what* to build and in what order. The design step decides *where* each piece of Terraform code should be put when the code is finally generated. This depends on our choice how to structure the code in general.

## 🎯 Intent of the exercise

In this exercise, the agent evaluates the design of the Terraform code structure based on the task list and the users descision on the Terraform code layout.

## 🛠️ Execute the design command

Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.design
```

Based on the task list, the agent will ask you about the desired Terraform file layout with the two options:

```powershell
- Per-stage directories — terraform/dev/btp/ and terraform/dev/cf/ (one directory per stage)
- Single configuration — terraform/btp/ and terraform/cf/ with a dev.tfvars file per stage
```

Chose the `Single Configuration`.

The proposed layout shoudl look like this:

```powershell
terraform/
    btp/   { main.tf  variables.tf  outputs.tf  providers.tf  backend.tf  dev.tfvars  terraform.tfvars.example }
    cf/    { main.tf  variables.tf  outputs.tf  providers.tf  backend.tf  dev.tfvars  terraform.tfvars.example }
```

Next the agent will ask to annotate the tasks in accordance to the proposed layout i.e. where to put each piece of Terraform code. We confirm that and the agent will update the task list accordingly.

### 🔬 Look behind the curtains

The technical foundation of this step is the skill `.claude/commands/sap-iac.design.md`. Open the file and notice how the skill:

1. Introduces the different design and layout options for the Terraform code based on a standard layout
2. Annotates the tasks in `specs/tasks.md` with the corresponding Terraform file paths based on the chosen layout

**Key insight:** The final ingredient for the code generation is the annotated task list, which tells the agent exactly where to place each piece of Terraform code.

### ✅ Result

An enahnced task list is available in `specs/tasks.md` with additional file path annotations indicating where each piece of Terraform code should be placed.

## 📋 Summary

The design is finalized. Every task in the task list has a target file path. In the next exercise we will instrauct the agent to generate the Terraform code.

Continue to - [Exercise 10 - Generate the code](../ex10/README.md)
