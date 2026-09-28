# BTP Services — Inventory Management CAP (dev)

Subaccount: `invmgmt-dev` · Region: `eu20` · CF org: `invmgmt-cf-dev` · CF space: `dev`

---

## Dependency Order

Items must be provisioned in the order listed. Dependencies are noted per item.

---

## 1. Entitlements

Entitlements must be assigned to the subaccount before any service instance or subscription can be created.

| # | Service | Plan | Category | Notes |
|---|---|---|---|---|
| E1 | `xsuaa` | `application` | SERVICE | Required by CAP for OAuth2 / JWT-based auth wiring |
| E2 | `html5-apps-repo` | `app-host` | SERVICE | Hosts the Fiori UI static assets |
| E3 | `html5-apps-repo` | `app-runtime` | SERVICE | Serves Fiori UI assets at runtime |
| E4 | `destination` | `lite` | ELASTIC_SERVICE | Required for S/4HANA PC destination |
| E5 | `auditlog` | `oauth2` | SERVICE | Write-capable audit log plan for CUD compliance logging |
| E6 | `cloud-logging` | `dev` | SERVICE | Dev-tier log collection plan |
| E7 | `hana-cloud` | `hana` | ELASTIC_SERVICE | HANA Cloud instance; permitted for dev per governance |
| E8 | `hana` | `hdi-shared` | SERVICE | HDI container for CAP schema deployment |
| E9 | `identity` | `application` | SERVICE | SAP Cloud Identity Services — custom OIDC IdP |
| E10 | `build-workzone-standard` | `standard` | ELASTIC_SERVICE | Fiori launchpad; no free plan available (noted exception) |
| E11 | `build-code` | `free` | APPLICATION | SAP Build Code / Business Application Studio |
| E12 | `cicd-app` | `free` | QUOTA_BASED_APPLICATION | CI/CD pipelines |

---

## 2. Subscriptions

Subscriptions are SaaS applications activated at subaccount level. No service instance configuration required.

| # | Application | Plan | Depends on | Notes |
|---|---|---|---|---|
| S1 | `build-workzone-standard` | `standard` | E10 | Fiori launchpad hosting; activates after entitlement |
| S2 | `build-code` | `free` | E11 | SAP Business Application Studio for developers |
| S3 | `cicd-app` | `free` | E12 | CI/CD service for pipeline execution |

---

## 3. Service Instances

Service instances are created in the CF space `dev` unless noted as subaccount-level (marked `[SA]`).

### 3.1 SAP Authorization and Trust Management (XSUAA)

| Field | Value |
|---|---|
| Instance name | `invmgmt-xsuaa` |
| Service | `xsuaa` |
| Plan | `application` |
| Depends on | E1 |
| CF space | `dev` |

**Configuration parameters:**
```json
{
  "xsappname": "invmgmt-dev",
  "tenant-mode": "dedicated"
}
```

> Required by the CAP application for JWT-based authentication and role management. Must exist before the CAP app is deployed.

---

### 3.2 SAP Destination Service

| Field | Value |
|---|---|
| Instance name | `invmgmt-destination` |
| Service | `destination` |
| Plan | `lite` |
| Depends on | E4 |
| CF space | `dev` |

**Configuration parameters:** none (default configuration sufficient)

**Destination to configure after instance creation:**

| Field | Value |
|---|---|
| Destination name | `S4HANA_PC` |
| Type | `HTTP` |
| Authentication | `OAuth2SAMLBearerAssertion` or `OAuth2ClientCredentials` (confirm with S/4HANA PC OAuth2 app registration) |
| URL | S/4HANA Public Cloud API endpoint |
| Token service URL | S/4HANA PC OAuth2 token endpoint |

> Exact OAuth2 parameters (client ID, secret, token URL) are supplied from the S/4HANA Public Cloud side-channel configuration and are not stored in this spec.

---

### 3.3 SAP Audit Log Service

| Field | Value |
|---|---|
| Instance name | `invmgmt-auditlog` |
| Service | `auditlog` |
| Plan | `oauth2` |
| Depends on | E5 |
| CF space | `dev` |

**Configuration parameters:** none (plan default)

> The CAP application binds to this instance to emit compliance audit events for create, update, and delete operations on inventory records.

---

### 3.4 SAP Cloud Logging Service

