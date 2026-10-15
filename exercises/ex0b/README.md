# Technical Setup

Before starting with the exercises some technical setup is required to ensure that all tools and services are properly configured.

You need the following components set up before starting the exercises:

1. MCP Server for Terraform
2. Configure Claude Code to connect to the MCP Server for Terraform
3. Install the spec-kit CLI
4. Check your access to your Business AI Platform account via the cockpit and btp CLI

The following sections guide you through the setup process for each of the required components.

It is important to use **PowerShell** as the environment variable syntax targets this shell. Do not use Git Bash or `cmd`. 

You start PowerShell via:

![Powershell Screenshot 1](./images/0a_powershell.png)

You will also see the version you are using on the tab of the terminal:

![Powershell Screenshot 2](./images/0b_powershell.png)


The Claude Code slash commands (`/sap-iac.*`) and all Terraform commands are platform-independent. 

### MCP Server for Terraform

We require the [MCP server](https://modelcontextprotocol.io/docs/2026-07-28/learn/server-concepts) for [Terraform](https://github.com/hashicorp/terraform-mcp-server) to give the agent access to the information about the Terraform providers available on the Terraform registry. 

On the system we are using [podman](https://podman.io/) as the container runtime. To install and run the MCP server, open a PowerShell window and execute the following commands:

👉 1. Start podman:

      ```powershell
      podman machine start
      ```

👉 2. Install and start the MCP server:

      ```powershell
      podman run -i --rm hashicorp/terraform-mcp-server
      ```

> [!NOTE]
> Some error might occur stating the the `TFE client` and the `TFE tools` will not be available. This is okay as we do not connect to a Terraform Enterprise instance.

Stop the MCP server by pressing `Ctrl+C` in the terminal where it is running. This step was required to initially download the MCP server image. In the next step we will instruct Claude Code to start the MCP server on its own.

### Wire up Claude Code to the MCP Server

👉 We need to setup the connection between Claude and the MCP server for Terraform that we just installed. To do so go to your Powershell terminal windows and enter the following command:

```powershell
claude mcp add terraform -s user -t stdio -- podman run -i --rm hashicorp/terraform-mcp-server
```
👉 You should get a confirmation message indicating that the MCP server has been successfully added to Claude Code:

```powershell
Added stdio MCP server terraform with command podman run -i --rm hashicorp/terraform-mcp-server to user config
```

👉 Verify that the MCP server has been correctly added to Claude Code by listing the configured MCP servers:

```powershell
claude mcp list
```
You should see an entry for the `terraform` MCP server, confirming that it is connected to Claude Code.

### Verify the model used in Claude

👉 To ensure that Claude is using the correct model, in the Powershell terminal run the following command:

```powershell
claude
```
You can select `Yes, I trust this folder` to continue.


👉 Next, enter `/model` and validate that the model is `Sonnet 4.6` and the reasoning effort is `High`, as shown in this screenshot:

![Claude Model Verification](./images/1_claude_model_verification.png)

If this is not the case select the right model and/or adjust the reasoning effort accordingly.

👉 Leave Claude for now by entering `/exit` in the terminal.

### Install the spec-kit CLI

> [!NOTE]
> If you are doing the hands-on on your own, take a look at the [documentation](https://sap.github.io/iac-spec-kit-for-sap-business-ai-platform/setup/) for the installation of the spec-kit CLI on the different platforms.

The central tool for this exercise is the spec-kit CLI. You need to install it manually:

👉 1. Open a terminal window.
👉 2. Run the following command in powershell terminal to download and install the spec-kit CLI:

      ```powershell
      iwr -useb https://raw.githubusercontent.com/SAP/iac-spec-kit-for-sap-business-ai-platform/refs/heads/main/sap-iac-install.ps1 | iex
      ```

`🎊 Easy installation via winget will be available soon!`

👉 Verify that the spec-kit CLI is correctly installed and accessible from your terminal by opening a new terminal and running:

```powershell
sap-iac
```

You should see the following output:

```powershell
Infrastructure-as-Code Specification Toolkit for SAP Business AI Platform

Usage:
  sap-iac [command]

Available Commands:
  catalogue   Manage the service parameters catalogue
  completion  Generate the autocompletion script for the specified shell
  help        Help about any command
  init        Bootstrap or update a project
  version     Print the binary version

Flags:
  -h, --help   help for sap-iac

Use "sap-iac [command] --help" for more information about a command.
```

### Validate your access to the platform

As we want to deploy resources to the SAP Business AI Platform, you need to have access to the platform. Validate this both via the cockpit and the btp CLI.

Your username is in the format `XP265-<0XX>`, where `0XX` should be replaced with your group number. The email of the user is `xp265-<0XX>@education.cloud.sap`
Your Password is the one provided by the hands-on team during the session.

#### Cockpit

- Open a browser and navigate to the URL <https://emea.cockpit.btp.cloud.sap/cockpit/?idp=ainfvn15r.accounts.ondemand.com#/>
- Log in with the credentials provided by the hands-on team during the session

Keep the browser window open as you will need the subdomain of the global account in the next exercise.

#### btp CLI

👉 Verify your access via the btp CLI by running, replace the `0XX` in the username with your group number::

```powershell
btp login --idp ainfvn15r --user 'XP265-<0XX>' --password '<your-password>'
```

> [!NOTE]
> Put the username and the password into single quotes

Confirm the defaulted CLI server URL (`https://cli.btp.cloud.sap`)

You should see a success message indicating that you have successfully logged in via the btp CLI.

## 📋 Summary

You now understand the motivation for spec-driven development, the hands-on scenario and its constraints.

In addition, you have installed the prerequisites for the exercises and validated your access to the SAP Business AI Platform. Now the real fun can start!

👉 Continue to - [Exercise 1 - Initialize the project](../ex1/README.md)
