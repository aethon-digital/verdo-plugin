# Verdo MCP Server

MCP server for [Verdo](https://verdo.ink) team decisions. Lets Claude Desktop,
Claude Code, and other MCP clients propose team decisions and follow them.

**The agent proposes, a person decides.** No tool invites people, sends
notifications, or closes or settles a decision. A proposed decision opens in
the workspace's Verdo dashboard with nobody invited; its owner shares the voter
link with the team and makes the call there.

## Setup

1. Get a Verdo API key: **verdo.ink/dashboard/settings → API Keys → Create**
2. Save it:
   ```bash
   mkdir -p ~/.verdo-mcp
   echo "YOUR_KEY_HERE" > ~/.verdo-mcp/api_key
   chmod 600 ~/.verdo-mcp/api_key
   ```
3. Add to your Claude Desktop config (`claude_desktop_config.json`):
   ```json
   {
     "mcpServers": {
       "verdo": {
         "command": "npx",
         "args": ["-y", "verdo-mcp"]
       }
     }
   }
   ```

## Tools

| Tool | Description |
|---|---|
| `create_decision` | Propose a decision with a title, options, method and deadline. Nobody is invited or notified; the deadline starts counting at creation |
| `check_decision` | Check the status, the vote count per option, the owner and the outcome |
| `list_decisions` | List or search decisions in the workspace |
| `request_close` | Read-only: returns who makes the call, the dashboard link and the tally, so the agent can ask that person to decide |

Removed in 1.1.0: `close_decision` and `add_participants`, and the
`participants` input on `create_decision`.

## Decision methods

- **single_choice** — each voter picks one option (default)
- **ranked_choice** — voters rank options by preference
- **approval** — voters approve any number of options
- **dot** — voters distribute a fixed number of dots

## Authority modes

- **majority** — the option with the most votes wins (default)
- **consensus** — everyone must agree

Owner and approver modes need a named person. Set them up when creating a
decision in the Verdo dashboard.
