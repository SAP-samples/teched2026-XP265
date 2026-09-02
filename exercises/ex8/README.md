# Evaluate the design

In this exercise, we will take a look at the design of the Terraform file layout that the agentic tooling will derive based on our specification.

## 🛠️ Execute the design command

Execute the following command in the claude terminal:

```bash
/btp-iac.design
```

### 🔬 Look behind the curtains

The technical foundation of the step is the `btp-iac.design.md` skill located in the `.claude/commands` folder. The skill contains the instructions for the agentic tooling to evaluate the design namely the Terrform file structure.

### ✅ Result

- Result + Check the created markdown file in the `specs` folder. Cross check what has happened in the `memory` folder.

## 📋 Summary

Now that you have evaluated the design, you can continue with the next step to generate the Terraform code.

Continue to - [Exercise 9 - Generate the code](../ex9/README.md)
