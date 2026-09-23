# Detail the security setup

Security configuration is often the most error-prone part of a BTP setup — IdP trust misconfiguration, missing role assignments, and destinations with wrong authentication types are common causes of application failures that are difficult to diagnose. In this exercise, the agent defines the complete security configuration for the inventory management environment.

The governance rules make this exercise particularly interesting: the default SAP ID Service is **not permitted** as the application IdP. The agent knows this from `memory/governance.md` and will enforce it. SAP Cloud Identity Services must be configured as both the platform IdP and the application IdP.

## What the security configuration covers for our scenario

### IdP Trust Configuration

| Trust type | Provider | Why |
|---|---|---|
| Platform IdP | SAP Cloud Identity Services (IAS) | Administrators logging into the BTP cockpit use corporate credentials |
| Application IdP | SAP Cloud Identity Services (IAS) | End users accessing the inventory Fiori app use corporate credentials |

> [!NOTE]
> Configuring SAP Cloud Identity Services as the **platform IdP** is a global account level setting. The subaccount-level trust configuration sets up IAS as the **application IdP** for the `invmgmt-dev` subaccount. Both are captured in the spec.

## 🛠️ Execute the security command

Execute the following command in the Claude Code terminal:

```bash
/btp-iac.security
```

The skill reads `specs/scenario.md`, `specs/landscape.md`, and `memory/governance.md`.

The skill will perform a governance check: since `Custom IdP: required` is set in the governance file, it will verify that an IAS trust configuration is included. If you had not mentioned SAP Cloud Identity Services in the scenario, the skill would stop here with an error — it cannot produce a compliant security spec without the required IdP trust.

It might raise some follow-up questions to clarify the trust configuration or role collections. Add the following information:

URL of SAP Cloud Identity tenant: SAP Cloud Identity Services tenant
Role Collections: `InvMgmt_Dev_Admin`, `InvMgmt_Dev_Developer`, `InvMgmt_Dev_Viewer`, `InvMgmt_Dev_AuditLog_Viewer`
User/group assignment: Group assignment should be done at the IAS group level following the pattern

| IAS Group | BTP Role Collection | Notes |
|---|---|---|
| `invmgmt-dev-admins` | `InvMgmt_Dev_Admin` | Assigned to senior devs / infra owners managing the dev environment |
| `invmgmt-dev-developers` | `InvMgmt_Dev_Developer` | Assigned to all active developers on the project |
| `invmgmt-dev-viewers` | `InvMgmt_Dev_Viewer` | Assigned to stakeholders or testers with read-only needs |
| `invmgmt-dev-audit-viewers` | `InvMgmt_Dev_AuditLog_Viewer` | Assigned to compliance/audit team members |


Role collections are assigned to user groups from SAP Cloud Identity Services (not individual users), so onboarding new team members only requires adding them to the correct IAS group.

Review the proposed security configuration and role collection assignments. Confirm by selecting `Yes`. This will create a new file in `specs/` as `trust.md`.

### 🔬 Look behind the curtains

Open the `btp-iac.security.md` skill file. Notice:

1. **Governance enforcement for IdP:** The skill checks whether `Custom IdP: required` is in the governance file. If it is, and no IAS trust is being configured, it stops. This is a hard block, not a warning.
2. **Role collection scope:** The governance file specifies role collection scope as `application` (not `platform`). The skill will ensure `InventoryViewer`, `InventoryManager`, and `InventoryAdmin` are application-level role collections assigned via IAS groups.

The output `specs/trust.md` is the final piece of the specification puzzle — after this step, the agent has everything it needs to generate Terraform code.

### ✅ Result

After the command completes:

- Open `specs/trust.md` — you should see:
  - IAS trust configuration for both platform and application IdP
  - Three role collections: `InventoryViewer`, `InventoryManager`, `InventoryAdmin`
  - IAS user group assignments for each role collection
- Verify that **no reference to the default SAP ID Service** appears in the trust configuration — the governance rule was enforced.
- Cross-check what has happened in the `memory/` folder.

**Key insight:** Without spec-driven development, this security configuration would be set up manually in the BTP cockpit — undocumented, unrepeatable, and impossible to review in a pull request. Here it is a structured document that can be version-controlled, reviewed, and reproduced exactly.

## 📋 Summary

The security configuration is now fully specified.  In the next exercise, we finish the specification tasks by detailing the connectivity setup.

Continue to - [Exercise 7 - Detail the connectivity setup](../ex7/README.md)
