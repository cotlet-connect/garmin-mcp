# How to connect Garmin Connect to Claude Code, ChatGPT and any AI agent

**Yes, Claude, ChatGPT and other AI agents can work in your Garmin Connect account.** Garmin's own API is pull-only and made for developers. Cotlet.dev connects your own Garmin Connect account to any AI agent, so the AI can read your training and build and schedule workouts for you.

## What you can ask

> Build a 10 km tempo run workout and put it on my Garmin calendar for Thursday.

> How did my sleep and HRV change over the last 4 weeks?

> Plan my next 8 weeks for a half marathon and schedule the workouts.

> Compare my last three long runs: pace, heart rate, cadence.

> What's my VO2 max trend this year?

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Garmin Connect tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"garmin":{"command":"uvx","args":["garmin-cotlet-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t garmin-mcp .
docker run -i --rm -e COTLET_API_KEY garmin-mcp
```

Tools: `garmin_guide`, `garmin_status`, `garmin_open`, `garmin_read`, `garmin_click`, `garmin_type`, `garmin_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install garmin-cotlet-mcp        # or run it once: uvx garmin-cotlet-mcp connect
garmin-cotlet-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 garmin_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 garmin_agent.py status    # is Garmin Connect ready in your Cotlet.dev account
python3 garmin_agent.py guide     # Garmin Connect screens and sign-in link
python3 garmin_agent.py read      # open Garmin Connect, read the page, close the browser
python3 garmin_agent.py tools     # list Cotlet.dev tools
python3 garmin_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Garmin Connect users

**Does Garmin Connect have an API?**

Garmin has a personal, pull-only API and a business Health API for developers. Neither lets an AI agent create and schedule workouts for you. Cotlet.dev works in your own Garmin Connect account, so it can.

**How do I connect my Garmin to Claude AI?**

Add Cotlet.dev to Claude (the `claude mcp add` command above, or a custom connector in the Claude app), sign in to Garmin Connect once in your Cotlet.dev Cloud Browser, and ask Claude about your training.

**How do I connect Garmin Connect to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to Garmin Connect once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no Garmin Connect API key.

**Is there a Garmin Connect MCP server?**

Garmin Connect has no official MCP server. Cotlet.dev is a hosted MCP server that works in Garmin Connect for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

## Find it on

- [Garmin Connect on Cotlet.dev](https://cotlet.dev/integrations/garmin-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=garmin&utm_content=find)
- [PyPI package garmin-cotlet-mcp](https://pypi.org/project/garmin-cotlet-mcp/)
- [Glama](https://glama.ai/mcp/servers/cotlet-connect/garmin-mcp)
- [Odel](https://odel.app/modules/cotlet/garmin)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/garmin/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-garmin-to-ai-agents-guide)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/garmin-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=garmin

Start free: https://cotlet.dev/signup?app=garmin&utm_source=github&utm_medium=readme&utm_campaign=garmin
