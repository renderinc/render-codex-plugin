---
name: render-mcp-setup
description: Help users optionally set up the bundled Render MCP server in Codex for advanced workflows. Use when users want direct creation, richer monitoring, or structured queries beyond the Render CLI.
license: MIT
compatibility: Requires the Render plugin to be installed in Codex. The plugin is useful with the Render CLI alone. MCP access is optional and requires a Render API key exported as RENDER_API_KEY in the environment that launches Codex.
metadata:
  author: Render
  version: "1.0.0"
  category: setup
---

# Set up Render MCP in Codex

Use this skill when a user wants the optional bundled Render MCP server for advanced workflows.

## Goals

- Confirm whether the Render plugin is installed
- Explain when MCP is worth setting up
- Explain how to create a Render API key
- Explain exactly how to relaunch Codex from Terminal with `RENDER_API_KEY` set
- Help the user verify that Render MCP is available after restart

## What to tell the user

The Render plugin works with the Render CLI alone. MCP is optional and useful when the user wants direct creation, structured service enumeration, richer monitoring, or structured database queries.

The plugin does not provide an API key input in the Codex plugin UI. Codex reads the bearer token for the bundled MCP server from the environment that launches Codex.

To enable the bundled Render MCP server:

1. Create a Render API key in the Render Dashboard:
   - https://dashboard.render.com/u/*/settings#api-keys
2. Quit Codex if it is already open.
3. Launch Codex from Terminal with the key set:

```bash
export RENDER_API_KEY="rnd_..."
open -a Codex
```

4. Keep using that Terminal-launched Codex session. If they reopen Codex from the Dock or Spotlight, it might not inherit `RENDER_API_KEY`.
5. If they want this to persist, tell them to add the export line to their shell profile such as `~/.zshrc`, then launch Codex from a new Terminal session.

Warn them not to save a placeholder like `your_render_api_key` as the value of `RENDER_API_KEY`. A bad value can cause confusing auth failures.

## Verification

After restart, ask the user to verify one of these:

- The Render MCP server appears as connected in Codex
- A Render MCP-backed task now works
- `codex mcp list` shows the Render server if they are using the CLI

## Troubleshooting

If MCP is still unavailable:

- Confirm the plugin is installed and enabled
- Confirm `RENDER_API_KEY` is present in the environment that launched Codex
- Confirm Codex was restarted after setting the variable
- Confirm they relaunched Codex from Terminal instead of reopening it from the Dock
- Confirm the value is a real key, not a placeholder copied into shell config
- Explain that plugin metadata can bundle MCP config, but Codex still relies on the launch environment for bearer-token auth
