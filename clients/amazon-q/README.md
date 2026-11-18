# Ultra for Amazon Q Developer

Ready-to-copy MCP config for the Amazon Q Developer CLI and the IDE plugins. The server is remote HTTP at `https://www.ultramcp.com/mcp`. The file contains no API key or token.

**Publication state:** manual config. Amazon Q has no public MCP catalog to submit to, and this is not an AWS Marketplace listing. A Pro-tier administrator can host an allow-list in the MCP registry subset and set that URL on the Q Developer profile. That registry is the organization's, not a public directory.

## Install

Copy [`mcp.json`](mcp.json) to one of these files, or merge the `ultra` entry into the `mcpServers` object that is already there.

CLI and legacy IDE file:

- User: `~/.aws/amazonq/mcp.json`
- Workspace: `.amazonq/mcp.json`

The IDE also reads that legacy file while `useLegacyMcpJson` is true, which is the default. The same JSON shape is an entry in a CLI agent under `~/.aws/amazonq/cli-agents`.

In the IDE, the MCP tools panel can add it instead of editing JSON: transport `http`, URL `https://www.ultramcp.com/mcp`. The panel writes `~/.aws/amazonq/default.json` for a global server and `.amazonq/default.json` for a workspace server. One AWS page names the IDE global file `~/.aws/amazonq/agents/default.json`. Use the file the panel opens.

Keep the CLI copy and the IDE panel copy separate. The IDE "Fix configuration" action can drop `type` and add `headers`, `timeout`, and `disabled`. The CLI expects `type` and `url`.

## Authentication

Remote HTTP MCP with OAuth is supported. No client secret goes in the config. Ultra uses OAuth 2.1 and dynamic client registration.

In the CLI, start a session that loads the server. It shows as not yet loaded. Run `/mcp`, open the URL Q prints, and finish sign-in in the browser while the session stays open. Tools appear after that.

In the IDE, saving an HTTP server that requires authorization opens the browser. Sign in at ultramcp.com. Access follows that person's workspace permissions.

## Revoke

In the CLI, run `qchat mcp remove` for `ultra`, or delete the `ultra` entry. In the IDE, open the MCP server and choose Delete MCP server. Removing the server drops the local session. Ultra's authorization server publishes `https://www.ultramcp.com/api/auth/oauth2/revoke` for a client that implements token revocation. There is no public catalog page that holds a grant.

## Supported versions

- Amazon Q Developer CLI 1.16.0 (17 September 2025) added remote HTTP MCP servers and OAuth. The AWS announcement on 18 September 2025 covers the CLI and the IDE plugins.
- Amazon Q for VS Code 1.92.0 (28 August 2025) added remote MCP servers. The IDE docs describe the browser step for an HTTP server that requires authorization. This package was not run against a Q install. The JetBrains plugin uses the same IDE docs; those docs do not give it a separate version number.
