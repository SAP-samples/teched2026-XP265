# BTP Services

## Subaccount: invmgmt-dev

### Entitlements

| Service offering | Plan | Consumption type | Quota required | Amount |
|---|---|---|---|---|
| `hana-cloud` | `hana` | service instance | — | — |
| `auditlog` | `standard` | service instance | quota_required | — |
| `cloud-logging` | `standard` | service instance | quota_required | — |
| `destination` | `lite` | service instance | — | — |
| `identity` | `application` | service instance | — | — |
| `cloudfoundry` | `standard` | service instance | — | — |
| `build-workzone-standard` | `standard` | service instance | — | — |
| `build-code` | `free` | subscription | — | — |
| `cicd-app` | `free` | subscription | quota_required | — |
| `APPLICATION_RUNTIME` | `MEMORY` | entitlement-only | — | 1 |

### Service instances

Dependency order: cloudfoundry → identity → destination → auditlog → cloud-logging → hana-cloud → build-workzone-standard

---

- service_offering_name: cloudfoundry
  service_plan_name: standard
  consumption_type: instance
  location: btp
  name: cf-invmgmt-dev

---

- service_offering_name: identity
  service_plan_name: application
  consumption_type: instance
  location: btp
  name: identity-invmgmt-dev

---

- service_offering_name: destination
  service_plan_name: lite
  consumption_type: instance
  location: cf
  cf_space: dev
  name: destination-invmgmt-dev
  parameters:
    HTML5Runtime_enabled: true

---

- service_offering_name: auditlog
  service_plan_name: standard
  consumption_type: instance
  location: btp
  quota_required: true
  name: auditlog-invmgmt-dev

---

- service_offering_name: cloud-logging
  service_plan_name: standard
  consumption_type: instance
  location: btp
  quota_required: true
  name: cloud-logging-invmgmt-dev

---

- service_offering_name: hana-cloud
  service_plan_name: hana
  consumption_type: instance
  location: btp
  name: hana-cloud-invmgmt-dev
  parameters:
    data:
      memory: 32
      edition: cloud
      generateSystemPassword: true

---

- service_offering_name: build-workzone-standard
  service_plan_name: standard
  consumption_type: instance
  location: btp
  name: build-workzone-invmgmt-dev

### Subscriptions

- service_offering_name: build-code
  service_plan_name: free
  consumption_type: subscription
  name: build-code-invmgmt-dev

- service_offering_name: cicd-app
  service_plan_name: free
  consumption_type: subscription
  quota_required: true
  name: cicd-app-invmgmt-dev

### Entitlement only

- service_offering_name: APPLICATION_RUNTIME
  service_plan_name: MEMORY
  consumption_type: entitlement-only
  amount: 1
