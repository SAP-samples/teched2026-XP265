# Getting started

## Motivation

In general, we would like to use the tools available for typical software development also when it comes to infrastructure-as-code (IaC). Looking at this topic in the age of AI and agentic engineering, we would of course like to leverage the power of LLMs to help us with that. But before we jump into that let us first revisit the typical vibe coding flow and its limitations.

The typical vibe coding flow is: write a prompt → LLM generates code → iterate with edits until it lands on something usable.

This is great for quick prototyping, but in this flow 100 tries may yield 100 different implementations, and there is no clear record of *why* the model made the decisions it did.

Looking at typical IaC projects that are much more complex this is a bad fit as:

- multiple people are involved in creating and maintaing the IaC Code
- a stable setup is required as IaC is the foundation for the app development
- the infrastructure management always requires auditability and traceability

The question there fore is: What is the alternative?

A disciplined alternative is the so called **Spec-driven Development**.

The approach is built on the idea that instead of prompting *what to build*, you prompt *what the system should do* (or in IaC terms what the infrastructure should look like) which targets its **behavior and constraints**.

Basically this approach produces a **specification** (the "contract"), which then drives:
  1. A **requirements document** (the authoritative hierarchy for the project)
  2. A **design document** with concrete implementation to-dos
  3. Only then: **code generation** by the LLM/agent
- At each stage you can review, approve, or refine before advancing — nothing is implemented until the spec is agreed on.

This approach comes with the following benefits:

- it reduces ambiguity for coding agents, which means fewer back-and-forth correction loops.
- the spec becomes the **primary artifact** that drives all downstream work: implementation, tests, documentation, verification.

However, the shift from coding to specifying is the hardest skill: instead of writing/reviewing code it's now  **effectively conveying what you want to build** to an LLM or agent.

As mentioned before spec-driven development is mainly targeting medium to complex setups, and due to its nature it is an overkill for a prototypes and small setups like creating a single subaccount resource.

> [!NOTE]
> While one might get the impression that spec-driven is a return back to waterfall, it is not. The spec-driven approach is still iterative and agile, but it is a more disciplined approach compared to a vibe coding approach.

## The Sample Scenario

What should be set up:

A full multi stage setup covering three stages, one team including entitlements, security settings.

## The Flow

Spec-driven development is a disciplined approach. All spec-kits support the user with guidance along a defined flow via a CLI and corresponding skills for the agentic tooling of your choice. The flow for our spec-kit is structured in the following steps:

1. Initialize the project
2. Define the Governance constraints
3. Define the Scenario
4. Detail the Account Setup
5. Detail the Service Setup
6. Detail the Security Setup
7. Define the Tasks
8. Evaluate the Design
9. Generate the Code

In the following sections we will go through each of the steps in detail and explain the intention and the technical foundation. You will execute each step in your own environment and we will discuss the expected results and the next steps.

## Technical Setup

### MCP Server for Terraform

- Start MCP Server for Terraform

### Wire up Claude Code to the MCP Server

- Wire up the MCP Server to Claude Code

### Install the spec-kit CLI

- Install the spec-kit CLI and make sure it is available in your PATH

## 📋 Summary

Now that you have ...
Continue to - [Exercise 1 - Exercise 1 Description](../ex1/README.md)
