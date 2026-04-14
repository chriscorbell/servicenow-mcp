# ServiceNow MCP Setup

This repository includes ServiceNow MCP server templates for Codex, Claude Code, GitHub Copilot or OpenCode.

Follow the instructions below to configure your environment and connect to your ServiceNow instances.

### 1. Install dependencies

Install NodeJS and npm on your machine (Windows, macOS or Linux):

- https://nodejs.org/en/download

You will also need one of the following:

- Codex CLI (or Codex desktop app)
- Claude Code CLI (or Claude Code desktop app)
- VS Code with GitHub Copilot
- OpenCode CLI (or OpenCode desktop app)

### 2. Install the MCP server package

Install the ServiceNow MCP server globally:

```sh
npm install -g @aartiq/servicenow-mcp
```

Resolve the installed package path:

```sh
npm list -g @aartiq/servicenow-mcp --parseable
```

Append `/dist/server.js` to the end of the output of the above command - that will be the value for the `SERVICENOW_MCP_SERVER_JS_PATH` placeholder in the template files.

### 3. Copy the templates to local runtime files

From the root of this cloned repository, run:

```sh
cp .vscode/mcp.template.json .vscode/mcp.json
cp .codex/config.template.toml .codex/config.toml
cp opencode.template.json opencode.json
cp .mcp.template.json .mcp.json
```

These runtime files are ignored by `.gitignore` so credentials stay local.

### 4. Replace the placeholders

Fill in these values in each runtime file:

| Placeholder                     | Meaning                                                  |
| ------------------------------- | -------------------------------------------------------- |
| `SERVICENOW_MCP_SERVER_JS_PATH` | Absolute path to `@aartiq/servicenow-mcp/dist/server.js` |
| `DEV_INSTANCE_NAME`             | Dev ServiceNow instance name (subdomain)                 |
| `QA_INSTANCE_NAME`              | QA ServiceNow instance name (subdomain)                  |
| `UAT_INSTANCE_NAME`             | UAT ServiceNow instance name (subdomain)                 |
| `PROD_INSTANCE_NAME`            | Prod ServiceNow instance name (subdomain)                |
| `USERNAME`                      | Service account username                                 |
| `PASSWORD`                      | Service account password                                 |

Use full instance URLs such as `https://yourinstance.service-now.com`.

The templates are aligned to the workspace operating rules:

- `dev` is writable with scripting enabled for development work.
- `qa` is writable for automated testing, but scripting is disabled.
- `uat` is read-only, reserved for human-led acceptance testing and validation.
- `prod` is read-only, reserved for production operations.

`CMDB_WRITE_ENABLED` is disabled everywhere.

If you change these defaults, do it intentionally. Production should remain read-only.

### 5. Client-specific usage

#### Codex

1. Navigate to this cloned repo's directory in your terminal and then run `codex`
   - Alternatively, open the Codex desktop app and open this repo as a workspace
2. Codex reads the project-scoped MCP config from `./.codex/config.toml` in the cloned repo directory and automatically connects to all configured MCP servers
3. The `AGENTS.md` file in this repo is automatically loaded as workspace instructions, providing the agent with ServiceNow-specific context and operating rules

#### Claude Code

1. Navigate to this cloned repo's directory in your terminal and then run `claude`
   - Alternatively, open the Claude Code desktop app and open this repo as a workspace
2. Claude Code reads the project-scoped MCP config from `./.mcp.json` in the cloned repo directory and automatically connects to all configured MCP servers
3. The `CLAUDE.md` file in this repo is automatically loaded as workspace instructions, providing the agent with ServiceNow-specific context and operating rules

#### VS Code with GitHub Copilot

1. Open the cloned repo directory in VSCode (approve the MCP server trust prompt if shown)
2. Open the `.vscode/mcp.json` file in the editor and click "Start" above each server that you want to be able to connect to
3. Start a new GitHub Copilot chat - it automatically connects to all running MCP servers
4. The `AGENTS.md` file in this repo is automatically loaded as workspace instructions, providing the agent with ServiceNow-specific context and operating rules

#### OpenCode

1. Navigate to this cloned repo's directory in your terminal and then run `opencode`
   - Alternatively, open the OpenCode desktop app and open this repo as a workspace
2. OpenCode reads the project-scoped MCP config from `./opencode.json` in the cloned repo directory and automatically connects to all configured MCP servers
3. The `AGENTS.md` file in this repo is automatically loaded as workspace instructions, providing the agent with ServiceNow-specific context and operating rules

## Troubleshooting

### Server path issues

If the server fails to start, the `server.js` path is usually wrong. Re-run:

```sh
npm list -g @aartiq/servicenow-mcp --parseable
```

Then update the copied runtime configs.

### Authentication failures

- Confirm the username and password are correct.
- Confirm the instance URL is the full URL, including `https://`.
- Confirm `SERVICENOW_AUTH_METHOD` is still `basic`.

### VS Code server does not appear

- Make sure the runtime file is `.vscode/mcp.json`, not the template.
- Reload the window after any config change.
- Check the output channel for GitHub Copilot MCP logs.

### Codex or OpenCode do not show the server

- Make sure you copied the template to the exact runtime filename.
- Start the client from the cloned repo root so the project-scoped config is discovered.

### Claude Code does not connect to MCP servers

- Make sure the runtime file is `.mcp.json` (not the template) in the repo root.
- Start Claude Code from the cloned repo root so the project-scoped config is discovered.
- Run `/mcp` inside Claude Code to check server connection status and see any error output.

## 7. Recommended workflow

- Build and refine changes in `dev`
- Validate promoted changes via agent-led testing in `qa`
- Reserve `uat` for human-led acceptance testing
- Keep `prod` read-only
