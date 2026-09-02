# XP265 - Define, generate, deploy: AI-driven spec-first Terraform

## Description

This repository contains the material for the SAP TechEd 2026 session called XP265 - Define, generate, deploy: AI-driven spec-first Terraform.

## Overview

This session introduces attendees to the concept of spec-driven development and how to use it for infrastructure as code (IaC) with Terraform. Attendees will experience an end-to-end flow from installing the relevant tooling, defining and refining the specification as well as generating and deploying the generated code to SAP BTP.

## Requirements

> [!NOTE]
> For TechEd attendees they will be provided with a pre-configured environment to run the exercises. As an attendee you can skip this section.

The requirements to follow the exercises in this repository are:

- A [GitHub](https://github.com/) account to clone the repository of the spec-kit
- A code editor like [Visual Studio Code](https://code.visualstudio.com/)
- A subscription to an AI coding assistant like [GitHub Copilot](https://github.com/features/copilot) or [Claude Code](https://claude.com/product/claude-code)
- The [Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
- A container runtime like [Docker](https://www.docker.com/get-started) or [Podman](https://podman.io/getting-started/installation)
- The [Terraform MCP Server](https://github.com/hashicorp/terraform-mcp-server) connected to your AI coding assistant of choice (e.g. GitHub Copilot or Claude Code)
- Access to an SAP BTP account with the necessary entitlements and authorization to deploy the generated Terraform code

Optionally you can also install:

- the [btp CLI](https://help.sap.com/docs/btp/sap-business-technology-platform/download-and-start-using-btp-cli-client)
- The [Terraform extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)


> [!CAUTION]
> Executing the steps that include the usage of a AI coding tool like Claude Code or GitHub Copilot will use tokens and consequently incur costs. Please make sure you are aware of the costs and have the necessary budget before executing the steps. SAP does not cover any costs for executing the steps in your own environment.

## Exercises

The exercises are designed to be completed in order, as each exercise builds upon the previous one. The exercises are structured as follows:

- [Getting Started](exercises/ex0/)
- [Exercise 1 - Initialize the project](exercises/ex1/)
- [Exercise 2 - Define the governance constraints](exercises/ex2/)
- [Exercise 3 - Define the scenario](exercises/ex3/)
- [Exercise 4 - Detail the account setup](exercises/ex4/)
- [Exercise 5 - Detail the service setup](exercises/ex5/)
- [Exercise 6 - Detail the security setup](exercises/ex6/)
- [Exercise 7 - Define the tasks](exercises/ex7/)
- [Exercise 8 - Evaluate the design](exercises/ex8/)
- [Exercise 9 - Generate the code](exercises/ex9/)
- [Exercise 10 - Deploy the generated code](exercises/ex10/)

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](https://github.com/SAP-samples/teched2026-XP265/issues/new) tab.

## License

Copyright 2026 SAP SE or an SAP affiliate company and teched2026-XP265 contributors. Please see our [LICENSE](LICENSE) for copyright and license information. Detailed information including third-party components and their licensing/copyright information is available [via the REUSE tool](https://api.reuse.software/info/github.com/SAP-samples/teched2026-XP265).
