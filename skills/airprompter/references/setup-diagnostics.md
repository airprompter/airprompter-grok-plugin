# Setup Diagnostics

Use this reference when AirPrompter tools are missing, or when the user asks
to diagnose, repair, reconnect, or reinstall AirPrompter in Grok Build.

## Readiness Layers

Treat the plugin, its enabled and trusted state, the MCP server definition,
the OAuth session, and the live tool inventory as separate states. A
Skill-only install cannot call AirPrompter.

1. Run `grok plugin list` and confirm `airprompter` is installed and
   enabled. Plugins stay off until enabled; turn it on with
   `grok plugin enable airprompter`.
2. Confirm the plugin is trusted. Grok Build keeps an untrusted plugin's MCP
   servers inactive (the Plugins panel shows them as `blocked`). Reinstall
   with `--trust` to trust it.
3. Run `grok plugin details airprompter` or `grok inspect` and confirm the
   bundled MCP server `airprompter` points at `https://mcp.airprompter.com/mcp`.
4. Open `/mcps`, select the AirPrompter server, and press `i` to authenticate
   if it has not signed in yet.
5. Call `search_tool` for AirPrompter, then `use_tool` with
   `airprompter__diagnose_connection` before executing a saved
   workflow. Grok Build exposes MCP tools only through those two tools, and
   tool discovery alone does not prove authentication.
6. Start a new Grok Build session after installing or reconnecting so the
   session loads the AirPrompter tool inventory.

## Customer Installation

Install AirPrompter from the Grok Build marketplace: type `/marketplace`,
select AirPrompter, and press `i`. Or run:

```bash
grok plugin install airprompter --trust
```

If AirPrompter is not listed, add xAI's catalog first, then install by
name:

```bash
grok plugin marketplace add xai-org/plugin-marketplace
```

Without any marketplace, install straight from the plugin repository:

```bash
grok plugin install airprompter/airprompter-grok-plugin --trust
```

Then authenticate from `/mcps` and sign in to AirPrompter in the browser.
Customers must not paste tokens or add or edit an AirPrompter MCP server
entry; the plugin's `.mcp.json` is the only one they need.

## Reconnect

If tools return Unauthorized after a completed sign-in, open `/mcps`,
authenticate AirPrompter again with `i`, and start a new session. Grok Build
stores MCP OAuth tokens in its own credential file; never print, copy, or
request its contents. Package reinstall and MCP authentication are
independent.

## Large Results

Grok Build truncates a tool result over 20,000 bytes and saves the full
result under the session `mcp/` folder. If a long workflow or prompt read
looks cut off, read the saved file. A user who wants larger inline results
can raise the cap in `~/.grok/config.toml`:

```toml
[mcp]
max_output_bytes = 60000
```

## Servers From Other Hosts

Grok Build also loads MCP servers configured for Claude Code and Cursor and
merges same-name servers by priority, so the AirPrompter server it uses may
come from another host's configuration. Run `grok inspect` and check the
AirPrompter server's origin (`[claude]` or `[cursor]`) and URL. If it is not
the plugin's server, disable the other entry from `/mcps` with `Space`.


If the person only asked to run a workflow and did not ask for setup repair,
stop after reporting what is missing and how to fix it; do not produce a
substitute result.
