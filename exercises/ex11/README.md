# Deploy the generated code

The Terraform code is generated, formatted, and validated. The final step is to deploy it to your account — provisioning the `invmgmt-dev` subaccount with all the services the inventory management team needs to start building.

## 🛠️ Deploy to SAP Business AI Platform

### Step 1 - Rename the terraform.tfvars example files

The code generation created `terraform.tfvars` files but added `.example` to their names to ensure that they are treated as examples.Rename them by removing the `.example` suffix so that Terraform can pick them up automatically.

### Step 2 - Add your credentials into the terraform.tfvars files

> [!WARNING]
> Critical: We put our credentials into the `terraform.tfvars` files. This is not a good practice as it exposes sensitive information and should therefore not be done in real-world scenarios. For the sake of this exercise, we will proceed this way, but always consider more secure alternatives in production environments.

- Open the file `terraform.tfvars` in the folder `SAPTechEd2026-000/terraform/btp/` and fill in your credentials. Set the value for the custom IdP to `ainfvn15r`.
- Open the file `terraform.tfvars` in the folder `SAPTechEd2026-000/terraform/cf/` and fill in your credentials. Set the value for the custom origin to `ainfvn15r-platform`.

### Step 3 - Remove the service instance for SAP HANA Cloud

As the provisioning of HANA Cloud can take a while, we remove the service instance from the Terraform configuration before applying the deployment. To do so open the file `SAPTechEd2026-000/terraform/btp/main.tf` and comment out or remove the HANA Cloud service instance block. The resulting block should look like this:

```terraform
#resource "btp_subaccount_service_instance" "hana_cloud_hana" {
#  subaccount_id         = btp_subaccount.dev.id
#  service_offering_name = "hana-cloud"
#  serviceplan_name      = "hana"
#  name                  = "hana-cloud-hana"
#  parameters = jsonencode({
#    data = {
#      memory                 = 32
#      edition                = "cloud"
#      generateSystemPassword = true
#    }
#  })
#  depends_on = [btp_subaccount_entitlement.hana_cloud_hana]
#}
```

### Step 4 - Plan the deployment

Open a terminal in the root folder of the generated Terraform code. Review what Terraform intends to create before applying:

```bash
terraform plan -out=plan.out
```

Scan the plan output and validate that it aligns with your expectations before proceeding to apply the deployment.

### Step 5 - Apply the deployment

Execute the following command in the terminal:

```bash
terraform apply plan.out
```

Terraform will provision resources in the order defined by the dependency graph in the generated code.

The Terraform apply command will also output the Cloud Foundry API URL, which you need to log in to the Cloud Foundry environment.


### Step 6 - Plan the deployment for Cloud Foundry

As we have the Cloud Foundry environment enabled, we can now plan the deployment for it specifically.

In the terminal, navigate to the root folder of the generated Terraform code for Cloud Foundry at `terraform/cf/`

Add the value for the Cloud Foundry API URL and the Cloud Foundry Org ID in the appropriate variable in the `terraform.tfvars` file before planning the deployment.

> [!TIP]
> If you cleaned or closed the terminal, you can access the Cloud Foundry API URL via the command `terraform output` in the folder of the Terraform configuration `terraform/btp/`.

Execute the following command in the terminal:

```bash
terraform plan -out=cf-plan.out
```

Scan the plan output and validate that it aligns with your expectations before proceeding to apply the deployment.

### Step 7 - Apply the deployment for Cloud Foundry

Execute the following command in the terminal:

```bash
terraform apply cf-plan.out
```

Terraform will provision resources in the order defined by the dependency graph in the generated code.


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
