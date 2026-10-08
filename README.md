# XP265 - Define, generate, deploy: AI-driven spec-first Terraform

## Description

This repository contains the material for the SAP TechEd 2026 session "XP265 - Define, generate, deploy: AI-driven spec-first Terraform".

## Overview

This session introduces attendees to the concept of *spec-driven development* and how to use it for Infrastructure as Code (IaC) with Terraform. Attendees will experience an end-to-end flow from installing the relevant tooling, defining and refining the specification as well as generating and deploying the generated Terraform code to the SAP Business AI Platform.

## The Scenario

You are the responsible platform engineer for a development team building an **Inventory Management application** using SAP Cloud Application Programming Model (CAP). Before the development can start, you must provision the required environment on SAP Business AI Platform in accordance to your organization's governance rules and best practices.

The application has the following requirements mapping to the corresponding platform resources:

| Requirement | Service |
|---|---|
| Cloud Foundry runtime for deploying and testing the CAP app | Cloud Foundry Environment |
| Persistent database for inventory data | SAP HANA Cloud |
| Compliance audit trail for all inventory changes | SAP Audit Log Service |
| Centralized application and runtime logs | SAP Cloud Logging Service |
| Fiori Launchpad for the inventory UI | SAP Build Work Zone, standard edition |
| Development tools and CI/CD pipeline | SAP Build Code |
| Enterprise authentication (no default SAP ID Service) | SAP Cloud Identity Services |

As part of the governance rules for the platform, data must stay in the EU region, subaccounts must follow a naming convention, HANA Cloud costs must be tracked, and SAP Cloud Identity Services is mandatory as the identity provider.

Rather than hand-crafting Terraform from scratch or prompting an LLM to generate it from a single (or probably multiple) vague sentences, you will follow a **spec-driven development** flow. This will result in specification documents that the AI agent validates at each step before generating Terraform code.

## Requirements

> [!NOTE]
> For TechEd attendees the requirements to run the exercises will be provided with a pre-configured environment. As an attendee of the TechEd onsite session, you can skip this section.

The requirements to follow the exercises in this repository **on your own environment** are:

- A [GitHub](https://github.com/) account to clone the repository of the spec-kit
- A code editor like [Visual Studio Code](https://code.visualstudio.com/)
- A subscription to an AI coding assistant like [Claude Code](https://claude.com/product/claude-code). While the exercise in this repository focuses on using Claude Code, you can use other AI coding assistants if preferred.
- The [Terraform CLI](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)
- A container runtime like [Docker](https://www.docker.com/get-started) or [Podman](https://podman.io/getting-started/installation)
- The [Terraform MCP Server](https://github.com/hashicorp/terraform-mcp-server) connected to your AI coding assistant of choice
- Access to an SAP Business AI Platform account and authorization to deploy the generated Terraform code
- The [btp CLI](https://help.sap.com/docs/btp/sap-business-technology-platform/download-and-start-using-btp-cli-client)

We also recommend installing:

- The [Terraform extension for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=HashiCorp.terraform)

> [!CAUTION]
> If executing the exercises in your own environment, be aware that the steps that include the usage of agentic AI tools like Claude Code or GitHub Copilot will use tokens and consequently incur costs. Please make sure you are aware of the costs and have the necessary budget before executing the steps. SAP does not cover any costs for executing the steps in your own environment.

## Exercises

The exercises are designed to be completed in order, as each exercise builds upon the previous one. The exercises are structured as follows:

- [Getting Started](exercises/ex0/README.md) - the initial technical setup
- [Exercise 1 - Initialize the project](exercises/ex1/README.md) - set up the initial project structure for spec-driven development
- [Exercise 2 - Define the governance constraints](exercises/ex2/README.md) - define the guardrails for the project
- [Exercise 3 - Define the scenario](exercises/ex3/README.md) - specify the main intent of the project
- [Exercise 4 - Detail the account setup](exercises/ex4/README.md) - detail the account landscape
- [Exercise 5 - Detail the service setup](exercises/ex5/README.md) - detail the service setup (environment, entitle-only etc.)
- [Exercise 6 - Detail the security setup](exercises/ex6/README.md) - detail the security setup around role collections and trust
- [Exercise 7 - Detail the connectivity setup](exercises/ex7/README.md) - detail the connectivity setup namely destinations
- [Exercise 8 - Define the tasks](exercises/ex8/README.md) - create the todo list for the agent
- [Exercise 9 - Evaluate the design](exercises/ex9/README.md) - specify the terraform design
- [Exercise 10 - Generate the Terraform code](exercises/ex10/README.md) - generate the terraform code based on the specification documents
- [Exercise 11 - Deploy the generated Terraform code](exercises/ex11/README.md) - bring the Terraform code to life on the platform

## Troubleshooting

You find a troubleshooting guide as FAQ in the [troubleshooting section](troubleshooting/README.md). This guide can help you resolve common issues encountered during the exercises.

## Where to find the spec-kit?

The Infrastructure-as-Code Specification Toolkit for SAP Business AI Platform is available as open source (Apache-2.0 license) on Github at <https://github.com/SAP/iac-spec-kit-for-sap-business-ai-platform>.
The documentation for the spec-kit CLI, including setup instructions and usage guidelines, is available at <https://sap.github.io/iac-spec-kit-for-sap-business-ai-platform/>.

## Contributing

Please read the [CONTRIBUTING.md](./CONTRIBUTING.md) to understand the contribution guidelines.

## Code of Conduct

Please read the [SAP Open Source Code of Conduct](https://github.com/SAP-samples/.github/blob/main/CODE_OF_CONDUCT.md).

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](https://github.com/SAP-samples/teched2026-XP265/issues/new) tab.

## License

Copyright 2026 SAP SE or an SAP affiliate company and `teched2026-XP265` contributors. Please see our [LICENSE](LICENSE) for copyright and license information. Detailed information including third-party components and their licensing/copyright information is available [via the REUSE tool](https://api.reuse.software/info/github.com/SAP-samples/teched2026-XP265).
