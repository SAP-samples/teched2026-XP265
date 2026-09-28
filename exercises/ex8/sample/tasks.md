| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-001 | Create subaccount "invmgmt-dev" | dev | — | — |
| T-002 | Create Cloud Foundry environment in subaccount "invmgmt-dev" | dev | T-001 | — |

- [ ] T-001 `[dev]` Create subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount`, `subaccount = invmgmt-dev`, `subdomain = invmgmt-dev`, `region = eu20`, `usage = NOT_USED_FOR_PRODUCTION`, `beta_enabled = true`
- [ ] T-002 `[dev]` Create Cloud Foundry environment in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_environment_instance`, `environment_type = cloudfoundry`, `subaccount = invmgmt-dev`, `name = invmgmt-cf-dev`

## Cloud Foundry Spaces

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-003 | Create Cloud Foundry space "dev" in org "invmgmt-cf-dev" | dev | T-002 | — |

- [ ] T-003 `[dev]` Create Cloud Foundry space "dev" in org "invmgmt-cf-dev"
  - Task metadata: `resource_type = cloudfoundry_space`, `name = dev`, `subaccount = invmgmt-dev`

## Security — IdP Trust

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-004 | Configure custom IdP trust in subaccount "invmgmt-dev" | dev | T-001 | — |

