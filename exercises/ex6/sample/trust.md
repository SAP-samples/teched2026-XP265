# BTP Trust & Security

## invmgmt-dev

### Custom IdP
- IdP URL: ainfvn15r.accounts.ondemand.com
- Origin: ainfvn15r-platform

### Role collections

- **Role collection**: InvMgmt_Dev_Admin
  - Roles: (none)

- **Role collection**: InvMgmt_Dev_Developer
  - Roles: (none)

- **Role collection**: InvMgmt_Dev_Viewer
  - Roles: (none)

- **Role collection**: InvMgmt_Dev_AuditLog_Viewer
  - Roles: (none)

### Role collection assignments (groups)

- group_name: invmgmt-dev-admins, origin: ainfvn15r-platform, role_collection_name: InvMgmt_Dev_Admin
- group_name: invmgmt-dev-developers, origin: ainfvn15r-platform, role_collection_name: InvMgmt_Dev_Developer
- group_name: invmgmt-dev-viewers, origin: ainfvn15r-platform, role_collection_name: InvMgmt_Dev_Viewer
- group_name: invmgmt-dev-audit-viewers, origin: ainfvn15r-platform, role_collection_name: InvMgmt_Dev_AuditLog_Viewer

### CF space user assignments

- space_name: dev, username: xp265-000@education.cloud.sap, origin: ainfvn15r-platform, role: space_developer
- space_name: dev, username: xp265-000@education.cloud.sap, origin: ainfvn15r-platform, role: space_manager
