# Troubleshooting

In case you run into techical issues during the exercise, here are some common troubleshooting hints that can help you resolve them.

## Podman is not available for container execution

In case `podman` is not available on your system, you can trigger the instalation by:

1. Enter `LMU_XP265` into the windows search bar
2. Click on the `LMU_XP265` application to open it

This will trigger the installation process for `podman` on your system.

## Podman is not running

As the MCP server is run as a container using `podman`, it requires the `podman` service to be running. if you see errors in Cluade Code stating that the MCP server is not running, execute the following command:

```powershell
podman machine start
```

If podman was not running this command will start the tool, in case that podman is already running you will see a message like this:

```powershell
Starting machine "podman-machine-default"
Error: unable to start "podman-machine-default": already running
```

## Claude has no connection to an API

The connection details for the API are stored in the file `.claude/settings.json`. First check if the file exists and contains the correct API connection information. Open a terminal and e

```powershell
code .claude/settings.json
```

The file should contain information like this:

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

If this is not the case get in touch with the handson team onsite.

## The login to SAP BTP via the btp CLI fails

Make sure that the following points are met:

- You have specified the `--idp` parameter with the value `ainfvn15r`
- You have set the username and password into quotes

## The Spec-kit CLI is not found

Make sure that the Spec-kit CLI is installed and available in your system's PATH as described in [Exercise 0 - Install the Spec-kit CLI](../exercises/ex0/README.md#install-the-spec-kit-cli).
