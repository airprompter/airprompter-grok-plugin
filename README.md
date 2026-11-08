# AirPrompter for Grok Build

Run, create, and improve your saved AirPrompter prompts and workflows from Grok Build. Sign in with your AirPrompter account on first connect.

## Install

In Grok Build, type `/marketplace`, select AirPrompter, and press `i`.
Or run:

```bash
grok plugin install airprompter --trust
```

If AirPrompter is not listed, run
`grok plugin marketplace add xai-org/plugin-marketplace` and install by
name, or install straight from this repository:

```bash
grok plugin install airprompter/airprompter-grok-plugin --trust
```

## Sign in

Open `/mcps`, select AirPrompter, and press `i`. Sign in to AirPrompter in
the browser window that opens, then start a new session and ask Grok to
list your AirPrompter workflows. Grok Build stores the sign-in, not this
plugin.

## What this plugin contains

- `.grok-plugin/plugin.json` -- plugin metadata.
- `skills/airprompter/` -- routing rules for saved prompts and workflows.
- `.mcp.json` -- names the AirPrompter MCP server.
- `assets/logo.png` -- marketplace logo.

## Network endpoints and credentials

- The hosted AirPrompter MCP server is at
  `https://mcp.airprompter.com/mcp`, which serves your saved prompts and workflows.
- It needs your AirPrompter account. Grok Build runs the browser sign-in
  and keeps the resulting token in its own credential store.
- No credential, client id, scope, or customer identifier is packaged here.
- The plugin registers no hooks, commands, or agents. The shared skill bundles
  optional install and verification scripts. The production skill uses MCP for runs.
  In development, the install helper uses a single-use AirPrompter install token
  with `https://api-dev.airprompter.com` and downloads files from
  `https://d3e65ddf0mnnkp.cloudfront.net`. It verifies files before saving them
  and does not execute downloaded files.

## Large results

Grok Build truncates tool results over 20,000 bytes inline and saves the
full result in the session folder. Long workflows can exceed that; raise
`[mcp] max_output_bytes` in `~/.grok/config.toml` for larger inline results.

## License

MIT. See `LICENSE`.
