# Detail the security setup

Security configuration is often a critical part of a setup comprising IdP trust configuration and role assignments. In this exercise, the agent defines the complete security configuration for the inventory management environment.
The previously defined governance rules will again play an important role as guardrail since we defined that the default SAP ID Service is **not permitted**.

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can use as a **cross reference**. Do not copy it as it might not be perfectly fitting to the previous specifications that have been created by you.

## 🎯 Intent of the exercise

In this exercise the goal is to derive the main settings of the Identity Provider (IdP) trust configuration and role assignments for the inventory management environment. The information can be partially derived from the already captured data, but the interesting part is to determine and close the gaps we left until now.

## 🛠️ Execute the security command

Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.security
```

This triggers the agent to first read the specification documents especially `specs/scenario.md`, `specs/landscape.md`, and `memory/governance.md`.

The agent will then perform a governance check as `Custom IdP: required` is set in the governance file. It will verify that an IAS trust configuration is included.

As we did not provide the full trust configuration details in the scenario, the agent might ask follow-up questions to clarify the setup. Add the following information when asked for the URL of SAP Cloud Identity tenant:

```text
ainfvn15r.accounts.ondemand.com
```

Claude will then ask you to provide an explicit origin for this IdP trust configuration or if this field should be left empty. Provide the following information when asked:

```text
Do not add an explicit origin, leave the field empty.
```

We did not explicitly mention which role collections should be created; therefore, the agent will ask which role collections are required.
Enter the following information when asked about role collections:

```text
Define the role collections:
InvMgmt_Dev_Admin
InvMgmt_Dev_Developer
InvMgmt_Dev_Viewer
InvMgmt_Dev_AuditLog_Viewer
```

The next logical step is to add roles to a role collection. When asked, state that:

```text
Do not add roles to the role collections
```

As a follow-up question the agent will ask about the role collection assignment approach (user- vs. group-based). Provide the following information:

```text
I want to define a group assignment. This group assignment should be done at the IAS group level following the pattern

| IAS Group | Role Collection | Notes |
|---|---|---|
| invmgmt-dev-admins | InvMgmt_Dev_Admin | Assigned to senior devs / infra owners managing the dev environment |
| invmgmt-dev-developers | InvMgmt_Dev_Developer | Assigned to all active developers on the project |
| invmgmt-dev-viewers | InvMgmt_Dev_Viewer | Assigned to stakeholders or testers with read-only needs |
| invmgmt-dev-audit-viewers | InvMgmt_Dev_AuditLog_Viewer | Assigned to compliance/audit team members |
```

As we have defined a Cloud Foundry environment including a space, the agent also needs to know how the roles should be assigned within that space. Add the following information and **replace the `0XX`** with your group number:

```text
Space: dev
User "xp265-0XX@education.cloud.sap" needs the following roles assigned:
- space_developer
- space_manager
```

With this the agent has gathered all required information. It will present an overview about the specification document and ask for confirmation. Confirm the proposal by selecting `Yes`.

The agent will create a new file `specs/trust.md`.

### 🔬 Look behind the curtains

The technical foundation of this step is the skill `.claude/commands/sap-iac.security.md`. Open the file and notice how the skill:

1. Collects the missing IdP information and derives the origin values required later
2. Gathers role collection requirements
3. Fetches the space-level role assignments in case that a Cloud Foundry environment is requested
4. Writes the result into `specs/trust.md`

### ✅ Result

- The security-specific requirements are collected in the `specs/trust.md` document.
- Gaps from the previous steps that needed to be detailed are now captured.

**Key insight:** The guided flow of the spec-driven development approach helps is to close gaps due to missing information in our intent description especially in critical areas like the security setup.

## 📋 Summary

The security configuration is now fully specified. In the next exercise, we finish the specification tasks by detailing the connectivity setup.

Continue to - [Exercise 7 - Detail the connectivity setup](../ex7/README.md)
