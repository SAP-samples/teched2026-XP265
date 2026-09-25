# Detail the connectivity setup

The last missing piece for our scenario is the connectivity setup, which defines the destination setup in the subaccount. We already mentioned the destinations in the earlier exercises, but now we will detail how to configure them properly.

## 🎯 Intent of the exercise

As we do not have a real S/4HANA system setup, we will simulate the connectivity by configuring the destinations with placeholder values.

| Property | Value |
|---|---|
| Name | InventoryQueryEndpoint |
| Type | HTTP |
| URL | https://mydemo-api.s4hana.ondemand.com |
| Proxy type | Internet |
| Authentication | BasicAuthentication |
| User | INVENTORY_API_USER |
| Password | test123 |

We also need to set one additional property:

| Property | Value |
|---|---|
| TrustAll | true |

## 🛠️ Execute the tasks command

Execute the following command in the Claude Code terminal:

```bash
/sap-iac.connectivity
```

The skill reads `specs/scenario.md` and `memory/governance.md`. Based on the information from the scenario it can identify the pattern for the connectivity setup. Based on that pattern, it will ask you for the necessary details to complete the configuration.

Copy the following text to the terminal when prompted by the skill about the data

```text
The URL is https://mydemo-api.s4hana.ondemand.com
The ProxyType is Internet
The authentication is BasicAuthentication
The user is INVENTORY_API_USER
The scope is subaccount
The destination needs an additional property `TrustAll` set to `true`
No destination certificates are required
```

The agent will present a summary and ask for confirmation before applying the connectivity setup. Confirm by selecting `Yes`. This will create a new file in `specs/` as `connectivity.md`.

### 🔬 Look behind the curtains

Open the `sap-iac.connectivity.md` skill file. Notice how the skill:

1. **Detected the pattern:** The skill checks if the description in the scenario matches known connectivity patterns and determines the necessary configuration steps accordingly.
2. **Avoided credential:** The skill does not ask your for sensitive information. If such information is required, it will put a placeholder into the specification.

The output `specs/connectivity.md` is the final piece of the specification puzzle — after this step, the agent has everything it needs to generate Terraform code.

### ✅ Result

After the command completes:

- Open `specs/connectivity.md` — you should see the connectivity configuration for the subaccount destination.
- Verify that the settings are correctly captured for the subaccount destination.

**Key insight:** Setting up destinations is a error-prone task esp. when automating the setup via Terraform or the btp CLI. The spec-driven approach based on examples ensures that the configuration is captured correctly and consistently, reducing the risk of misconfiguration.

## 📋 Summary

The connectivity configuration is now fully specified. The four spec files (`specs/landscape.md`, `specs/services.md`, `specs/trust.md`, `specs/connectivity.md`) together form the complete picture of the `invmgmt-dev` environment.

Continue to - [Exercise 8 - Define the tasks](../ex8/README.md)
