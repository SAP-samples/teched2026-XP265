# Generate the Terraform code

This is the step that the entire spec-driven flow has been building toward. The `/sap-iac.generate` skill reads the annotated task list and generates complete, validated Terraform configuration code.

## 🎯 Intent of the exercise

In this step the agent will synthesize the captured information from all previous exercises and specifically the annotated task list to generate a complete Terraform configuration that is ready for deployment.

## 🛠️ Execute the generate command

Before we start the code generation, we first clean the context of Claude as we accumulated a lot of information during the previous exercises. Execute the following command in the Claude Code terminal:

```powershell
/clear
```

Now we trigger the Terraform code generation. Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.generate
```

The agent reads `specs/tasks.md` including the file path annotations.

If the agent asks you which stages to generate make sure that you generate for the `dev` stage only.

> [!NOTE]
> If the agent tries to do access the Terraform registry via webfetches or curl requests, make sure that the MCP server is running and accessible (see [Getting Started - MCP Server for Terraform](../ex0/README.md#mcp-server-for-terraform) and [Getting Started - Wire up Claude Code to the MCP Server](../ex0/README.md#wire-up-claude-code-to-the-mcp-server)).

Before generating the Terraform code the information about how you want to authenticate is missing. The agent will ask you how you want to authenticate

- For the Terraform Provider for *SAP BTP*, choose:
      ```text
      username/password with custom IdP
      ```
- For the Terraform Provider for *Cloud Foundry*, choose:
      ```text
      username/password with custom origin
      ```

These values will be used in the provider configuration within the generated Terraform files as well as in the definition of variables.

The agent will ask you to allow the connection to the Terraform registry via the MCP server. Do so as this enables the agent to fetch the latest provider schemas and ensure that the generated Terraform code is compliant with the current provider specifications.

After collecting all required information the agent will ask for confirmation to create the Terraform file directories. Confirm by selecting `Yes, and always allow access to...`.

When the agent created the files you will get asked if the agent is allowed to do edits. Confirm by selecting `Yes, and switch to accept edits ...`.

> [!NOTE]
> The task list is used to guide the code generation, consequently the agent using the skill will also tick off tasks as they are processed.

After the generation of the files is complete, the agent wants to run `terraform init`. Confirm the execution if asked.

After that, the agent will also run to end up with a formatted and validated Terraform configuration:

```powershell
terraform fmt
terraform validate
```

After successfully finishing the code generation the agent will provide additional information how to proceed with the generated code.

### 🔬 Look behind the curtains

The technical foundation of this step is the skill `.claude/commands/sap-iac.generate.md`. Open the file and notice how the skill:

1. Executes a final round of checks against the governance constraints
2. Injects platform specific prerequisites like subdomain uniqueness and determination of Cloud Foundry landscape label values.
3. Executes the provider schema lookup from the registry
4. Writes the Terraform files in the correct folders depending in your project structure respecting the dependencies between resources but also between the main setup of the account and the environment specifics for Cloud Foundry.

**Key insight:** Compared with the `terraform/` output from a "vibe-coded" approach, the spec-driven result is governance-compliant, dependency-ordered, consistently structured, validated, and reproducible.

### ✅ Result

- The Terraform code is generated, formatted, and validated.
- The structure reflects your specification including the required dependency order.


## 📋 Summary

The Terraform code is generated, formatted, and validated. In the next exercise we will deploy the generated code to the global account.

Continue to - [Exercise 11 - Deploy the generated code](../ex11/README.md)
