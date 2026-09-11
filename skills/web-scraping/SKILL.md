---
description: Use when the user asks to scrape a webpage, pull structured data (prices, listings, contacts, specs) from a URL, run an AI-powered web search, or crawl a multi-page site. Routes the request to the ScrapeGraphAI MCP tools instead of guessing content or fetching raw HTML by hand.
---

# Web scraping with ScrapeGraphAI

This plugin connects to the official ScrapeGraphAI MCP server, which turns a URL (or a search query) directly into clean markdown or structured JSON using an LLM under the hood. Prefer these tools over manually fetching and parsing HTML whenever the request is about live web content.

## Picking the right tool

- **One page, just need readable content** → `scrape` (returns clean markdown).
- **One or a few pages, need specific fields** (price, name, email, spec sheet, etc.) → `extract` with a JSON schema describing exactly what to pull out. Use `schema` first if you want the server to draft the schema from a natural-language description.
- **Don't have a URL yet, need to find and pull info from the live web** → `search` (AI web search that returns extracted, relevant results, not raw SERP links).
- **Need every page on a site, or content behind pagination/navigation** → `crawl_start`, then poll `crawl_get_status`; use `crawl_stop` / `crawl_resume` to manage long-running crawls.
- **Recurring/scheduled scraping** (e.g. "let me know when this price changes") → `monitor_create`, `monitor_list`, `monitor_get`, `monitor_pause`, `monitor_resume`, `monitor_delete`, `monitor_activity`.
- **Housekeeping** → `credits` (check remaining usage) and `history` (past requests).

## Notes

- Every call spends ScrapeGraphAI API credits. Before a large `extract`/`crawl` job, check `credits` and mention the likely cost to the user if it's non-trivial.
- For `extract`, write the tightest schema that answers the actual question — fewer fields means a cheaper, faster, more reliable extraction.
- If a tool call fails with an auth error, the `SGAI_API_KEY` environment variable is likely missing or invalid — tell the user to check it rather than retrying blindly (see this plugin's README).
