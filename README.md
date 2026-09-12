# sills

A Claude Code / Claude Cowork plugin that gives Claude live web-scraping tools in chat, backed by the official [ScrapeGraphAI](https://scrapegraphai.com) MCP server. Once installed, you can ask Claude to scrape a page, pull structured data from a URL, search the live web, or crawl a whole site — in this session and every future one.

It does **not** run any scraping code itself. It's a thin, versioned plugin manifest that points Claude at ScrapeGraphAI's hosted MCP endpoint (`https://sgai-mcp-main.onrender.com`) using your own API key.

## 1. Get an API key

Sign up at [scrapegraphai.com](https://scrapegraphai.com), grab your API key from the dashboard, and set it as an environment variable wherever Claude Code / Cowork runs:

```bash
export SGAI_API_KEY="your-key-here"
```

Never commit your real key to this repo — `.mcp.json` only ever references `${SGAI_API_KEY}`, it does not contain a key itself.

## 2. Install the plugin

From a Claude Code / Cowork session:

```
/plugin marketplace add onlyfortimepass0000-tech/sills
/plugin install scrapegraphai@sills
```

Or test it locally without installing, from a clone of this repo:

```bash
claude --plugin-dir .
```

## 3. Use it

Just ask, e.g.:
- "Scrape https://example.com and summarize it"
- "Pull the price and title of every product on this listing page"
- "Search the web for the latest funding round of [company]"
- "Crawl this docs site and pull all the API endpoint names"

Claude routes these to the right ScrapeGraphAI tool automatically (see `skills/web-scraping/SKILL.md`).

## What's included

| File | Purpose |
|---|---|
| `.claude-plugin/plugin.json` | Plugin manifest (name, version, description) |
| `.claude-plugin/marketplace.json` | Lets this repo be added directly as a plugin marketplace |
| `.mcp.json` | Wires up the ScrapeGraphAI MCP server, reading the API key from `SGAI_API_KEY` |
| `skills/web-scraping/SKILL.md` | Teaches Claude which ScrapeGraphAI tool to use for a given request |

## Tools exposed

`scrape`, `extract`, `search`, `crawl_start`, `crawl_get_status`, `crawl_stop`, `crawl_resume`, `schema`, `credits`, `history`, `monitor_create`, `monitor_list`, `monitor_get`, `monitor_pause`, `monitor_resume`, `monitor_delete`, `monitor_activity`.

Every call spends ScrapeGraphAI API credits — check usage anytime by asking Claude to check your `credits`.
