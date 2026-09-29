# Deploy the generated Terraform code

The Terraform code is generated, formatted, and validated. The final step is to deploy it to your account provisioning the `invmgmt-dev` subaccount with all the services the inventory management team needs to start building.

## 🛠️ Deploy to SAP Business AI Platform

Before proceeding, open a new terminal window in the root folder of the generated Terraform code `SAPTechEd2026-0XX/terraform/`.

### Step 1 - Rename the terraform.tfvars example files

The code generation created `terraform.tfvars` files in the `terraform/btp/` and `terraform/cf/` folders and added `.example` to their names to ensure that they are treated as examples. Rename them by removing the `.example` suffix so that Terraform can pick them up automatically.

### Step 2 - Add your credentials into the terraform.tfvars files

> [!WARNING]
> Critical: We put our credentials into the `terraform.tfvars` files. This is not a good practice as it exposes sensitive information and should therefore not be done in real-world scenarios. For the sake of this exercise, we will proceed this way, but always consider more secure alternatives in production environments.

#### Terraform provider for SAP BTP

- Open the file `terraform.tfvars` in the folder `SAPTechEd2026-0XX/terraform/btp/` and fill in your credentials. Set the value for the custom IdP to `ainfvn15r`.
- If the service names contain placeholders replace them with your group name.
- Set the password for the S/4HANA destination to any value,e.g. `test1234`

#### Terraform provider for Cloud Foundry

- Open the file `terraform.tfvars` in the folder `SAPTechEd2026-0XX/terraform/cf/` and fill in your credentials. Set the value for the custom origin to `ainfvn15r-platform`.
- If the service names contain placeholders replace them with your group name.

The file contains placeholders for the Cloud Foundry API as well as the Cloud Foundry Org ID. They cannot be filled yet as the Cloud Foundry environment is not yet provisioned. We will add them later once the environment is available.

### Step 3 - Remove the service instance for SAP HANA Cloud

As the provisioning of HANA Cloud can take a while, we remove the service instance from the Terraform configuration before applying the deployment.

Open the file `SAPTechEd2026-0XX/terraform/btp/main.tf` and comment out the HANA Cloud service instance block. The resulting block should look like this:

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

Change to the directory `terraform/btp/`. Before applying review the plan of the Terraform deployment by executing a `terraform plan` command in the terminal:

```powershell
terraform plan -out=plan.out
```

Scan the plan output and validate that it aligns with your expectations before proceeding to apply the deployment.

### Step 5 - Apply the deployment

Deploy the Terraform configuration to the global account. Execute the following command:

```powershell
terraform apply plan.out
```

Terraform will provision resources in the order defined by the dependency graph in the generated code.

When finished, the Terraform apply command will output the *Cloud Foundry API URL* as well as the *Cloud Foundry Org ID*. You need these values as input for the Cloud Foundry deployment.

### Step 6 - Plan the deployment for Cloud Foundry

As we have the Cloud Foundry environment enabled, we plan the deployment for it. Change the root folder of the generated Terraform code for Cloud Foundry `SAPTechEd2026-0XX/terraform/cf/`

Add the values for the *Cloud Foundry API URL* and the *Cloud Foundry Org ID* in the appropriate variable in the `cf/terraform.tfvars` file before planning the deployment.

> [!TIP]
> If you cleaned or closed the terminal, you can access the Cloud Foundry API URL via the command `terraform output` in the folder of the Terraform configuration `terraform/btp/`.

As before we review the plan of the Cloud Foundry deployment. Execute the following command in the terminal:

```powershell
terraform plan -out=cf-plan.out
```

Scan the plan output and validate that it aligns with your expectations before proceeding to apply the deployment.

### Step 7 - Apply the deployment for Cloud Foundry

Deploy the Terraform configuration to Cloud Foundry. Execute the following command:

```powershell
terraform apply cf-plan.out
```

Terraform will provision resources in the order defined by the dependency graph in the generated code.

### ✅ Result

After the deployment succeeds:

- Log in to the [Cockpit](https://emea.cockpit.btp.cloud.sap/cockpit/?idp=ainfvn15r.accounts.ondemand.com#/) and verify that:
  - The `invmgmt-dev` subaccount exists in the `eu20` region
  - The Cloud Foundry environment is enabled with a `dev` space
  - All services are created and the subscriptions are active
  - The IAS trust configuration is visible under *Security → Trust Configuration*
  - The three role collections are listed under *Security → Role Collections*
  - The Cloud Foundry space `dev` contains the destination service instance


## 📋 Summary

🎉 Congratulations — you have completed the full spec-driven development flow for IaC!

You encoded governance rules and started with a plain-language description of an inventory management application that you refined through the spec-driven process. Finally you generated deployment-ready Terraform code that follows the specifications.

The specification in `specs/` documents every decision that you made and serves as foundation for future development and maintenance of your infrastructure, ensuring that your team can continue to build and evolve the system with confidence.
