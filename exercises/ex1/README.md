# Initialize the project

In this exercise, you will initialize the project and set up the necessary structure to begin the spec-driven flow in Claude Code

## 🛠️ Execute the spec kit init command

As a first step we must lay the foundation for the project. This is done by executing the `btp-iac init` command of the spec-kit CLI in a terminal. The command will create a project structure and a set of files that will be used in the following steps by the agentic tooling.

Open a terminal and execute the following command:

```bash
btp-iac init SAPTechEd2026-XXX
```
Replace the XXX with your group name.

You will then be asked to select the agentic tooling you want to use. For this session we will use **Claude Code**.

The command will create a folder with the name `SAPTechEd2026-XXX`. Navigate into the folder using the command:

```bash
cd SAPTechEd2026-XXX
```

As we want to inspect the project structure, we will open the content of the folder in the code editor Visual Studio Code. Execute the following command in the terminal:

```bash
code .
```
A VS Code screen opens and you should see the following project structure:

```bash
├── .claude
│   └── commands
│       ├── btp-iac.accounts.md
│       ├── btp-iac.analyse.md
│       ├── btp-iac.design.md
│       ├── btp-iac.generate.md
│       ├── btp-iac.govern.md
│       ├── btp-iac.scenario.md
│       ├── btp-iac.security.md
│       ├── btp-iac.services.md
│       └── btp-iac.tasks.md
├── .gitignore
├── memory
├── specs
└── terraform
```

Let's walk through the files and folders in the project structure:

- The `.claude` folder contains all the commands and the corresponding instructions for the agentic tooling (in this case Claude Code). The commands are structured in a way that they can be executed in a sequence to achieve the desired result.
- The `.gitignore` file contains the files and folders that should be ignored by Git. This is important to avoid committing sensitive information or unnecessary files to the repository.
- The `memory` folder will contain the memory of the agentic tooling. This is used to store the context of the conversation and the state of the project. It is important to keep this folder as it is, as it will be used in the following steps.
- The `specs` folder will contain the specifications that will be created when you interact with the different commands. The specifications are the primary artifact that will drive the code generation.
- The `terraform` folder will contain the generated code that will be used to create the infrastructure. This is the final output of the spec-driven development process.

## 📋 Summary

Having this setup in place, we are now ready to start with the spec-driven development process.

Continue to - [Exercise 2 - Define the governance constraints](../ex2/README.md)
