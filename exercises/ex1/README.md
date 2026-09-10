# Initialize the project

In this exercise, you will initialize the project and set up the necessary structure to begin the spec-driven flow in Claude Code.

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
│   └── commands
│       ├── btp-iac.accounts.md
│       ├── btp-iac.analyse.md
│       ├── btp-iac.design.md
│       ├── btp-iac.generate.md
│       ├── btp-iac.govern.md
│       ├── btp-iac.scenario.md
│       ├── btp-iac.security.md
│       ├── btp-iac.services.md
│       └── btp-iac.tasks.md
├── .gitignore
├── memory
├── specs
└── terraform
```

Let's walk through the files and folders in the project structure:

- The `.claude` folder contains all the commands and the corresponding instructions for the agentic tooling (in this case Claude Code). These are the **skills** — each one knows exactly what to build, what to validate, and what to write to disk. You will invoke them one by one throughout the exercises.
- The `.gitignore` file contains the files and folders that should be ignored by Git. This is important to avoid committing sensitive information or unnecessary files to the repository.
- The `memory` folder will contain the memory of the agentic tooling. The governance constraints written in Exercise 2 live here and are read by every subsequent skill.
- The `specs` folder will contain the specifications that will be created when you interact with the different commands. By the end of Exercise 8, this folder will contain a complete, human-readable specification for the inventory management environment — the primary artifact that drives code generation.
- The `terraform` folder will contain the generated code that will be used to create the infrastructure. This is the final output of the spec-driven development process.

> [!NOTE]
> Notice that the `specs` and `terraform` folders are empty at this point. They will be populated progressively as you work through the exercises. This is intentional — spec-driven development builds up the artifact incrementally, with a review opportunity at each step.

## 📋 Summary

Having this setup in place, we are now ready to start with the spec-driven development process. The first real step is defining the governance constraints that will act as guardrails for all subsequent decisions.

Continue to - [Exercise 2 - Define the governance constraints](../ex2/README.md)
