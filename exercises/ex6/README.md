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

### Role Collections

The inventory application has three user personas, each mapped to a role collection:

| Role Collection | Purpose | Assigned Roles |
|---|---|---|
| `InventoryViewer` | Read-only access — warehouse staff viewing stock levels | Viewer roles from the CAP app, Build Work Zone site viewer |
| `InventoryManager` | Full CRUD on inventory records — procurement team | All viewer roles + create/update/delete on inventory entities |
| `InventoryAdmin` | Configuration and user management — IT admin | All manager roles + application administration |

Role collections are assigned to user groups from SAP Cloud Identity Services (not individual users), so onboarding new team members only requires adding them to the correct IAS group.

### Destination for S/4HANA Connectivity

The inventory app reads material master data from an on-premise SAP S/4HANA system. A destination is required:

| Field | Value |
|---|---|
| Destination name | `S4HANA-MaterialMaster` |
| Type | HTTP |
| Authentication | `OAuth2SAMLBearerAssertion` |
| Description | SAP S/4HANA on-premise system for material master data |

## 🛠️ Execute the security command

Execute the following command in the Claude Code terminal:

```bash
/btp-iac.security
```

The skill reads `specs/scenario.md`, `specs/landscape.md`, and `memory/governance.md`.

The skill will perform a governance check: since `Custom IdP: required` is set in the governance file, it will verify that an IAS trust configuration is included. If you had not mentioned SAP Cloud Identity Services in the scenario, the skill would stop here with an error — it cannot produce a compliant security spec without the required IdP trust.

### 🔬 Look behind the curtains

Open the `btp-iac.security.md` skill file. Notice:

1. **Governance enforcement for IdP:** The skill checks whether `Custom IdP: required` is in the governance file. If it is, and no IAS trust is being configured, it stops. This is a hard block, not a warning.
2. **Role collection scope:** The governance file specifies role collection scope as `application` (not `platform`). The skill will ensure `InventoryViewer`, `InventoryManager`, and `InventoryAdmin` are application-level role collections assigned via IAS groups.
3. **Destination validation:** If the governance file had restricted destinations to specific allowed types, the `OAuth2SAMLBearerAssertion` destination would be validated against that list.

The output `specs/trust.md` is the final piece of the specification puzzle — after this step, the agent has everything it needs to generate Terraform code.

### ✅ Result

After the command completes:

- Open `specs/trust.md` — you should see:
  - IAS trust configuration for both platform and application IdP
  - Three role collections: `InventoryViewer`, `InventoryManager`, `InventoryAdmin`
  - IAS user group assignments for each role collection
  - The `S4HANA-MaterialMaster` destination configuration
- Verify that **no reference to the default SAP ID Service** appears in the trust configuration — the governance rule was enforced.
- Cross-check what has happened in the `memory/` folder.

**Key insight:** Without spec-driven development, this security configuration would be set up manually in the BTP cockpit — undocumented, unrepeatable, and impossible to review in a pull request. Here it is a structured document that can be version-controlled, reviewed, and reproduced exactly.

## 📋 Summary

The security configuration is now fully specified. The three spec files (`specs/landscape.md`, `specs/services.md`, `specs/trust.md`) together form the complete picture of the `invmgmt-dev` environment. In the next exercise, the agent consolidates them into a single dependency-ordered task list.

Continue to - [Exercise 7 - Define the tasks](../ex7/README.md)
