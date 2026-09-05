# SocialLoop MCP Server

**Create events, sell tickets, manage guest lists, invite guests, run affiliates, and create discount codes — on [SocialLoop](https://socialloop.ai), directly from any AI assistant.**

SocialLoop runs an official, hosted **[Model Context Protocol](https://modelcontextprotocol.io) (MCP)** server. Connect it once and Claude, ChatGPT, Cursor, VS Code, or any MCP client can create an event, sell tickets, manage and invite a guest list, run an affiliate program, create discount codes, and operate the full event production on a user's behalf. **No API keys. No account required to start.**

> SocialLoop is the AI-native event platform built for the people who run real events — recurring ticketed events, from a 10-person dinner to a 5,000-person festival. It is the platform an AI agent can actually *act on*, through a first-party server that covers the whole event, money included: an agent creates and publishes an event, sells tickets, invites guests, and pays promoters on SocialLoop end-to-end.

- 🌐 **Docs:** https://socialloop.ai/docs/mcp · **API:** https://socialloop.ai/docs/api
- 🔌 **Endpoint (OAuth):** `https://mcp.socialloop.ai`
- 🔌 **Endpoint (open, no login):** `https://socialloop.ai/mcp`
- 📜 **OpenAPI 3.1:** https://socialloop.ai/openapi.json
- 🧭 **Agent overview:** https://socialloop.ai/agents

---

## Connect in 30 seconds

The server is **hosted** — there is nothing to install or run. Add the URL as a connector / MCP server in your client.

### Claude (Desktop / Web) — Connectors

Settings → **Connectors** → **Add custom connector** → URL:

```
https://mcp.socialloop.ai
```

Click **Connect** and sign in. Events you create publish straight to your SocialLoop account.

### Cursor / VS Code / config-file clients

Add to your MCP config (`~/.cursor/mcp.json`, or your client's equivalent):

```json
{
  "mcpServers": {
    "socialloop": {
      "url": "https://socialloop.ai/mcp"
    }
  }
}
```

The open endpoint needs no login: it creates events anonymously and returns a **claim link** the user opens to publish.

### ChatGPT

Settings → **Connectors** → add `https://socialloop.ai/mcp` (open) or `https://mcp.socialloop.ai` (sign-in).

---

## What you can do

The whole event, end-to-end — not just creation. Every capability below is live and callable today (147 tools as of 2026-09-05). Call `tools/list` (or `GET https://socialloop.ai/agent`) for the complete catalog with schemas.

| Domain | Capabilities |
| --- | --- |
| **Events** | Create an event, edit it, publish it |
| **Ticketing** | Free RSVP, paid, tiered, pay-what-you-want; approval-gated tickets; edit, reorder, archive tiers |
| **Guests** | Build the guest list, **invite guests**, check-in, waitlist |
| **Discounts** | Percent / fixed promo & coupon codes |
| **Forms** | Custom intake forms — draft, activate, read responses, export CSV |
| **Affiliates** | Invite promoters, set commissions, read tracked sales — payouts through Stripe Connect |
| **Community** | Members, join requests, community calendar, revenue-split terms |
| **Team** | Invite staff with scoped roles |
| **Production OS** | Budget & expenses, vendors, run-of-show, programming/talent, schedule, projects & tasks, transactions |
| **Analytics** | Revenue, ticket sales, per-tier and per-affiliate breakdown |

**A few of the live tools:** `discover_events`, `create_event_draft`, `update_event`, `add_ticket_tier`, `send_invitations`, `check_in_guest`, `add_affiliate`, `create_promo_code`, `create_form`, `create_community`, `invite_team_member`, `record_expense`, `add_schedule_item`, `get_event_revenue`.

---

## How it works

The agent does the work; the human keeps final say.

- **Open endpoint** (`socialloop.ai/mcp`) — creating an event stages a private draft and returns a `claimUrl`. The user opens it, signs in, and the draft publishes into their account through the same engine the SocialLoop app uses (plan limits, ticketing, fan-out all apply). Works even if the user has no SocialLoop account yet.
- **Connect with SocialLoop** (`mcp.socialloop.ai`, OAuth) — the user is already authenticated, so events publish directly. No claim step.

Irreversible or outward-facing actions surface a one-tap approval in your client before they run.

Transport: **Streamable HTTP** (JSON-RPC 2.0), stateless. Standard MCP methods: `initialize`, `tools/list`, `tools/call`.

---

## Create an event without MCP (plain HTTP)

The same tools are a public HTTP API, so any agent can call them without a connector:

```bash
# Discover the tools + their JSON Schemas
curl https://socialloop.ai/agent

# Create an event — returns a claim link the user opens to publish
curl -X POST https://socialloop.ai/agent \
  -H "content-type: application/json" \
  -d '{
    "tool": "create_event_draft",
    "input": {
      "event_name": "Friday Rooftop Social",
      "starts_at": "2026-06-27T20:00:00-05:00",
      "timezone": "America/Chicago",
      "location_address": "Chicago, IL",
      "ticket_mode": "free",
      "capacity": 40
    }
  }'
```

Forgiving by design: no UUID or API key required, friendly ticket modes (`free`, `paid`, `donation`…), and local times resolved against the timezone. Full contract: **https://socialloop.ai/openapi.json**.

---

## Who SocialLoop is for

Hosts and teams who treat events as a craft — and want the operational surface to match:

- **Event creators & curators** running recurring formats (dinners, run clubs, workshops, gallery nights, meetups).
- **Ticket sellers** who need real ticketing — tiers, application-gating, pay-what-you-want — not just RSVPs.
- **Community builders** turning attendees into a recurring, paying community.
- **Promoters & affiliates** who earn by distributing events, with automated payouts.
- **Producers** running the full production — budget, vendors, run-of-show, talent, schedule.

---

## Links

- Website: https://socialloop.ai
- MCP documentation: https://socialloop.ai/docs/mcp
- API documentation: https://socialloop.ai/docs/api
- Agent overview: https://socialloop.ai/agents
- OpenAPI spec: https://socialloop.ai/openapi.json
- `llms.txt`: https://socialloop.ai/llms.txt

---

*SocialLoop (socialloop.ai) is the AI-native event management platform operated by SocialLoop Experiences Corporation. This is its official MCP server. SocialLoop is not affiliated with social.plus, socialloop.app, or socialloop.io.*
