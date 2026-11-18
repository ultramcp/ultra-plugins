# Ultra for Windsurf and Devin Desktop

`docs.windsurf.com` redirects to the Devin Desktop docs at `https://docs.devin.ai/desktop/getting-started`. Local MCP setup is documented at `https://docs.devin.ai/desktop/devin-local/customizations`. Cascade and Devin Desktop read `mcp_config.json`. A remote server uses `serverUrl` or `url`. Both are set here to the same endpoint so a build that reads only one of them still connects. The file contains no API key or header.

**Publication state:** manual config. Devin Desktop has no public one-click listing for this package. A plugin can be offered to the Devin marketplace with a pull request; that pull request is not opened from this repository. The Windsurf MCP Plugin Store is what team allow-lists call a default server config. Its docs do not describe a public submission form.

## Install

Merge [`mcp_config.json`](mcp_config.json) into the file for the scope you want.

| Scope | Path |
| --- | --- |
| User | `~/.config/devin/mcp_config.json` (`$XDG_CONFIG_HOME/devin/mcp_config.json` when that variable is set). Windows: `%AppData%\devin\mcp_config.json` |
| Project | `.devin/mcp_config.json` |
| Project, this machine only | `.devin/mcp_config.local.json` |

In Devin Desktop, Customize → MCPs → This computer → open the config file, then refresh. Cascade's Open MCP config file action opens the same user file.

The editor's own discovery file is separate: `~/.codeium/windsurf/mcp_config.json` (stable) or `~/.codeium/windsurf-next/mcp_config.json` (Next). Enable the `windsurf` source, labeled Devin configurations, under `chat.mcp.discovery.enabled` if the editor should read that file.

Devin CLI uses the same `mcp_config.json` since v3000.3 (Local 3.6). Older builds keep `mcpServers` in the main config and migrate it on startup. The CLI field table calls the endpoint `url`. `serverUrl` is the Cascade name for the same value.

To install the folder as a Devin plugin instead of pasting JSON:

```text
devin plugins install --local ./clients/devin
devin plugins install ultramcp/ultra-plugins#clients/devin
```

The plugin manifest sets `url`, transport `http`, and `tokenScope` `user`. It has no client secret. Sign in with `devin mcp login ultra` or from Customize → MCPs.

## Authentication

Devin Local supports OAuth for Streamable HTTP and SSE. Ultra supports dynamic client registration, so no client id or secret is required, and Devin's callback URL does not need to be registered in advance. The sign-in page is ultramcp.com. Access follows that person's workspace permissions.

Choose Personal access where Devin asks who the connection is for. Organization access shares one connection with the whole org. Use Personal for a person's own workspace permissions.

When the stored grant expires, the server shows Needs authentication. Authenticate clears the stored credentials and opens the browser again.

## Revoke

Delete the `ultra` entry from `mcp_config.json`, or run `devin plugins uninstall ultra` if you installed the plugin. Authenticate again only when you want a new grant. Ultra's authorization server publishes `https://www.ultramcp.com/api/auth/oauth2/revoke` for a client that implements token revocation.

## Supported versions

Devin Desktop and Devin CLI current docs (November 2026): `mcp_config.json` since Devin CLI v3000.3 / Local 3.6, Streamable HTTP, and OAuth. Cascade's page documents `serverUrl` and `url` for remote HTTP. This package was checked against those docs, not against a Devin or Windsurf install.

## Marketplace

Devin marketplace intake is a pull request to [CognitionAI/devin-marketplace](https://github.com/CognitionAI/devin-marketplace):

- Add `plugins/ultra/.devin-plugin/plugin.json` with `mcpServers.ultra.url` set to `https://www.ultramcp.com/mcp`. Do not commit a token or client secret.
- Put a logo in that plugin directory.
- Add `"./plugins/ultra"` to `optionalPlugins` in the repository manifest.
- Run `python3 scripts/validate.py`.

Enterprise admins can also point Devin Desktop at MCP registries that follow the official registry schema. Those registries are configured per team. This repository does not submit one.
