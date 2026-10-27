# DEALITHIC MCP servers

Connect Claude, ChatGPT, Cursor, Cline or any other MCP client to [DEALITHIC](https://www.dealithic.co): investor matching for private raises, and institutional deal intelligence.

This repository contains documentation and configuration only. The servers are hosted by DEALITHIC; there is nothing to build or run.

## 1. DEALITHIC Investor Intro (free, no API key)

**Endpoint:** `https://mcp.dealithic.co/public/mcp` (Streamable HTTP, no authentication)

Describe a raise and see how much of DEALITHIC's network of 93,000+ funds, family offices and angels fits it. Results are counts and breakdowns only. No firm names or contacts are returned. Names are released only after you request an introduction and book a demo.

| Tool | What it does |
|---|---|
| `investor_intro_preview` | Read-only. Scores a raise (asset class, size in USD millions, industry, optional geography and EBITDA) and returns how many investors fit, by tier and investor type. |
| `request_intro` | Records a request to be introduced to DEALITHIC for a demo. Only used with the person's explicit consent. |
| `lp_match_preview` | Read-only. For fund managers raising a fund: LP counts in the network by type (pension, endowment, foundation, sovereign wealth, fund of funds, family office). Counts by type only; not yet scored against your strategy or fund size. |
| `request_lp_intro` | Records a fund manager's request for an LP introduction and demo. Explicit consent required. |

More: https://mcp.dealithic.co/investor-intro

## 2. DEALITHIC Deal Intelligence (paid, API key)

**Endpoint:** `https://mcp.dealithic.co/mcp` (Streamable HTTP, header `X-Api-Key: agai_...`)

Get a key at https://mcp.dealithic.co/get-key (card on file, pay per call).

| Tool | Price per call | What it does |
|---|---|---|
| `sec_extract` | $0.25 | Financial statements and derived metrics for a US-listed ticker, from SEC EDGAR XBRL filings. |
| `validate_financials` | $0.50 | Validation pass over company financials (statement consistency, derived-metric sanity checks). |
| `deal_score` | $2.00 | Underwriting chain on a deal profile: financial integrity, trend, industry position, stress tests, conviction score, risk register. |
| `company_report` | $25.00 | Eight-lens institutional analysis of a US-listed company, grounded in SEC-reported data. |

This endpoint also includes the free `investor_intro_preview` and `request_intro` tools.

## Setup

### Claude (claude.ai and Claude Desktop)
Settings → Connectors → **Add custom connector**, then paste the endpoint URL. For the free server use `https://mcp.dealithic.co/public/mcp`.

Claude Code:
```bash
claude mcp add dealithic-intro --transport http https://mcp.dealithic.co/public/mcp
claude mcp add dealithic --transport http https://mcp.dealithic.co/mcp --header "X-Api-Key: agai_..."
```

### Cursor
Add to `~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project):
```json
{
  "mcpServers": {
    "dealithic-intro": { "url": "https://mcp.dealithic.co/public/mcp" },
    "dealithic": { "url": "https://mcp.dealithic.co/mcp", "headers": { "X-Api-Key": "agai_..." } }
  }
}
```

### ChatGPT
Settings → Apps & Connectors → Advanced → enable **Developer mode**, then create a connector with the URL `https://mcp.dealithic.co/public/mcp` and no authentication. (A ChatGPT directory listing is in progress.)

### Cline
MCP Servers → **Remote Servers**, then add `https://mcp.dealithic.co/public/mcp` with the name `dealithic-intro`. Or put it in `cline_mcp_settings.json`:
```json
{
  "mcpServers": {
    "dealithic-intro": { "type": "streamableHttp", "url": "https://mcp.dealithic.co/public/mcp" }
  }
}
```

## Example prompts
- "I'm raising $15M of growth equity for a B2B SaaS company in Texas. How many investors fit?"
- "Which kinds of investors would look at a $40M private credit deal for a manufacturer with $8M EBITDA?"
- "I'm raising a $250M lower-middle-market buyout fund. What LPs are in DEALITHIC's network?"
- "Pull SEC financials for TSLA and validate them." *(paid server)*

## Privacy and terms
- Privacy: https://www.dealithic.co/privacy
- Terms: https://www.dealithic.co/terms
- The intro tools collect name, email, company and a deal summary only when you explicitly agree to be contacted.

## About DEALITHIC
DEALITHIC is a capital markets platform for private issuers, with STEADSPARK, RYRA and William live and a network of 93,000+ funds, family offices and angels. Demo: https://www.dealithic.co/demo · Contact: info@dealithic.co

© 2026 DEALITHIC, Inc. All rights reserved. See [LICENSE](LICENSE).
