# ShipItFam for Claude

Run your [ShipItFam](https://shipitfam.com) AI dev crew from Claude. ShipItFam is a human-in-the-loop AI dev team: you describe a goal, a crew of specialist agents plans and builds it, and nothing risky or final happens without your say-so. This plugin lets Claude do the whole job of running it with you: queue missions, show what the crew is waiting on, carry your approvals and answers back, follow up on finished work, steer a mission that is running, change how much the crew asks you, and open previews.

It works in Claude Code, claude.ai and Cowork.

## What you get

- **The ShipItFam MCP server** at `https://shipitfam.com/mcp`: 57 tools for the mission loop, projects and starters, routines, the Treasure map (project knowledge), your team and the crew's tools.
- **A skill** (`shipitfam`) that teaches Claude how to drive it well: read the board, put what the crew needs in plain language, ask before it approves, cancels or ships, and never decide for you.
- **Five commands**:

| Command | What it does |
|---|---|
| `/shipitfam:inbox` | Walks you through every request the crew is waiting on, one at a time: plans, questions, risky commands, step reviews, "Ship it?" and failures. |
| `/shipitfam:status` | Where your projects stand: what needs you, what is running, what is queued, what finished, and why a queue is stuck. Read-only. |
| `/shipitfam:new-mission` | Queues a mission from a plain-language goal. |
| `/shipitfam:follow-up` | Asks for changes on finished work, steers a running step, or leaves the crew a note. |
| `/shipitfam:settings` | Shows and changes how much the crew asks you: plan approval, risky commands, pauses, keep working. |

### The loop

```
inbox, request_get, request_answer   what the crew needs from you, read in full, and your answer
mission_create                       queues a mission (oldest first, one at a time per crew)
mission_list, mission_get, job_log   watch it (step_diff shows the code a step changed)
mission_comment                      follow up on finished work, steer a running step, leave a note
project_settings_set                 plan approval, risky commands, pauses, keep working
preview_pick                         which mission's preview the crew serves
```

Claude shows you a plan, a risky command or a "Ship it?" and waits for your yes before it approves, allows or ships. Skipping a step and cancelling a mission need your yes as well. If you tell it up front ("approve the plan for the pricing mission"), that is your yes.

## Install

You need a ShipItFam account. Create one at [shipitfam.com](https://shipitfam.com). You do not need a project yet: once Claude is connected, ask it to create one ("Create a ShipItFam project from the blank starter called Landing"). It lists the starters with `project_starter_list` and creates the project with `project_create`. A new project runs missions when it is created from a starter (a project you already have may run them too: `core_v2` in `project_list` says).

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

- "What needs me on ShipItFam?" or `/shipitfam:inbox`
- "Queue a mission on my landing page project: add a pricing section with three tiers."
- "Show me the plan for the pricing mission. If it looks right, approve it."
- "How is the pricing mission going? What is the crew doing right now?"
- "The pricing mission is done. Follow up: make the middle tier the highlighted one."
- "Stop pausing me on every plan for the landing page project, but keep asking before risky commands."
- "Show me the preview of the pricing mission."
- "What can I start a ShipItFam project from?" then "Create one called Landing from the blank starter."

The crew runs on your own Claude login, which you set up in the ShipItFam app. If a step fails with "Claude is not logged in on this project", Claude offers to reuse a login you already have working on another project, or sends you to the app to sign in.

## What Claude is allowed to do

The server exposes 57 tools, and every one declares `title`, `readOnlyHint`, `destructiveHint` and `openWorldHint`:

| Class | Tools | Hints |
|---|---|---|
| Read-only | 21 (every list and get, `inbox`, `request_get`, `mission_list`, `mission_get`, `job_log`, `step_diff`, `project_starter_list`, `routine_run_list`, status and diagnostics) | `readOnlyHint` true |
| Write | 26 (creates and edits, for example `project_create`, `mission_create`, `mission_comment`, `preview_pick`, `project_settings_set`, `project_wake`) | `readOnlyHint` false, `destructiveHint` false |
| Destructive | 10 (every `*_delete`, `mission_cancel`, `project_repo_credential_clear`, `project_invite_revoke`, and `request_answer`) | `destructiveHint` true |

`openWorldHint` is true on exactly two tools, because the call itself reaches outside your ShipItFam account: `mcp_server_verify` (calls the URL you registered for an MCP server) and `request_answer` (on a "Ship it?" request it pushes the mission branch to your git remote). It is false on the other 55. `request_answer` is destructive because it is how a request is answered: approving a plan, allowing a command, cancelling a mission and shipping all go through it.

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
  commands/                         the five slash commands
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
