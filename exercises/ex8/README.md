# Detail the connectivity setup

The last missing piece for our scenario is the connectivity setup, namely the destination configuration in the subaccount. We already mentioned the destinations in our scenario, but now we will add the missing parts.

> [!TIP]
> In case you get stuck in one step and want to continue, you find a sample markdown document in the `sample` folder  that you can use as a **cross reference**. Do not copy it as it might not be perfectly fitting to the previous specifications that have been created by you.

## 🎯 Intent of the exercise

In this exercise we will add the details for the required destination to the S/4HANA backend system. As we do not have a real S/4HANA system setup, we will "simulate" the connectivity by configuring the destinations with a dummy URL.

## 🛠️ Execute the tasks command

👉 Execute the following command in the Claude Code terminal:

```powershell
/sap-iac.connectivity
```

Based on the information from the scenario the agent can identify the pattern for the connectivity setup. Based on that pattern, it will ask you for the necessary details to complete the configuration.

👉 Copy the following text to the terminal when prompted by the skill about the destination configuration:

```text
The URL is https://mydemo-api.s4hana.ondemand.com
The ProxyType is Internet
The authentication is BasicAuthentication
The user is INVENTORY_API_USER
The password should be supplied via a sesntive variable
The scope is subaccount
The destination needs an additional property `TrustAll` set to `true`
No destination certificates are required
```

As in the previous exercises, the agent will present a summary and ask for confirmation before applying the connectivity setup.

👉 Confirm by selecting `Yes`.

The agent will create a new file `specs/connectivity.md`.

<details>

<summary><b>🔬 Look behind the curtains</b></summary>

The technical foundation of this step is the skill `.claude/commands/sap-iac.connectivity.md`. Open the file and notice how the skill:

1. Identifies the necessary destination configuration based on the scenario and governance information.
2. Collects the required details from the user to complete the connectivity setup based on different patterns

</details>

**Key insight:** Destination configuration can be quite cumbersome because of its technical parameter names. The skill uses a pattern-based approach to help users provide the correct information efficiently.

### ✅ Result

The destination specific data is collected in `specs/connectivity.md`.

## 📋 Summary

The connectivity configuration is now fully specified. All specification documents are in place to form the complete picture of the `invmgmt-dev` environment. The next exercise translates the specification documents into tasks for the agent.

👉 Continue to - [Exercise 9 - Define the tasks](../ex9/README.md)