- [ ] T-004 `[dev]` Configure custom IdP trust in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_trust_configuration`, `subaccount = invmgmt-dev`, `idp_url = ainfvn15r.accounts.ondemand.com`, `origin = ainfvn15r-platform`

## Security — CF Space Roles

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-005 | Assign space_developer to xp265-000@education.cloud.sap in space "dev" | dev | T-003 | ✦ |
| T-006 | Assign space_manager to xp265-000@education.cloud.sap in space "dev" | dev | T-003 | ✦ |

- [ ] T-005 `[dev]` Assign space_developer to xp265-000@education.cloud.sap in space "dev"
  - Task metadata: `resource_type = cloudfoundry_space_role`, `space_name = dev`, `username = xp265-000@education.cloud.sap`, `role_type = space_developer`, `origin = ainfvn15r-platform`
- [ ] T-006 `[dev]` Assign space_manager to xp265-000@education.cloud.sap in space "dev"
  - Task metadata: `resource_type = cloudfoundry_space_role`, `space_name = dev`, `username = xp265-000@education.cloud.sap`, `role_type = space_manager`, `origin = ainfvn15r-platform`

## Services — Entitlements

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-007 | Assign entitlement cloudfoundry/build-code to subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-008 | Assign entitlement hana-cloud/hana to subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-009 | Assign entitlement hana-cloud-tools/tools to subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-010 | Assign entitlement auditlog/standard to subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-011 | Assign entitlement auditlog-viewer/free to subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-012 | Assign entitlement cloud-logging/standard to subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-013 | Assign entitlement build-workzone-standard/standard to subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-014 | Assign entitlement build-code/free to subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-015 | Assign entitlement destination/lite to subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-016 | Assign entitlement cicd-app/free to subaccount "invmgmt-dev" | dev | T-001 | ✦ |

- [ ] T-007 `[dev]` Assign entitlement cloudfoundry/build-code to subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_entitlement`, `subaccount = invmgmt-dev`, `service = cloudfoundry`, `plan = build-code`, `quota_required = true`
- [ ] T-008 `[dev]` Assign entitlement hana-cloud/hana to subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_entitlement`, `subaccount = invmgmt-dev`, `service = hana-cloud`, `plan = hana`
- [ ] T-009 `[dev]` Assign entitlement hana-cloud-tools/tools to subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_entitlement`, `subaccount = invmgmt-dev`, `service = hana-cloud-tools`, `plan = tools`
- [ ] T-010 `[dev]` Assign entitlement auditlog/standard to subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_entitlement`, `subaccount = invmgmt-dev`, `service = auditlog`, `plan = standard`, `quota_required = true`
- [ ] T-011 `[dev]` Assign entitlement auditlog-viewer/free to subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_entitlement`, `subaccount = invmgmt-dev`, `service = auditlog-viewer`, `plan = free`
- [ ] T-012 `[dev]` Assign entitlement cloud-logging/standard to subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_entitlement`, `subaccount = invmgmt-dev`, `service = cloud-logging`, `plan = standard`, `quota_required = true`
- [ ] T-013 `[dev]` Assign entitlement build-workzone-standard/standard to subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_entitlement`, `subaccount = invmgmt-dev`, `service = build-workzone-standard`, `plan = standard`
- [ ] T-014 `[dev]` Assign entitlement build-code/free to subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_entitlement`, `subaccount = invmgmt-dev`, `service = build-code`, `plan = free`
- [ ] T-015 `[dev]` Assign entitlement destination/lite to subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_entitlement`, `subaccount = invmgmt-dev`, `service = destination`, `plan = lite`
- [ ] T-016 `[dev]` Assign entitlement cicd-app/free to subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_entitlement`, `subaccount = invmgmt-dev`, `service = cicd-app`, `plan = free`, `quota_required = true`

## Services — Instances

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-017 | Create service instance hana-cloud/hana in subaccount "invmgmt-dev" | dev | T-008 | ✦ |
| T-018 | Create service instance auditlog/standard in subaccount "invmgmt-dev" | dev | T-010 | ✦ |
| T-019 | Create service instance cloud-logging/standard in subaccount "invmgmt-dev" | dev | T-012 | ✦ |
| T-020 | Create service instance build-workzone-standard/standard in subaccount "invmgmt-dev" | dev | T-013 | ✦ |
| T-021 | Create CF service instance destination/lite in space "dev" | dev | T-005, T-006, T-015 | — |

- [ ] T-017 `[dev]` Create service instance hana-cloud/hana in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_service_instance`, `subaccount = invmgmt-dev`, `service = hana-cloud`, `plan = hana`, `location = btp`, `parameters = {"data": {"memory": 32, "edition": "cloud", "generateSystemPassword": true}}`
- [ ] T-018 `[dev]` Create service instance auditlog/standard in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_service_instance`, `subaccount = invmgmt-dev`, `service = auditlog`, `plan = standard`, `location = btp`, `quota_required = true`
- [ ] T-019 `[dev]` Create service instance cloud-logging/standard in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_service_instance`, `subaccount = invmgmt-dev`, `service = cloud-logging`, `plan = standard`, `location = btp`, `quota_required = true`
- [ ] T-020 `[dev]` Create service instance build-workzone-standard/standard in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_service_instance`, `subaccount = invmgmt-dev`, `service = build-workzone-standard`, `plan = standard`, `location = btp`
- [ ] T-021 `[dev]` Create CF service instance destination/lite in space "dev"
  - Task metadata: `resource_type = cloudfoundry_service_instance`, `subaccount = invmgmt-dev`, `service = destination`, `plan = lite`, `location = cf`, `cf_space = dev`, `parameters = {"HTML5Runtime_enabled": true}`

## Services — Subscriptions

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-022 | Subscribe to hana-cloud-tools/tools in subaccount "invmgmt-dev" | dev | T-009 | ✦ |
| T-023 | Subscribe to auditlog-viewer/free in subaccount "invmgmt-dev" | dev | T-011 | ✦ |
| T-024 | Subscribe to build-code/free in subaccount "invmgmt-dev" | dev | T-014 | ✦ |
| T-025 | Subscribe to cicd-app/free in subaccount "invmgmt-dev" | dev | T-016 | ✦ |

- [ ] T-022 `[dev]` Subscribe to hana-cloud-tools/tools in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_subscription`, `subaccount = invmgmt-dev`, `service = hana-cloud-tools`, `plan = tools`
- [ ] T-023 `[dev]` Subscribe to auditlog-viewer/free in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_subscription`, `subaccount = invmgmt-dev`, `service = auditlog-viewer`, `plan = free`
- [ ] T-024 `[dev]` Subscribe to build-code/free in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_subscription`, `subaccount = invmgmt-dev`, `service = build-code`, `plan = free`
- [ ] T-025 `[dev]` Subscribe to cicd-app/free in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_subscription`, `subaccount = invmgmt-dev`, `service = cicd-app`, `plan = free`, `quota_required = true`

## Security — Role Collections

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-026 | Create role collection "InvMgmt_Dev_Admin" in subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-027 | Create role collection "InvMgmt_Dev_Developer" in subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-028 | Create role collection "InvMgmt_Dev_Viewer" in subaccount "invmgmt-dev" | dev | T-001 | ✦ |
| T-029 | Create role collection "InvMgmt_Dev_AuditLog_Viewer" in subaccount "invmgmt-dev" | dev | T-001 | ✦ |

- [ ] T-026 `[dev]` Create role collection "InvMgmt_Dev_Admin" in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_role_collection_base`, `collection_name = InvMgmt_Dev_Admin`, `subaccount = invmgmt-dev`
- [ ] T-027 `[dev]` Create role collection "InvMgmt_Dev_Developer" in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_role_collection_base`, `collection_name = InvMgmt_Dev_Developer`, `subaccount = invmgmt-dev`
- [ ] T-028 `[dev]` Create role collection "InvMgmt_Dev_Viewer" in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_role_collection_base`, `collection_name = InvMgmt_Dev_Viewer`, `subaccount = invmgmt-dev`
- [ ] T-029 `[dev]` Create role collection "InvMgmt_Dev_AuditLog_Viewer" in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_role_collection_base`, `collection_name = InvMgmt_Dev_AuditLog_Viewer`, `subaccount = invmgmt-dev`

## Security — Role Collection Assignments

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-030 | Assign group "invmgmt-dev-admins" to "InvMgmt_Dev_Admin" | dev | T-026 | ✦ |
| T-031 | Assign group "invmgmt-dev-developers" to "InvMgmt_Dev_Developer" | dev | T-027 | ✦ |
| T-032 | Assign group "invmgmt-dev-viewers" to "InvMgmt_Dev_Viewer" | dev | T-028 | ✦ |
| T-033 | Assign group "invmgmt-dev-audit-viewers" to "InvMgmt_Dev_AuditLog_Viewer" | dev | T-029 | ✦ |

- [ ] T-030 `[dev]` Assign group "invmgmt-dev-admins" to "InvMgmt_Dev_Admin"
  - Task metadata: `resource_type = btp_subaccount_role_collection_assignment`, `subaccount = invmgmt-dev`, `role_collection_name = InvMgmt_Dev_Admin`, `group_name = invmgmt-dev-admins`, `origin = ainfvn15r-platform`
- [ ] T-031 `[dev]` Assign group "invmgmt-dev-developers" to "InvMgmt_Dev_Developer"
  - Task metadata: `resource_type = btp_subaccount_role_collection_assignment`, `subaccount = invmgmt-dev`, `role_collection_name = InvMgmt_Dev_Developer`, `group_name = invmgmt-dev-developers`, `origin = ainfvn15r-platform`
- [ ] T-032 `[dev]` Assign group "invmgmt-dev-viewers" to "InvMgmt_Dev_Viewer"
  - Task metadata: `resource_type = btp_subaccount_role_collection_assignment`, `subaccount = invmgmt-dev`, `role_collection_name = InvMgmt_Dev_Viewer`, `group_name = invmgmt-dev-viewers`, `origin = ainfvn15r-platform`
- [ ] T-033 `[dev]` Assign group "invmgmt-dev-audit-viewers" to "InvMgmt_Dev_AuditLog_Viewer"
  - Task metadata: `resource_type = btp_subaccount_role_collection_assignment`, `subaccount = invmgmt-dev`, `role_collection_name = InvMgmt_Dev_AuditLog_Viewer`, `group_name = invmgmt-dev-audit-viewers`, `origin = ainfvn15r-platform`

## Connectivity

| ID | Task | Stage | Depends on | Parallel |
|---|---|---|---|---|
| T-034 | Create destination "S4HANA_PC" in subaccount "invmgmt-dev" | dev | T-001 | — |

- [ ] T-034 `[dev]` Create destination "S4HANA_PC" in subaccount "invmgmt-dev"
  - Task metadata: `resource_type = btp_subaccount_destination_generic`, `subaccount = invmgmt-dev`, `name = S4HANA_PC`, `type = HTTP`, `proxy_type = Internet`, `url = https://mydemo-api.s4hana.ondemand.com`, `authentication = BasicAuthentication`, `user = INVENTORY_API_USER`, `password = <PLACEHOLDER>`, `properties = {TrustAll: "true"}`
