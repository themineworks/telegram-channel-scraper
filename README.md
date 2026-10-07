# Telegram Channel Scraper: Public Posts, No Login

Scrape public Telegram channel posts over plain HTTP: text, view counts (raw label and parsed integer), outbound links, media flag, timestamp, and permalink. Walks the channel's full history using Telegram's own cursor pagination. No Bot API token, no phone number, no login. Pay per post delivered.

**Run it on Apify:** [apify.com/themineworks/telegram-channel-scraper](https://apify.com/themineworks/telegram-channel-scraper)
**Docs, FAQ and pricing:** [themineworks.com/actors/telegram-channel-scraper](https://themineworks.com/actors/telegram-channel-scraper/)

**Price:** From $1.00 per 1,000 posts on Apify's higher plans, plus a $0.005 start fee per run. Failed and empty results are never charged.

## What it returns

* Post text, view count (raw label and parsed integer), and outbound links
* Media presence flag, publish timestamp, and direct permalink per post
* Walks full channel history via Telegram's cursor-based pagination
* Channel-level metadata: display title and subscriber count
* No Bot API token, no phone number, no MTProto session, no login

## Quick start

You need a free [Apify account](https://console.apify.com/sign-up) and its API token (Settings, API & Integrations).

### Python

```bash
pip install apify-client
```

```python
from apify_client import ApifyClient

client = ApifyClient("YOUR_APIFY_TOKEN")
run = client.actor("themineworks/telegram-channel-scraper").call(run_input={
    "channels": [
        "durov",
        "telegram"
    ],
    "maxPostsPerChannel": 40
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item)
```

### Node.js

```bash
npm install apify-client
```

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_APIFY_TOKEN' });
const run = await client.actor('themineworks/telegram-channel-scraper').call({
    "channels": [
        "durov",
        "telegram"
    ],
    "maxPostsPerChannel": 40
});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items);
```

### cURL

One request that runs the actor and returns the results in the response (for runs under 5 minutes):

```bash
curl -X POST "https://api.apify.com/v2/acts/themineworks~telegram-channel-scraper/run-sync-get-dataset-items?token=YOUR_APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"channels": ["durov", "telegram"], "maxPostsPerChannel": 40}'
```

### Command line

This repo includes ready-made clients that save results to JSON and CSV:

```bash
python3 telegram_channel_scraper.py --token YOUR_APIFY_TOKEN --channels "durov,telegram" --max-posts-per-channel "40"
node telegram_channel_scraper.mjs --token YOUR_APIFY_TOKEN --channels "durov,telegram" --max-posts-per-channel "40"
```

## Input

| Field | Type | Default | Description |
|---|---|---|---|
| `channels` (required) | array |  | Public Telegram channels |
| `maxPostsPerChannel` | integer | `100` | Hard cap per channel |

## Output

One row per result, as JSON, CSV, Excel or through the API.

| Field | Type | Description |
|---|---|---|
| `channel` | string | Channel username |
| `channel_title` | string | Channel display title |
| `subscribers` | string | Subscriber count as Telegram displays it |
| `post_id` | string | Post number within the channel |
| `post_url` | string | Direct public link to the post |
| `text` | string | Post text with formatting stripped |
| `views_label` | string | View count as displayed, for example 11.4M |
| `views` | integer | View count parsed to a sortable number |
| `has_media` | boolean | Whether the post carries photo, video, document, voice or sticker |
| `links` | array | Outbound links found in the post |
| `posted_at` | string | ISO timestamp the post was published |
| `scraped_at` | string | ISO timestamp of capture |

## Use it from an AI agent

The actor works as a tool in Claude, Cursor or any MCP client through Apify's MCP server:

```
https://mcp.apify.com/?tools=themineworks/telegram-channel-scraper
```

## FAQ

### Does this need a Telegram Bot API token or phone number?

No. It reads the public, server-rendered channel preview pages over plain HTTP; no Bot API, no phone verification, no MTProto session.

### How far back can I pull a channel's history?

The actor walks Telegram's own cursor-based pagination, so it can retrieve large archives, not just the latest page of posts.

### Does it work on private channels?

No, only channels with a public preview page (t.me/channelname).

### What are the main use cases?

Brand and reputation monitoring, crypto and market signal tracking from announcement channels, competitor campaign tracking, and supplying structured channel history to RAG agents and summarizers.

### How does pricing work?

Pay per result: from $1.00 per 1,000 posts on Apify's higher plans, plus a $0.005 start fee per run.

### How much does the Telegram Channel Scraper cost?

From $1.00 per 1,000 posts on Apify's higher plans, plus a $0.005 start fee per run. Failed and empty results are never charged. You can cap what a single run may spend with the maximum cost setting on Apify.

### Can I export the results to CSV or Excel?

Yes. Every run saves to an Apify dataset you can download as JSON, CSV, Excel or XML, or read through the API. The Python and Node clients in this repo also write the results to local files.

### Can I run it on a schedule?

Yes. Save your input as a task on Apify and attach a schedule, or call the API from your own cron job. Scheduled runs are billed the same way as manual ones.

## Related scrapers

* [Threads Scraper](https://themineworks.com/actors/threads-scraper/): Meta Threads data that returns actual data
* [Reddit Scraper](https://themineworks.com/actors/reddit-scraper/): Free Reddit data with full comment trees
* [LinkedIn Post Scraper](https://themineworks.com/actors/linkedin-post-search/): Search LinkedIn posts by keyword without login

Part of [The Mine Works](https://themineworks.com/): 151 pay-per-result scrapers with no login and no browser setup on your side.

## License

MIT © The Mine Works
