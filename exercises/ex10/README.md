# Deploy the generated code

As a last step, we will deploy the generated code to the SAP BTP environment.

## 🛠️ Deploy to SAP BTP

### Step 1 - Initialize the Terraform working directory

Execute the following command in the terminal:

```bash
terraform init
```

### Step 2 - Format and Validate the Terraform code

Execute the following commands in the terminal:

```bash
terraform fmt
terraform validate
```

### Step 3 - Add your credentials

Add your SAP BTP credentials as environment variables in the terminal:

```bash
export BTP_USER=<your-username>
export BTP_PASSWORD=<your-password>
```

### Step 3 - Plan the deployment

Execute the following command in the terminal:

```bash
terraform plan -out=plan.out
```

### Step 4 - Apply the deployment

Execute the following command in the terminal:

```bash
terraform apply plan.out
```

### ✅ Result

After the deployment succeeded you should have a deployed SAP BTP environment based on the generated Terraform code.

## 📋 Summary

🎉 Congratulations - you have done your first spec-driven development flow for Terraform on SAP BTP!
