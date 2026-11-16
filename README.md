# Ultra

Ultra connects an AI assistant to a team's workspace at [ultramcp.com](https://www.ultramcp.com). Once someone signs in, the assistant can search team knowledge, use shared skills, and create or find work cards. The same remote server is used from every client.

- Endpoint: `https://www.ultramcp.com/mcp` (streamable HTTP)
- Auth: OAuth for each person. This repository contains no API keys.
- Version: 1.0.0
- Support: [support@i.ultramcp.com](mailto:support@i.ultramcp.com)

[Privacy](https://www.ultramcp.com/privacy) · [Terms](https://www.ultramcp.com/terms)

## Install

The plugin lives at the root of this repository. `mcp.json` and `.mcp.json` are the same document: Agent Plugins clients read `mcp.json`, and Claude Code reads `.mcp.json`. Skills live in `skills/`.

### Cursor

Install **Ultra** from the [Cursor Marketplace](https://cursor.com/marketplace) when it is listed, or add this repository as a team marketplace: Dashboard → Plugins & MCPs → Add Marketplace → Import from Repo → `https://github.com/ultramcp/ultra-plugins`.

To add only the MCP server, use [Add to Cursor](https://cursor.com/install-mcp?name=ultra&config=eyJ1cmwiOiJodHRwczovL3d3dy51bHRyYW1jcC5jb20vbWNwIn0%3D). That page opens this install link:

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=ultra&config=eyJ1cmwiOiJodHRwczovL3d3dy51bHRyYW1jcC5jb20vbWNwIn0%3D
```

### Claude Code

```text
/plugin marketplace add ultramcp/ultra-plugins
/plugin install ultra@ultra-plugins
```

Then run `/mcp` and sign in when Claude asks.

### VS Code and GitHub Copilot

[Install in VS Code](https://vscode.dev/redirect/mcp/install?name=ultra&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fwww.ultramcp.com%2Fmcp%22%7D). That redirect opens:

```text
vscode:mcp/install?%7B%22name%22%3A%22ultra%22%2C%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fwww.ultramcp.com%2Fmcp%22%7D
```

Or add `.vscode/mcp.json` to a project:

```json
{
  "servers": {
    "ultra": {
      "type": "http",
      "url": "https://www.ultramcp.com/mcp"
    }
  }
}
```

`server.json` is the listing document for the [MCP Registry](https://github.com/modelcontextprotocol/registry), which feeds the VS Code and GitHub MCP gallery. The registry already lists `com.ultramcp/ultra` at 1.1.1 from another repository. This file is 1.0.0 for this package, and the registry's validate endpoint accepts it. Publishing it would replace that listing, so publish only when this repository should become the source, and use a version newer than the one already live.

### Other Agent Plugins clients

Clients that implement [Agent Plugins](https://agent-plugins.org/) load the repository root: `plugin.json`, `skills/`, and `mcp.json`. The server name is `ultra`.

## Authentication

The server answers unauthenticated requests with `401` and a `WWW-Authenticate` challenge for `https://www.ultramcp.com/.well-known/oauth-protected-resource/mcp`. The client opens Ultra's sign-in. Access follows that person's workspace permissions.

Leave tokens out of `mcp.json`, `.mcp.json`, and `.vscode/mcp.json`.

## Layout

```text
.
├── plugin.json                      Agent Plugins manifest
├── mcp.json                         Shared MCP config (Agent Plugins)
├── .mcp.json                        Same config, for Claude Code
├── .cursor-plugin/plugin.json       Cursor Marketplace manifest
├── .cursor-plugin/marketplace.json  Cursor marketplace entry for this repo
├── .claude-plugin/plugin.json       Claude Code manifest
├── .claude-plugin/marketplace.json  Claude Code marketplace
├── skills/use-ultra/SKILL.md        When to use the Ultra workspace
├── assets/logo.png                  Square logo
├── server.json                      MCP Registry listing
├── README.md
├── LICENSE
├── SECURITY.md
└── CHANGELOG.md
```

Claude Code pins an install to the `version` in `.claude-plugin/plugin.json` until that value changes. A release bumps the version in every manifest, in `server.json`, and in `CHANGELOG.md`.