| Field | Value |
|---|---|
| Instance name | `invmgmt-cloud-logging` |
| Service | `cloud-logging` |
| Plan | `dev` |
| Depends on | E6 |
| CF space | `dev` |

**Configuration parameters:**
```json
{
  "ingest_otlp": {
    "enabled": true
  }
}
```

> Collects application and CF runtime logs. The CAP app and CF runtime are bound to this instance for log forwarding.

---

### 3.5 SAP HANA Cloud

| Field | Value |
|---|---|
| Instance name | `invmgmt-hana-cloud` |
| Service | `hana-cloud` |
| Plan | `hana` |
| Depends on | E7 |
| Level | Subaccount (managed via HANA Cloud Central or BTP cockpit) `[SA]` |

**Configuration parameters:**
```json
{
  "data": {
    "memory": 32,
    "generateSystemPassword": true,
    "whitelistIPs": []
  }
}
```

> 30 GB is the minimum HANA Cloud instance size. Metered service — cost-centre tag `cost-center: CC-123456` applies per governance.

---

### 3.6 SAP HANA HDI Container

| Field | Value |
|---|---|
| Instance name | `invmgmt-hdi-container` |
| Service | `hana` |
| Plan | `hdi-shared` |
| Depends on | E8, 3.5 (HANA Cloud instance must be running) |
| CF space | `dev` |

**Configuration parameters:** none (defaults)

> The CAP application's `db` module binds to this instance for schema deployment via HDI.

---

### 3.7 SAP Cloud Identity Services

| Field | Value |
|---|---|
| Instance name | `invmgmt-identity` |
| Service | `identity` |
| Plan | `application` |
| Depends on | E9 |
| CF space | `dev` |

**Configuration parameters:**
```json
{
  "oauth2-configuration": {
    "grant-types": ["authorization_code", "client_credentials"],
    "token-policy": {
      "token-validity": 3600,
      "refresh-token-validity": 43200
    }
  },
  "subject-name-identifier": {
    "format": "email"
  },
  "consumed-services": []
}
```

> Creates a dedicated application registration in the SAP Cloud Identity Services tenant. Used as the custom OIDC IdP for the CAP application. The default SAP ID Service is not permitted per governance.

---

### 3.8 HTML5 Application Repository — App Host

| Field | Value |
|---|---|
| Instance name | `invmgmt-html5-host` |
| Service | `html5-apps-repo` |
| Plan | `app-host` |
| Depends on | E2 |
| CF space | `dev` |

**Configuration parameters:** none

> Used during `cf deploy` (MTA) to upload the Fiori UI static assets. One instance per deployment unit.

---

## 4. Full Dependency Graph

```
E1 (xsuaa)            → 3.1 (xsuaa instance)          → CAP app deployment
E2 (html5 app-host)   → 3.8 (html5 host instance)     → CAP app deployment (MTA)
E3 (html5 app-runtime) → (consumed by build-workzone)
E4 (destination)      → 3.2 (destination instance)    → S4HANA_PC destination config
E5 (auditlog)         → 3.3 (auditlog instance)        → CAP app binding
E6 (cloud-logging)    → 3.4 (cloud-logging instance)  → CAP app + CF runtime binding
E7 (hana-cloud)       → 3.5 (HANA Cloud instance)     → 3.6 (HDI container)
E8 (hana hdi-shared)  → 3.6 (HDI container)           → CAP db module binding
E9 (identity)         → 3.7 (identity instance)        → trust config → CAP app
E10 (build-workzone)  → S1 (subscription)              → Fiori launchpad
E11 (build-code)      → S2 (subscription)              → developer tooling
E12 (cicd-app)        → S3 (subscription)              → CI/CD pipelines
```

---

## 5. Governance Compliance

| Check | Status |
|---|---|
| Dev plan — `hana` for `hana-cloud` | permitted per governance |
| Dev plan — `oauth2` for `auditlog` | write-capable plan required for CUD compliance logging; `free` plan is read-only viewer only |
| Dev plan — `dev` for `cloud-logging` | permitted (`free` tier) |
| Dev plan — `lite` for `destination` | free elastic plan |
| Dev plan — `free` for `build-code` | compliant |
| Dev plan — `free` for `cicd-app` | compliant |
| `build-workzone-standard` `standard` | no free plan available — noted exception from scenario |
| Metered service | `hana-cloud` flagged; cost-centre tag `cost-center: CC-123456` applied |
