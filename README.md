# Render Codex Plugin

Deploy, debug, monitor, and troubleshoot applications on [Render](https://render.com) from Codex. Start with the Render CLI for the core workflows. Add MCP later for advanced workflows.

## What it includes

- `skills/`: Render skills derived from [render-oss/skills](https://github.com/render-oss/skills)
- `scripts/validate-render-yaml.sh`: hook script that runs `render blueprints validate`
- `scripts/sync-skills.sh`: manual skill sync script
- `.mcp.json`: Render MCP server configuration using `RENDER_API_KEY`
- `.codex-plugin/plugin.json`: Codex plugin manifest
- `.github/workflows/sync-skills.yml`: daily skill sync workflow
- `assets/logo.svg`: plugin logo

## Reused from the Cursor plugin

This repository intentionally reuses content from the existing local Cursor Render plugin where the formats align cleanly:

- skills
- MCP configuration
- validation script
- logo asset

Cursor-specific concepts such as Cursor commands, rules, and agents are not included here because they do not map directly to Codex plugin packaging.

## Use it locally in Codex

This repository is the source of truth for the plugin. For local Codex use, install the plugin into the standard local plugin path at `~/plugins/render` and register it in `~/.agents/plugins/marketplace.json`.

To refresh the local plugin after you edit this repository, run:

```bash
rsync -a ./ ~/plugins/render/
```

## Get started with the Render CLI

The plugin is useful without MCP. Start with the Render CLI for Blueprint generation, `render.yaml` validation, workflow setup, logs, deploy status, and CLI-guided troubleshooting.

1. Install the Render CLI:

```bash
brew install render
```

2. Authenticate the CLI:

```bash
render login
```

3. Verify the CLI is ready:

```bash
render whoami -o json
```

If `render whoami -o json` fails, do not assume the CLI is usable yet. Fix authentication first.

If you previously set `RENDER_API_KEY` in your shell profile, make sure it is a real key or unset it. A placeholder value can break CLI auth in misleading ways.

4. Start using the plugin from Codex.

Good first prompts:

- `Help me install and authenticate the Render CLI for Render.`
- `Validate my render.yaml for Render.`
- `Debug my Render deployment with the Render CLI.`

## Add MCP for advanced workflows

The bundled MCP server is optional. Add it if you want direct service creation, structured service enumeration, richer monitoring, or structured database queries.

The plugin does not provide an API key input in the Codex plugin UI. Codex reads the bearer token for the bundled MCP server from the environment that launches Codex.

To enable MCP:

1. Create a Render API key in the [Render Dashboard](https://dashboard.render.com/u/*/settings#api-keys).
2. Quit Codex if it is already open.
3. Launch Codex from Terminal with the key set:

```bash
export RENDER_API_KEY="rnd_..."
open -a Codex
```

4. Keep using that Terminal-launched Codex session. If you reopen Codex later from the Dock or Spotlight, it might not inherit `RENDER_API_KEY`.
5. If you want this to persist for future launches, add the export line to your shell profile such as `~/.zshrc`, then launch Codex from a new Terminal session.

Never save a placeholder like `your_render_api_key` as the value of `RENDER_API_KEY`. A bad value is worse than an unset variable because it can make auth failures look unrelated.

The bundled MCP server is configured in `.mcp.json` and reads `RENDER_API_KEY` through `bearer_token_env_var`.

## Keep skills up to date

Run the manual sync script to refresh `skills/` from [render-oss/skills](https://github.com/render-oss/skills):

```bash
./scripts/sync-skills.sh
```

GitHub Actions also runs `.github/workflows/sync-skills.yml` every day and opens a pull request when upstream skills change.

## Publish it

Push this repository to GitHub and publish it as the plugin source repository. The plugin payload lives at the repository root, not under a nested `plugins/` directory.

## License

MIT. See [LICENSE](LICENSE).
