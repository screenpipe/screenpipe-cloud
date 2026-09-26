# Screenpipe Cloud

Search your synced screen content, audio transcriptions, app data, and memories
across Screenpipe devices. Requires a Screenpipe account with Data Sync available
and enabled. Only records already synced to that account are searchable.

This cloud connector provides `synced-devices` and `search-synced-content`.
It does not expose local-only recordings, desktop control, or enterprise team
records. Signing in does not enable Data Sync or start uploading recordings.

## Quick install in Cursor or VS Code

[Install in Cursor](https://cursor.com/en/install-mcp?name=screenpipe-cloud&config=eyJ0eXBlIjoiaHR0cCIsInVybCI6Imh0dHBzOi8vc2NyZWVucGlwZS5jb20vYXBpL3VzZXIvZGF0YS1zeW5jL21jcCJ9) ·
[Install in VS Code](https://insiders.vscode.dev/redirect/mcp/install?name=screenpipe-cloud&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fscreenpipe.com%2Fapi%2Fuser%2Fdata-sync%2Fmcp%22%7D)

Each link opens your client's installation prompt with the HTTPS endpoint
already filled in. Review the configuration, then sign in to Screenpipe through
OAuth. Data Sync must already be available and enabled on your account. These
links do not enable uploads or bypass the client's approval step.

If your browser cannot open the installed app, add this remote MCP server in
your client's MCP settings:

```json
{
  "type": "http",
  "url": "https://screenpipe.com/api/user/data-sync/mcp"
}
```

See the official [Cursor install-link documentation](https://prod.cursor.com/docs/mcp/install-links)
and [VS Code MCP guide](https://code.visualstudio.com/api/extension-guides/ai/mcp).
The link configuration has been checked against those formats; an authenticated
end-to-end session in either client has not yet been verified.

## ChatGPT

For a private developer connection, enable Developer mode if permitted by your
account/workspace, open [Plugins](https://chatgpt.com/plugins), select the plus
button, and connect this HTTPS MCP endpoint:

```text
https://screenpipe.com/api/user/data-sync/mcp
```

Complete Screenpipe's OAuth sign-in and review the tools. Use a new conversation
to ask which devices have synced recently. An empty device list does not mean
the connector can see local-only recordings.

The ZIP is a portable plugin package for local plugin hosts and private testing.
It is not a public ChatGPT listing. Public installation from the directory
requires a **With MCP** submission and platform review; submit the endpoint,
not a localhost URL or the Claude Desktop `.mcpb` file.

## Claude

In Claude's connector settings, add a custom remote connector with the same
HTTPS URL and complete OAuth sign-in. Availability depends on workspace policy.
This repository includes `.claude-plugin/plugin.json` and `.mcp.json` for Claude
plugin hosts. For Claude Code local development, clone this repository and run:

```sh
claude --plugin-dir /absolute/path/to/screenpipe-cloud
```

Use `/mcp` to finish authentication. No API token belongs in the archive.

For recordings that stay on your computer, use the separate
`screenpipe-local.mcpb` installer in Claude Desktop instead. Install it through
Settings → Extensions → Advanced settings → Install Extension, then select the
file. Keep Screenpipe running. Its optional local API key field is stored by
the host as a sensitive setting; if blank, the existing Screenpipe CLI discovery
path is used. Do not enable both local connection methods under the same server
name.

## Privacy and support

The cloud connector reads records already uploaded through Data Sync. Results
you request are sent to the connected AI provider. Its OAuth scope is
`data-sync:read`; it has no write tools. Disconnect it in your AI client's
settings when you no longer want access.

[Privacy](https://screenpipe.com/privacy) ·
[Terms](https://screenpipe.com/terms) ·
[Support](https://github.com/screenpipe/screenpipe/issues)

Packaging references:
[OpenAI](https://developers.openai.com/plugins/build/plugins),
[Claude plugins](https://code.claude.com/docs/en/plugins-reference),
[Claude Desktop bundles](https://github.com/anthropics/mcpb).

## Distribution source

This small repository distributes the cloud plugin built and tested in
[Screenpipe PR #7310](https://github.com/screenpipe/screenpipe/pull/7310).
The initial files come from the
[successful MCP installer CI run](https://github.com/screenpipe/screenpipe/actions/runs/36202966947).
It is separate from the main repository so directory scanners do not need to
fetch the desktop application's Git LFS assets.

For the Claude directory submission, use repository `screenpipe/screenpipe-cloud`,
branch `main`, and leave the plugin path empty. Submit the remote MCP endpoint
separately as an MCP connector. A repository or ZIP is not a reviewed directory
listing.

To update this distribution, build and test `packages/screenpipe-mcp` in the main
repository, copy the contents of `installers/screenpipe-cloud`, and review the
resulting diff here. Keep credentials and recording data out of the repository.
