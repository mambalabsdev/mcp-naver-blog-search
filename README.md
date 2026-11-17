# Naver Blog Search MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-naver-blog-search)](https://www.npmjs.com/package/@mambalabsdev/mcp-naver-blog-search) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-naver-blog-search)](https://github.com/mambalabsdev/mcp-naver-blog-search/blob/main/LICENSE)

An MCP server for the Mamba Labs Naver Blog Search actor on Apify. Korean blog posts from Naver search, as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes keywords and returns the Naver blog search results for each, one row per unique post with its URL, title, snippet, blog, and publish date, plus a free summary row per keyword.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-naver-blog-search": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-naver-blog-search"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `search_naver_blogs` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find the newest Naver blog posts about 캠핑 from the last week."
- "What are Korean bloggers writing about Tesla this month? Give me 60 posts."

## Tool

`search_naver_blogs`. One tool. Example arguments:

```json
{
  "keywords": [
    "캠핑"
  ],
  "sort": "newest",
  "period": "1w",
  "max_posts_per_keyword": 30
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `keywords` | array | no | One keyword per item, in Korean or any language, for example 캠핑 or tesla. Up to 200 keywords per run. As text, put one keyword per line. |
| `sort` | string | no | relevance (default): Naver's own order, about 70 to 90 unique posts per keyword before Naver repeats them. newest: newest posts first, up to about 900 per keyword. |
| `period` | string | no | Only posts from this period: all (default), 1d, 1w, 1m, 6m, or 1y. date_from or date_to wins over this. |
| `date_from` | string | no | Optional. Only posts on or after this date, as 2026-01-31. |
| `date_to` | string | no | Optional. Only posts on or before this date, as 2026-01-31. |
| `max_posts_per_keyword` | number | no | Unique posts returned per keyword. Default 100, maximum 900, which is about as deep as Naver's results go. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `post-row`, one unique blog post | $0.0008 | $0.00076 | $0.00072 | $0.00068 |
| `apify-actor-start`, per run start | $0.006 | $0.006 | $0.006 | $0.006 |

Repeats, sponsored results, keyword summaries, keywords with no post, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/naver-blog-search](https://apify.com/mambalabs/naver-blog-search)

## License

MIT. See [LICENSE](LICENSE).
