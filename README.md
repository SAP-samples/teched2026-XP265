# XP265 - Define, generate, deploy: AI-driven spec-first Terraform

## Description

This repository contains the material for the SAP TechEd 2026 session "XP265 - Define, generate, deploy: AI-driven spec-first Terraform".

## Overview

This session introduces attendees to the concept of *spec-driven development* and how to use it for Infrastructure as Code (IaC) with Terraform. Attendees will experience an end-to-end flow from installing the relevant tooling, defining and refining the specification as well as generating and deploying the generated code to the SAP Business AI Platform.

## The Scenario

You are the infrastructure lead for a development team building an **Inventory Management application** using SAP Cloud Application Programming Model (CAP). Before a single line of application code can be written, you need to provision the complete developer environment on SAP Business AI Platform — correctly, consistently, and in a way that can be reproduced and audited.

The application needs:

| Requirement | Service |
|---|---|
| Cloud Foundry runtime for deploying and testing the CAP app | Cloud Foundry Environment |
| Persistent database for inventory data | SAP HANA Cloud |
| Compliance audit trail for all inventory changes | SAP Audit Log Service |
| Centralized application and runtime logs | SAP Cloud Logging Service |
| Fiori Launchpad for the inventory UI | SAP Build Work Zone, standard edition |
| Development tools and CI/CD pipeline | SAP Build Code |
| Enterprise authentication (no default SAP ID Service) | SAP Cloud Identity Services |

The team operates under real-world governance rules: data must stay in the EU region, subaccounts must follow a naming convention, HANA Cloud costs must be tracked, and SAP Cloud Identity Services is mandatory as the identity provider.

Rather than hand-crafting Terraform from scratch or prompting an LLM to generate it from a single (or multiple) vague sentences, you will follow a **spec-driven development** flow — producing a living specification that the AI agent validates at each step before writing a single line of Terraform code.

## Requirements

> [!NOTE]
> For TechEd attendees the requirements to run the exercises will be provided with a pre-configured environment. As an attendee of the TechEd onsite session, you can skip this section.

The requirements to follow the exercises in this repository *on your own environment* are:

- A [GitHub](https://github.com/) account to clone the repository of the spec-kit
- A code editor like [Visual Studio Code](https://code.visualstudio.com/)
- A subscription to an AI coding assistant like [Claude Code](https://claude.com/product/claude-code). The exercise in this repository focuses on using Claude Code, but you can use other AI coding assistants if preferred.
- The [Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
- A container runtime like [Docker](https://www.docker.com/get-started) or [Podman](https://podman.io/getting-started/installation)
- The [Terraform MCP Server](https://github.com/hashicorp/terraform-mcp-server) connected to your AI coding assistant of choice
- Access to an SAP Business AI Platform account and authorization to deploy the generated Terraform code
- The [btp CLI](https://help.sap.com/docs/btp/sap-business-technology-platform/download-and-start-using-btp-cli-client)

We also recommend installing:

- The [Terraform extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)

> [!CAUTION]
> If executing the exercises in your own environment, be aware that the steps that include the usage of a AI coding tool like Claude Code or GitHub Copilot will use tokens and consequently incur costs. Please make sure you are aware of the costs and have the necessary budget before executing the steps. SAP does not cover any costs for executing the steps in your own environment.

## Exercises

The exercises are designed to be completed in order, as each exercise builds upon the previous one. The exercises are structured as follows:

- [Getting Started](exercises/ex0/README.md)
- [Exercise 1 - Initialize the project](exercises/ex1/README.md)
- [Exercise 2 - Define the governance constraints](exercises/ex2/README.md)
- [Exercise 3 - Define the scenario](exercises/ex3/README.md)
- [Exercise 4 - Detail the account setup](exercises/ex4/README.md)
- [Exercise 5 - Detail the service setup](exercises/ex5/README.md)
- [Exercise 6 - Detail the security setup](exercises/ex6/README.md)
- [Exercise 7 - Detail the connectivity setup](exercises/ex7/README.md)
- [Exercise 8 - Define the tasks](exercises/ex8/README.md)
- [Exercise 9 - Evaluate the design](exercises/ex9/README.md)
- [Exercise 10 - Generate the Terraform code](exercises/ex10/README.md)
- [Exercise 11 - Deploy the generated Terraform code](exercises/ex11/README.md)

## Troubleshooting

You find a FAQ-like troubleshooting guide in the [troubleshooting section](troubleshooting/README.md) that can help you resolve common issues encountered during the exercises.

## Contributing
Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct
Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](https://github.com/SAP-samples/teched2026-XP265/issues/new) tab.

## License

Copyright 2026 SAP SE or an SAP affiliate company and teched2026-XP265 contributors. Please see our [LICENSE](LICENSE) for copyright and license information. Detailed information including third-party components and their licensing/copyright information is available [via the REUSE tool](https://api.reuse.software/info/github.com/SAP-samples/teched2026-XP265).
