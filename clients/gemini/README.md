# Ultra for Gemini CLI

Extension for [Gemini CLI](https://geminicli.com/docs/extensions/reference/). It adds the remote MCP server `UltraMCP` at `https://www.ultramcp.com/mcp`. OAuth is enabled in `gemini-extension.json`. The file contains no API key or client secret. Ultra registers the client with dynamic client registration.

**Publication state:** manual install from a local checkout or from a GitHub Release archive. This repository is not in the Gemini CLI extension gallery. Do not add the `gemini-cli-extension` topic here.

## Install

`gemini extensions install <github-url>` and the gallery both treat the extension root as the repository root, or as the root of a release archive. A subdirectory is not an install source ([google-gemini/gemini-cli#25676](https://github.com/google-gemini/gemini-cli/issues/25676)). This file lives in `clients/gemini/` so the multi-client package can stay at the repository root.

From a checkout of this repository:

```text
gemini extensions validate ./clients/gemini
gemini extensions install ./clients/gemini --consent
```

The install command still asks you to trust the folder. `--consent` skips the third-party warning, not that question. `gemini extensions link ./clients/gemini` points the CLI at this directory while you are editing it.

The installed extension name is `ultra`. Restart Gemini CLI after installing. In the session, `/extensions list` shows it, and `/mcp auth UltraMCP` starts sign-in.

### Release archive

[`.github/workflows/gemini-extension-archive.yml`](../../.github/workflows/gemini-extension-archive.yml) builds `ultra.tar.gz` with `gemini-extension.json` at the archive root. Pull requests upload that archive as a workflow artifact. Pushing a `gemini-v*` tag publishes it as the only asset on that GitHub Release.

Gemini CLI installs the Latest release asset when the repository has one, and uses that single generic asset when its name does not contain `darwin`, `linux`, or `win32`. After that release exists:

```text
gemini extensions install https://github.com/ultramcp/ultra-plugins
```

Until then, the same command clones this repository and looks for `gemini-extension.json` at the clone root, which is not where this package keeps it.

A `gemini-v*` release becomes this repository's Latest release. Later plugin releases that are marked Latest, and that do not carry `ultra.tar.gz` as their only asset, replace the install source. The clean public install is a dedicated repository, `ultramcp/ultra-gemini-extension`, with this manifest at the root and the `gemini-cli-extension` topic on that repository. The gallery crawls that topic daily and does not take a separate submission.

## Authentication

The server answers an unauthenticated request with `401` and a `WWW-Authenticate` challenge for `https://www.ultramcp.com/.well-known/oauth-protected-resource/mcp`. Gemini CLI discovers the authorization server, registers a public client, and opens a browser on `http://localhost:<port>/oauth/callback`. Sign in at ultramcp.com. Access follows that person's workspace permissions.

The current CLI schema still accepts `httpUrl` for Streamable HTTP and `oauth.enabled` to turn OAuth on. `httpUrl` is deprecated in favor of `url` with `type` set to `http`; this package keeps `httpUrl` because that field still selects Streamable HTTP. Do not put a client id or client secret in the manifest.

Tokens are stored in `~/.gemini/mcp-oauth-tokens.json`. Run `/mcp auth UltraMCP` again when a token expires. Headless and remote sessions cannot finish the browser redirect.

## Revoke

Run `gemini extensions uninstall ultra`, then delete the `UltraMCP` entry in `~/.gemini/mcp-oauth-tokens.json` if it is still there. That stops this CLI from using the grant. Ultra's authorization server publishes `https://www.ultramcp.com/api/auth/oauth2/revoke` for a client that implements token revocation. This package does not document a connected-apps page.

## Supported versions

Gemini CLI 0.68.0 (the November 2026 release that added the stricter `extensions validate` checks). `oauth.enabled` is the OAuth switch. `httpUrl` still selects Streamable HTTP. Validate prints a deprecation warning and exits 0. Older releases that only document `httpUrl` can load the same manifest. Confirm the copy you install with `gemini extensions validate ./clients/gemini`.
