# Generate the code

As a last step in our spec-driven development journey we will generate the Terraform code based on the specifications that we have defined in the previous steps.

## 🛠️ Execute the generate command

Execute the following command in the claude terminal:

```bash
/btp-iac.generate
```

### 🔬 Look behind the curtains

The technical foundation of the step is the `btp-iac.generate.md` skill located in the `.claude/commands` folder. The skill contains the instructions for the agentic tooling to generate the Terraform code.

### ✅ Result

- Result + Check the created markdown file in the `specs` folder. Cross check what has happened in the `memory` folder.

## 📋 Summary

Now that you have the generated the Terraform code, there is one last step you certainly want to do: deploy the Terraform code to the SAP BTP and bring the specification to life.

Continue to - [Exercise 10 - Deploy the generated code](../ex10/README.md)
