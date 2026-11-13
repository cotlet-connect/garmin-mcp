# How to connect Garmin Connect to Claude Code, ChatGPT and any AI agent

**Yes, Claude, ChatGPT and other AI agents can work in your Garmin Connect account.** Garmin Connect data and workouts are available through Cotlet.dev in your signed-in account.

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

**Can I use the Garmin Connect API without an AI agent?**

Yes. Call Cotlet's REST API from a script or an HTTP step in n8n, Make or Zapier with your Cotlet key. MCP is the alternative for AI agents; you do not need to connect an agent to use REST.

**Is there an official ChatGPT or Claude app for Garmin Connect?**

Garmin has no app of its own in ChatGPT or Claude (checked 6 Nov 2026). ChatGPT lists third-party fitness apps, and on iPhone Garmin data can reach ChatGPT through Apple Health, read only. Cotlet works in your own Garmin Connect account, so the AI can also build workouts and put them on your Garmin calendar.

**Can ChatGPT create workouts in Garmin Connect?**

Yes, with Cotlet. ChatGPT and Claude build runs, rides and strength workouts, schedule them on your Garmin calendar and read your training, sleep and HRV.

**How do I get access?**

Create a free account at https://cotlet.dev, open Garmin Connect under Integrations in the console and sign in to Garmin Connect in its Cloud Browser. The trial starts when you sign up, no card asked.

**How do I connect Garmin Connect to ChatGPT or Claude?**

Sign up at cotlet.dev, log in to Garmin Connect once in its Cloud Browser and add Cotlet to ChatGPT or Claude, following the steps in your console. No API key is needed. ChatGPT and Claude then work in your Garmin Connect account through Cotlet, in their phone apps too.

**What can ChatGPT do in Garmin Connect?**

With Cotlet, ChatGPT, Claude and other AI agents handle jobs like these in Garmin Connect: morning readiness check; review last week\; build and schedule a training week. You ask in plain words, and anything that reaches other people waits for your OK first.

**Why not use Garmin's free Personal API key?**

For reading your own activity and health data, you can (checked 27 Oct 2026). The key is read-only, pull only and expires after 30 days to a year, and you need your own code or an open-source server to use it. Cotlet is for what the key does not do: building and scheduling workouts, and training status, in a hosted server.

**Does a scheduled workout reach my watch?**

Scheduling puts the workout on your Garmin Connect training calendar. Garmin's manuals say workouts sent from Connect to the device overwrite the training calendar already on it, so check the calendar before you confirm.

**Do I need Garmin Connect+?**

No for the tools here. Cotlet works in your account on the plan you have. Features Garmin sells with Connect+, such as nutrition tracking, need that subscription in Garmin first (checked 27 Oct 2026).

**Does Garmin Connect have webhooks?**

Only for approved businesses. Starter and Enterprise push data to an app's server, while the Personal tier is pull only (checked 27 Oct 2026). Run your agent on a schedule instead, for example a daily get_sleep and get_training_status pass.

**Does it work with any Garmin watch?**

Cotlet reads what Garmin Connect shows for your account. A device that does not record a metric, such as HRV status or training readiness, returns it empty.

**Can the agent see my Garmin password?**

No. We keep the signed-in session, never the password, and no tool returns the cookie.

**Is the MCP server different from the REST API?**

Only the way in. Both have the same 11 tools, inputs and answers, one API key and one price. Agents use MCP; scripts, cron jobs and HTTP steps use REST.

**Is Cotlet affiliated with Garmin?**

No. Cotlet is independent and not affiliated with Garmin Ltd.

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
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Boulevard MCP](https://github.com/cotlet-connect/boulevard-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [Meevo MCP](https://github.com/cotlet-connect/meevo-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [AestheticsPro MCP](https://github.com/cotlet-connect/aestheticspro-mcp)
- [Club OS MCP](https://github.com/cotlet-connect/club-os-mcp)
- [EZ Lawn Biller MCP](https://github.com/cotlet-connect/ez-lawn-biller-mcp)
- [Eversports Studio Manager MCP](https://github.com/cotlet-connect/eversports-studio-manager-mcp)
- [Exercise.com MCP](https://github.com/cotlet-connect/exercise-com-mcp)
- [Gymdesk MCP](https://github.com/cotlet-connect/gymdesk-mcp)
- [Iris Works MCP](https://github.com/cotlet-connect/iris-works-mcp)
- [CoachingPortal MCP](https://github.com/cotlet-connect/coachingportal-mcp)
- [CrewNest MCP](https://github.com/cotlet-connect/crewnest-mcp)
- [Paired Plus MCP](https://github.com/cotlet-connect/paired-plus-mcp)
- [ClubReady MCP](https://github.com/cotlet-connect/clubready-mcp)
- [Joist MCP](https://github.com/cotlet-connect/joist-mcp)
- [Trainerize MCP](https://github.com/cotlet-connect/trainerize-mcp)
- [Substack MCP](https://github.com/cotlet-connect/substack-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/garmin-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=garmin

Start free: https://cotlet.dev/signup?app=garmin&utm_source=github&utm_medium=readme&utm_campaign=garmin
