# Yelp for Business API integration and MCP server: connect Yelp for Business to ChatGPT, Claude and any AI agent

**Yelp for Business API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your Yelp for Business account.** Yelp's official connector in Claude is for customers who find businesses, request quotes and check restaurant availability, not for owners managing their account. Cotlet.dev connects your own signed-in Yelp for Business account to any AI agent, so the AI replies to leads and answers reviews for you.

## What you can ask

> Show new Yelp quote requests and draft a reply to each. Send after my OK.

> List this week's Yelp reviews with no owner reply and draft a calm public reply to each.

> What did my Yelp ads cost this month, and how many leads and calls did they bring?

> Which Yelp leads from the last 7 days still have no reply?

> How many of my reviews are not recommended, and which ones are new this month?

## What business owners ask their AI to do in Yelp for Business

From business owners' posts on Reddit, Facebook groups, review sites and the Yelp for Business help center (checked 25 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Reply to quote requests fast | "Show new Yelp Request a Quote messages and draft a reply to each within minutes. Send after my OK." | When a lead comes in |
| Answer reviews | "List reviews from this week with no owner reply and draft a calm public reply to each." | Weekly |
| Watch not recommended reviews | "Count my reviews that are not recommended this month and list the new ones." | Monthly |
| Ad spend against leads | "What did my Yelp ads cost this month, and how many leads, calls and clicks did they bring?" | Monthly |
| Unanswered leads | "Which leads from the last 7 days still have no reply? List them oldest first." | Daily |
| Holiday hours | "Set special hours for the holidays on my Yelp page after my OK." | Before holidays |

Your AI asks you before it sends, buys, replies or changes anything. Payments stay in your hands.

## Yelp for Business API

Yelp for Business has a Leads API for approved partners only and Yelp Leads on Zapier, but no owner API for reviews or page stats (checked 25 Nov 2026). With Cotlet.dev you build your own Yelp for Business API: show your AI agent a job once in your signed-in Yelp for Business account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/yelp-for-business

Endpoints you can build in a few minutes (none of them exists until you build it):

- `unreplied-leads`: Time, customer, job and ZIP code of each lead
- `reviews-without-reply`: Stars, text and date of each review you have not answered
- `page-stats`: Views, calls, website clicks and directions for the period
- `business-hours`: Your hours and special hours as Yelp shows them
- `ad-spend`: Ad cost, leads, calls and clicks for the period you name
- `mark-lead-replied`: A lead you handled by phone, marked as replied

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/unreplied-leads`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

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

This repo is a real MCP server. It speaks MCP over stdio, lists its Yelp for Business tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"yelp-for-business":{"command":"uvx","args":["yelp-for-business-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t yelp-for-business-mcp .
docker run -i --rm -e COTLET_API_KEY yelp-for-business-mcp
```

Tools: `yelp_for_business_guide`, `yelp_for_business_status`, `yelp_for_business_open`, `yelp_for_business_read`, `yelp_for_business_click`, `yelp_for_business_type`, `yelp_for_business_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install yelp-for-business-mcp        # or run it once: uvx yelp-for-business-mcp connect
yelp-for-business-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 yelp_for_business_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 yelp_for_business_agent.py status    # is Yelp for Business ready in your Cotlet.dev account
python3 yelp_for_business_agent.py guide     # Yelp for Business screens and sign-in link
python3 yelp_for_business_agent.py read      # open Yelp for Business, read the page, close the browser
python3 yelp_for_business_agent.py tools     # list Cotlet.dev tools
python3 yelp_for_business_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Yelp for Business users

**Does Yelp for Business have an API I can use with AI?**

Yelp's Leads API is for approved partners whose software holds their clients' leads, not for an owner's own account (checked 25 Nov 2026). Cotlet.dev works in your own signed-in Yelp for Business account, so your AI agent can reply to leads and answer reviews.

**How do I connect Yelp for Business to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to Yelp for Business once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no Yelp for Business API key.

**Is there a way to use Claude with Yelp for Business?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own Yelp for Business account.

**Is there a Yelp MCP server for business owners?**

Claude lists an official Yelp connector, but it is built for customers: finding businesses, requesting quotes and checking restaurant availability, not managing an owner's account. Cotlet.dev is a hosted MCP server that works in Yelp for Business for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**Can AI auto-reply to Yelp Request a Quote leads?**

Yelp's Leads API is for partner tools, and Yelp Leads on Zapier passes new leads to other apps. With Cotlet.dev your AI reads each lead in your own Yelp for Business account, drafts a reply that fits the job and sends it after your OK.

**Can AI tell me if Yelp ads pay off?**

Your AI reads your ad spend, leads, calls and page stats in your account and puts them side by side for the month.

## Find it on

- [Yelp for Business on Cotlet.dev](https://cotlet.dev/integrations/mcp/yelp-for-business?utm_source=github&utm_medium=readme&utm_campaign=yelp-for-business&utm_content=find)
- [PyPI package yelp-for-business-mcp](https://pypi.org/project/yelp-for-business-mcp/)
- [Odel](https://odel.app/modules/cotlet/yelp-for-business)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/yelp-for-business/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-yelp-for-business-to-ai-agents-guide)

## Also works with

- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
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
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
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
- [hipages MCP](https://github.com/cotlet-connect/hipages-mcp)
- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/yelp-for-business?utm_source=github&utm_medium=readme&utm_campaign=yelp-for-business

Start free: https://cotlet.dev/signup?app=yelp-for-business&utm_source=github&utm_medium=readme&utm_campaign=yelp-for-business
