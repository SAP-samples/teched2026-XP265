# Deploy the generated code

The Terraform code is generated, formatted, and validated. The final step is to deploy it to your account — provisioning the `invmgmt-dev` subaccount with all the services the inventory management team needs to start building.

## 🛠️ Deploy to SAP Business AI Platform

### Step 1 - Initialize the Terraform working directory

Navigate to the `terraform/` folder and initialize the working directory. This downloads the Terraform provider for SAP BTP and prepares the backend.

```bash
cd terraform
terraform init
```

You should see the Terraform provider for SAP BTP being downloaded. The version used will be the one resolved by the Terraform MCP server during code generation.

### Step 2 - Format and validate the Terraform code

The generate skill already ran these — but it is good practice to run them again after `terraform init` confirms the provider is in place:

```bash
terraform fmt
terraform validate
```

Both should succeed without errors.

### Step 3 - Add your credentials

Add your redentials as environment variables in the terminal.

**macOS / Linux:**

```bash
export BTP_USER=<your-username>
export BTP_PASSWORD=<your-password>
```

**Windows — PowerShell:**

```powershell
$env:BTP_USER = "<your-username>"
$env:BTP_PASSWORD = "<your-password>"
```

**Windows — Command Prompt:**

```cmd
set BTP_USER=<your-username>
set BTP_PASSWORD=<your-password>
```

> [!CAUTION]
> Never commit these values to Git. The `.gitignore` file created by `sap-iac init` already excludes files that commonly contain credentials, but always verify before committing.

### Step 4 - Plan the deployment

Review what Terraform intends to create before applying:

```bash
terraform plan -out=plan.out
```

Scan the plan output and verify:

- A `btp_subaccount` resource named `invmgmt-dev` in region `cf-eu10` will be created
- Entitlements for all 10 services are included
- The HANA Cloud instance and HDI container are present
- The IAS trust configuration is included
- The three role collections (`InventoryViewer`, `InventoryManager`, `InventoryAdmin`) will be created

### Step 5 - Apply the deployment

Execute the following command in the terminal:

```bash
terraform apply plan.out
```

Terraform will provision resources in the order defined by the dependency graph in the generated code — the same dependency order the spec-driven flow established back in Exercise 7.

> [!NOTE]
> Some resources take longer to provision than others. SAP HANA Cloud instances, SAP Build Work Zone subscriptions, and SAP Cloud Identity Services subscriptions can take several minutes. Terraform will wait for each resource to become available before proceeding to dependent resources.

### ✅ Result

After the deployment succeeds:

- Log in to the [Cockpit](https://emea.cockpit.btp.cloud.sap/cockpit/?idp=ainfvn15r.accounts.ondemand.com#/) and verify that:
  - The `invmgmt-dev` subaccount exists in the `cf-eu10` region
  - The Cloud Foundry environment is enabled with a `development` space
  - All 10 services are entitled and the subscriptions are active
  - The IAS trust configuration is visible under *Security → Trust Configuration*
  - The three role collections are listed under *Security → Role Collections*
- The inventory management development team can now access the subaccount, connect to HANA Cloud via the HDI container, and start building the CAP application.

## 📋 Summary

🎉 Congratulations — you have completed the full spec-driven development flow!

You started with a plain-language description of an inventory management application, encoded governance rules that were enforced automatically at every step, built up a living specification across five structured documents, and ended with validated, deployment-ready Terraform code.

The `invmgmt-dev` environment is now live. The specification in `specs/` documents exactly why every decision was made. When the team needs to add staging or production environments, the spec-driven flow means it is a matter of adding a new environment tier — not starting from scratch.
