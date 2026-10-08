# ShipItFam for Claude

Drive your [ShipItFam](https://shipitfam.com) AI dev crew from Claude. ShipItFam is a human-in-the-loop AI dev team: you describe a goal, a crew of specialist agents plans and builds it, and nothing risky or final happens without your say-so. This plugin lets Claude list your projects, create new ones from a starter, start missions, show you what the crew is waiting on, carry your answers back, open previews and ship.

It works in Claude Code, claude.ai and Cowork.

## What you get

- **The ShipItFam MCP server** at `https://shipitfam.com/mcp` (projects and project starters, missions, requests, previews, routines, the Treasure map, and more).
- **A skill** (`shipitfam`) that teaches Claude how to drive ShipItFam well: read the board, relay what the crew needs in plain language, and never decide for you.
- **Four commands**:

| Command | What it does |
|---|---|
| `/shipitfam:status` | Where your projects stand: what needs you, what is running, what finished. Read-only. |
| `/shipitfam:inbox` | Walks you through every request the crew is waiting on, one at a time. |
| `/shipitfam:new-mission` | Starts a mission from a plain-language goal. |
| `/shipitfam:ship` | Reviews a finished mission (preview, PR) and ships it, sends it back, or holds it. |

## Install

You need a ShipItFam account. Create one at [shipitfam.com](https://shipitfam.com). You do not need a project yet: once Claude is connected, ask it to create one ("Create a ShipItFam project from the blank starter called Landing"). It lists the starters with `project_starter_list` and creates the project with `project_create`. Missions run on projects created from a starter.

### Claude Code

```
/plugin marketplace add yanko-ivanov/shipitfam-claude-plugin
/plugin install shipitfam@shipitfam
```

Then run `/mcp`, pick `shipitfam` and choose to authenticate. A browser tab opens: log in to ShipItFam, click Allow, and you are connected.

### claude.ai and Cowork

Open Customize, then Plugins, add the marketplace `yanko-ivanov/shipitfam-claude-plugin`, install **ShipItFam**, and click Connect when asked. You log in to ShipItFam and click Allow.

### Just the connector, no plugin

Add `https://shipitfam.com/mcp` as a custom connector (claude.ai: Settings, Connectors, Add custom connector; Claude Code: `claude mcp add --transport http shipitfam https://shipitfam.com/mcp`). You get the tools without the skill and commands.

## How sign-in works

ShipItFam is an OAuth server. There are no tokens to copy and nothing to paste into a config file.

1. Claude registers itself with ShipItFam (dynamic client registration).
2. A browser opens on ShipItFam's consent page. You log in and click Allow (PKCE, no client secret).
3. Claude receives an access token that lasts 24 hours and renews itself with a refresh token.

Every connected app shows up in ShipItFam under **MCP access**. Revoke one there and its refresh stops working.

Do not add an `Authorization` header to this plugin's MCP config. A static header switches OAuth off.

## Using it

Try:

- "What can I start a ShipItFam project from?"
- "Create a ShipItFam project called Landing from the blank starter."
- "What needs me on ShipItFam?"
- "Start a mission on my landing page project: add a pricing section with three tiers."
- "Show me the preview for the pricing mission and ship it if it looks right."

The crew runs on your own Claude login, which you set up in the ShipItFam app. If a step fails with "Claude is not logged in on this project", sign in to Claude in the app and retry from Claude.

## What Claude is allowed to do

The server exposes 78 tools, and every one declares `title`, `readOnlyHint`, `destructiveHint` and `openWorldHint`:

| Class | Tools | Hints |
|---|---|---|
| Read-only | 27 (every list and get, `helm_feed`, `request_list`, `mission_list`, `mission_get`, `project_starter_list`, previews, status and diagnostics) | `readOnlyHint` true |
| Write | 37 (creates and edits, for example `project_create`, `mission_create`, `project_update`, `project_settings_set`) | `readOnlyHint` false, `destructiveHint` false |
| Destructive | 14 (every `*_delete`, `session_stop`, `session_cleanup`, `session_force_resume`, `approval_reject`, `project_repo_credential_clear`, `project_invite_revoke`, and `action`) | `destructiveHint` true |

`openWorldHint` is true on exactly two tools, because the call itself reaches outside your ShipItFam account: `mcp_server_verify` (calls the URL you registered for an MCP server) and `action` (on a "Ship it?" card it pushes the mission branch to your git remote). It is false on the other 76. `action` is destructive because it is how a mission card is answered, cancelled or shipped.

A connected app cannot change your billing or manage your Claude logins: those stay behind your own ShipItFam session, so Claude sends you to the app for them.

## Develop

```
claude plugin validate . --strict
claude plugin validate plugins/shipitfam --strict
```

Layout:

```
.claude-plugin/marketplace.json     the marketplace (name: shipitfam)
plugins/shipitfam/
  .claude-plugin/plugin.json        plugin manifest
  .mcp.json                         the remote MCP server, no static headers
  skills/shipitfam/SKILL.md         how to drive ShipItFam
  commands/                         the four slash commands
  assets/logo.png                   plugin icon
```

The ChatGPT and Codex version of this plugin lives in [shipitfam-chatgpt-app](https://github.com/yanko-ivanov/shipitfam-chatgpt-app).

## Links

- Website: https://shipitfam.com
- Privacy: https://shipitfam.com/privacy.html
- Terms: https://shipitfam.com/terms.html
- Support: https://shipitfam.com/contact.html

## License

MIT, see [LICENSE](LICENSE).
