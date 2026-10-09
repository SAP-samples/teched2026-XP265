# Troubleshooting

> [!NOTE]
> This section is only relevant for onsite attendees of the TechEd session XP265. In any case the on-site team is here to support you

In case you run into technical issues during the exercise, here are some common troubleshooting hints that can help you resolve them.

## Podman is not available for container execution

If `podman` is not available on your system, you can trigger the installation by:

1. Enter `LMU_XP265` into the windows search bar
2. Click on the `LMU_XP265` application to open it

This script will trigger the installation process for `podman` on your system.

## Podman is not running

As the MCP server runs as a container using `podman`, it requires the `podman` service to be up and running. If you see errors in Claude Code stating that the MCP server is not running or available, execute the following command:

```powershell
podman machine start
```

If podman was not running this command will start the tool, in case that podman is already running you will see a message like this:

```powershell
Starting machine "podman-machine-default"
Error: unable to start "podman-machine-default": already running
```

If the error still occurs after starting the podman machine, get in touch with the hands-on team onsite.

## Claude has no connection to an API

The connection details for the API are stored in the file `.claude/settings.json`. First check if the file exists and contains the correct API connection information. Open a terminal and enter:

```powershell
code .claude/settings.json
```

The file opened in VS Code should contain information like this:

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "<SOME API KEY>",
    "ANTHROPIC_BASE_URL": "https://xp265-teched-proxy.<SOME DOMAIN>",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "claude-sonnet-4-6",
    "ANTHROPIC_MODEL": "claude-sonnet-4-6",
    "CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS": "1",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1",
    "CLAUDE_CODE_ENABLE_TELEMETRY": "1",
    "DISABLE_ERROR_REPORTING": "1",
    "DISABLE_TELEMETRY": "1",
    "ENABLE_TOOL_SEARCH": "auto"
  },
  "includeCoAuthoredBy": false,
  "alwaysThinkingEnabled": false,
  "theme": "dark",
  "autoCompactEnabled": true,
  "gitAttribution": false
}
```

If this is not the case, get in touch with the hands-on team onsite.

## Login via the btp CLI fails

Make sure that the following prerequisites are met:

- You have specified the `--idp` parameter with the value `ainfvn15r`
- You have put the username and password into **quotes**

## The spec-kit CLI is not found

Make sure that the spec-kit CLI is installed and available in your system's PATH as described in [Exercise 0 - Install the spec-kit CLI](../exercises/ex0/README.md#install-the-spec-kit-cli).

## I see a red error message "Invalid Tool parameters"

This error does not interfere with the execution of the skill and can be ignored. The agent will continue to function correctly despite this message.

## The skill execution somehow gets off-track

Depending on your interaction with the agent, it might deviate from the expected sequence of steps. If you get the clear impression that the agent is off-track, do the following:

- Abort the execution by pressing the escape key
- Clear the context by entering `/clear`
- Redo the previous step by re-executing the relevant command

## "Compacting Conversation" message

Depending on the flow of the conversation you might see an interuption of the conversation with a message stating "Compacting Conversation". This indicates that the system is condensing previous messages to optimize performance. You can generally ignore this message. The conversation will resume automatically.

## Keyboard Layout changed

In case you accidentally changed your keyboard layout (from English to German or vice versa), you can switch it back to the desired layout via the key combination `Windows Key + Spacebar`.
